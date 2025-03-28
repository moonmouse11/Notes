# Bad Practice
***
## Логика в контроллерах
Контроллер принимает входящий запрос, вызывает необходимые действия (сервисы, экшены, команды) и возвращает ответ клиенту. Ничего более в нем быть не должно.
***
## Слишком тонкие контроллеры
``` php
/* Bad */
public function store(Request $request, PostService $postService)
{
	return $postService->create($request);
}
```
Здесь явное нарушение `SRP`: `PostService` зависит от `Request` и отвечает за `Response`, В результате такой сервис нельзя нигде переиспользовать, а смысл контроллера сводится к нулю.
***
## DI в конструкторе контроллера
Следующий пример - результат наличия логики в контроллере и, как следствие, попытка "разгрузить" методы. Требовать зависимости необходимо в конструкторе **сервиса, а не контроллера**.
``` php
/* Bad */

public function __construct(MyService $myService)
{
	$this->myService = $myService;
}

public function store() 
{
	$this->myService->handle(); 
}

public function update()
{
	$this->myService->handle();
}
```
Что в этом коде такого, спросишь ты?
1. **Он бесполезен.** Визуально кажется, что сервис используется в нескольких методах и логично инициализацию вынести в одно место, но публичные методы контроллера не должны вызывать друг друга. Следовательно сервис используется только в одном методе на один запрос и больше не будет вызван в рамках жизненного цикла для текущего контроллера, поэтому использование $this->service в контроллере обычно не имеет смысла. Исключением могут быть приватные методы контроллера, но размещать логику в таких методах может быть не самым удачным решением по сравнению с сервисными классами.
2. Не всем методам может понадобиться один и тот же сервис, зачем его загружать для всех запросов? Незачем.
3. И самое важное, в жизненном цикле **конструктор контроллера вызывается до `middlewares`**. Согласно документации, в конструкторе контроллера можно указывать `middleware` (для порядка и удобства рекомендую указывать их только в роутах), а значит сам контроллер создаётся до этапа выполнения "мидлварок" и в момент внедрения зависимостей ещё будут недоступны некоторые функции, например, сессия и куки, а значит отсутствовать и авторизация. И если конструктор сервиса зависит от подобных вещей, то работать правильно он не сможет, а отладка бага может занять некоторое время.
Поэтому запрашивай необходимые сервисы в методах контроллера:
```php
/* Good */

public function store(MyService $myService)
{
	$myService->handle();
}

public function update(MyService $myService)
{
	$myService->handle();
}
```
Последний пример не является нарушением `DRY`. Даже если в разных методах сервис используется одинаково, то это всё равно не повод выносить куда-либо, ибо "экшены" контроллера независимы друг от друга и в любой момент может понадобиться внести изменения в один из них. И если методы получились полностью одинаковыми, то всё равно вызов логики - это не сама логика.
***
## Логика в шаблонах
Можно подготовить заранее - вынеси и подготовь заранее. В шаблоне должна остаться только логика отвечающая за само отображение. Можно вызывать сервисы и хелперы, обращаться к константам, но **никаких запросов к БД из шаблонов быть не должно**.
``` php
/* Bad */

@foreach (Product::get() as $product)
  @php($total += $product->price * $product->qty)

  <img src="{{ Storage::disk('products')->url($product->cover) }}" />

  {{ new \Carbon\Carbon($product->created_at)->format('Y-m-d') }}
@endforeach

Total price: {{ $total }}
```
Кейсы бывают разные и порой сложно полностью отказаться от использования php и определения новых переменных, но всегда старайся свести к минимуму, особенно избавиться от каких-то вычислений не связанных с вёрсткой. В примере выше, подсчитывается общая цена при переборе товаров. Это ужаснейшая идея реализовывать подобную логику в шаблоне. Либо считай заранее в сервисе/контроллере, либо вызывай сервис из шаблона, который подсчитает, но не пиши прямо в блейде.
Представь, что тебе надо будет изменить формулу ценообразования и учитывать текущие акции, код может вырасти на десятки строк, а у тебя 5 разных шаблонов, где отображается товар, в каждый будешь копипастить?
**Чем чище html, тем лучше.**
``` php
/* Good */

@foreach ($products as $product)
  <img src="{{ $product->cover_url }}" />

  {{ $product->created_at->format('Y-m-d') }}
@endforeach

Total price: {{ $totalPrice }}
```
Также не стоит забывать, что шаблон может быть переиспользован на странице несколько раз и если в нем присутствует тяжёлая логика, например, какой-то цикл с вызовом сервиса, то каждое использование шаблона будет повторно запускать этот код, поэтому крайне рекомендуется передавать в шаблон максимально подготовленные данные, на сколько позволяет ситуация, конечно же.
***
## Логика в маршрутах (routes)
В документации логика в роутах используется для наглядности примеров, не более.
В роутах можно что-то протестировать быстро для себя, но потом обязательно удалить или перенести в контроллер, хуже места для написания любой логики придумать сложно.
``` php
/* Bad */

Route::get('posts', static function () {
    $posts = Post::active()->paginate(config('post.per_page'));

    return view('post.index', compact('posts'));
});
```
Даже что-то крайне простое (возврат шаблонов, редиректы) лучше вынести в контроллер или использовать готовые для этого методы.
``` php
/* Good */

Route::get('posts', [PostController::class, 'index']);

Route::view('posts', 'posts.index');

Route::redirect('old-posts', 'posts');
```
Стоит отметить, что на практике использование `Route::view()` неоправданно, т.к. это вносит двойственность (где-то шаблоны возвращаются роутами, а где-то контроллерами) и чтобы добавить какую-либо логику, необходимо всё равно создавать контроллер.
***
## Валидация в контроллере
Сама по себе валидация в контроллере не критична, т.к. контроллер отвечает за обработку запроса и ответ в случае какой-либо ошибки. Но это загрязняет методы контроллера и всё же нарушает `SRP`, тем более в Laravel есть готовый механизм для таких задач.
``` php
/* Bad */

public function store(Request $request)
{
    $data = $request->validate([
      'heading' => ['required', 'string', 'max:255'],
      'content' => ['nullable', 'string'],
      'status' => ['required', 'boolean'],
    ]);

    Post::create($data);
}
```
Вместо валидации в контроллерах используй `FormRequest`.
``` php
/* Good */

public function store(PostCreateRequest $request)
{
    Post::create($request->validated());
}

public function update(PostUpdateRequest $request, $id)
{
    Post::where('id', $id)->update($request->validated());
}
```
- для каждого запроса используется свой `FormRequest`. Не стоит делать общий `PostRequest`, даже если правила в конечном итоге получились одинаковыми.
- Не используй в контроллере ручное создание валидатора `Validator::make()` без явной на то необходимости. В случае возникновения ошибки валидации, фреймворк сам вернёт ошибку и завершит выполнение. Иначе при использовании `Validator::make()` необходимо также вручную сформировать ответ.
***
## Запросы в циклах
Код ниже сгенерирует по 2 запроса на каждую итерацию. Если в `$ids` 100 элементов, значит запросов будет 200, если 500, то 1000...
``` php
/* Bad */

foreach ($ids as $id) {
    $post = Post::first('id', $id);
    $post->update(['status' => 1]);
}
```
Есть редкие кейсы, когда выгоднее или проще создать много запросов, чем писать несколько, но сложных, особенно если код будет выполняться в фоне, не важно сколько времени это может занять и какую нагрузку дать на сервер.
**Но в подавляющем большинстве случаев** необходимо избегать использования запросов к БД из циклов.
``` php
/* Good */
Post::whereIn($ids)->update(['status' => 1]);
```
***
## Ленивая загрузка связей
**Сколько запросов к БД происходит в примере ниже?**
``` php
/* Bad */

public function index()
{
    $posts = Post::active()->paginate(10);

    return view('posts.index', compact('posts'));
}


@foreach ($posts as $post)
  {{ $post->heading }}

  @foreach($post->comments as $comment)
    {{ $comment->message }}
    {{ $comment->user->name }}
  @endforeach
@endforeach
```
Мы выбираем 10 постов из БД одним запросом и передаём коллекцию в шаблон. В шаблоне мы перебираем посты и обращаемся на связь `$post->comments`. Каждое такое обращение это +1 запрос к таблице comments, в котором подгружаются комментарии к посту. Значит на 10 постов +10 запросов, итого уже 11. Далее происходит перебор комментариев (мы условились что их 5 у каждого поста) и у каждого комментария есть обращение на связь `$comment->user`, чтобы получить автора комментария, т.е. ещё +1 запрос на каждый комментарий, а значит `10*5 = +50` запросов. Итого имеем 61 запрос к БД.
Эта называется **«проблема N+1».** В реальности постов и комментариев на одной странице может быть больше и количество запросов легко переходит в сотни (например 10 постов по 30 комментариев в итоге даст 311 запросов).
Чтобы исправить данную проблему существует понятие `Eager Loading` или `Жадная загрузка`, смысл которой заранее получить все необходимые данные для отображения, а не делать запросы из циклов по требованию (ленивая загрузка). В Laravel это предусмотрено.
Исправим наш запрос в контроллере с помощью `with()`:
``` php
/* Good */

public function index()
{
    $posts = Post::with('comments.user')->active()->paginate(10);

    return view('posts.index', compact('posts'));
}
```
По итогу получаем всего 3 запроса к БД вместо 61: посты, комментарии и авторы комментариев.
- Начиная с 8-ой версии появилась возможность отключить ленивую загрузку, чтобы случайно не обратиться к связям, которые не были загружены заранее.
- Laravel сам загрузит все необходимые связи указанные в цепочке. Нет необходимости указывать отдельно: `with('comments', 'comments.user')`, хоть это и не будет ошибкой.
***
## Получение текущего пользователя
Популярнейшая проблема встречается в коде начинающих:
``` php
/* Bad */

User::find(Auth::user()->id);
// or
User::find(Auth::id());
```
В примере выше происходит бесполезная выборка из БД. `Auth::user()` уже возвращает модель авторизированного пользователя. Нет смысла вытаскивать данные ещё раз через `User::find()`.
``` php
/* Good */

$user = Auth::user();
$user->email;

$userId = Auth::id();
```
- Если пользователь не авторизирован, то `Auth::user()` вернёт `null`.
- Фасад `Auth` и хелпер `auth()` эквивалентны.
***
## Обновление данных
Чтобы обновить данные, часто можно встретить такой код:
``` php 
/* Bad */

$post = Post::find($id);

$post->update($data);
// or
$post->fill($data);
$post->save();
```
В данном примере нет проблем, если получаемые данные нужны для каких-либо действий, например, для ответа клиенту.
Если же задача просто обновить данные в БД, то `Post::find()` не нужен, т.к. это лишний `SELECT` запрос. Достаточно выполнить `UPDATE` запрос напрямую, без выборки:
``` php
/* Good */

Post::where('id', $id)->update($data);
```
Следует различать методы модели и билдера:
- `Illuminate/Database/Eloquent/Model::update`
- `Illuminate/Database/Eloquent/Builder::update`
В первом примере используется метод модели, во втором - билдера. Оба варианта правильные, но второй предпочтителен, если нужно только обновить данные. Стоит отметить, что события, при таком обновлении, не будут вызваны.
***
## Mass assignment
Немного спорный вопрос, балансирующий между "краткостью" и "контролем".
В первом примере заполнение полей происходит вручную и код выглядит громоздко.
``` php
$post = new Post();
$post->heading = $request->get('heading');
$post->description = $request->get('description');
$post->content = $request->get('content');
$post->save();
```
Во втором примере используется массовое заполнение и код выглядит компактно.
``` php
$post = Post::create($request->validated());
```
На практике второй вариант обычно предпочтителен, т.к. имеет лаконичный формат и снижает риск опечаток в именах полей. Требует указания `$fillable` в модели.
С другой стороны, первый вариант, более явный, сразу видно какие поля сохраняются и удобно добавить новое, но необходимо вручную перечислять все поля.
Можно использовать оба варианта вместе:
``` php
$post = new Post();
$post->fill($request->validated());
$post->user_id = Auth::id();
$post->active = 1;
$post->save();
```
***
## Тяжелые выборки
В первом примере код выбирает все записи из БД. Это хорошо работает пока в таблице пару десятков или даже сотен записей.
``` php
/* Bad */

Post::all();
//or
Post::get();
```
Но когда количество записей в таблице заметно возрастает, то начинаются проблемы с производительностью и такие запросы долго выполняются и потребляют большое количество памяти.
Поэтому рекомендуется не использовать метод `all()`, а ограничивать количество записей, например, с помощью пагинации или явно указав лимит.
``` php
/* Good */

Post::paginate(10);
// or
Post::limit(100)->get();
```
Иногда действительно необходимо получить все записи из небольшой таблицы, например, цвета товара или статусы заказа. Даже в таком случае лучше перестраховаться и ограничить выборку.
Другой вариант используется для выборки всех записей из большой таблицы, например, нам необходимо обработать каждую запись или экспортировать в файл. Для этого необходимо использовать метод `chunk()`, который будет порционно доставать записи, снижая нагрузку на БД и экономя память.
``` php
/* Good */

Post::chunk(100, static function ($posts) {
  // Process the records
  // $posts->each(...);
});
```
***
## Делегирование выборки на коллекции
Популярная ошибка у новичков, которые не задумываются как работает их код, а смотрят лишь на результат. Оба примера ниже дают одинаковый результат, но работают с разной эффективностью.
``` php
/* Bad */

Post::get()->where('user_id', Auth::id());
// or
Post::get()->sortBy('created_at');
// or
Post::get()->first();
```
В первом случае, всё что после методов `get()` или `all()` является коллекцией моделей. Запрос к БД уже выполнен. Т.е. выбираются все записи из таблицы и затем на уровне php фильтруется/сортируется коллекция.
Правильный вариант - выбирать запросом **только необходимые записи** и сортировать их на уровне БД.
``` php
/* Good */

Post::where('user_id', Auth::id())->get();
// or
Post::orderBy('created_at')->get();
// or
Post::first();
```
Всё, что можно сделать на уровне БД, то лучше делать там, т.к. СУБД имеет оптимизированные алгоритмы для таких задач. Редкие кейсы со сложными сортировками и группировками, которые не позволяет сделать СУБД, можно выполнять на уровне коллекций или массивов в php.
***
## Хардкод
Избегай использования жёстко заданных значений прямо в коде.
``` php
/* Bad */

public function update(Post $post)
{
    $post->status = 'updated';

    Mail::to('admin@localhost')->send();

    return redirect()->withSuccess('Post successfully updated');
}
```
Выноси значения в конфиги, переводы, константы и базу данных.
``` php
/* Good */

public function update(Post $post)
{
    $post->status = Post::STATUS_UPDATED;

    Mail::to(config('mail.to.admin'))->send();

    return redirect()->withSuccess(__('messages.post_updated'));
}
```
***
## Использование `env()`
Использовать хелпер `env()` следует только в файлах конфигурации.
Это связано с тем, что после кэширования конфига, `env()` не будет возвращать значения из файла `.env`
Об этом говорится в документации, но к сожалению, часто упускается новичками.
Получать значения необходимо только через `config()`.
***
## Высокая связанность / High coupling
**«Качественный дизайн должен обладать слабой связанностью (low coupling) и сильной связностью (high cohesion)»**
``` php
/* Bad */

public function index()
{
    $myService = new MyService();
    $myService->handle();
}
```
Проблема данного кода, что он имеет высокую связанность, т.к. в нем создаётся новый объект. Ситуация усугубляется ещё больше, когда данный сервис начинает требовать какие-либо зависимости, а те зависимости, в свою очередь, требуют свои зависимости и так далее...
Также такой код будет затруднительно тестировать, т.к. сервис нельзя "замокать".
Вместо прямого создания объекта необходимо использовать `DI`, реализация которого уже есть в Laravel:
``` php
/* Good */

public function index(MyService $myService)
{
    $myService->handle();
}
```
И если следовать букве `D` из `«SOLID»`, то в данном примере `MyService` должен быть абстракцией, а не конкретным классом (реализацией).
***
## Дублирование частей `QueryBuilder`
Часто необходимо выбирать данные из БД по одинаковым условиям. Чтобы избежать дублирования кода, используются `Scopes`.
``` php
/* Bad */

public function index()
{
    Post::where('active', 1)->get();
}

public function show()
{
    Post::where('active', 1)->first();
}
```
Кроме дублирования, скоупы повышают читаемость запроса (если правильно именованы).
``` php
/* Good */

public function index()
{
    Post::active()->get();
}

public function show()
{
    Post::active()->first();
}

// App\Models\Post
public function scopeActive(Builder $query)
{
    $query->where('active', 1);
}
```
Также можно использовать глобальные скоупы, которые будут применяться ко всем запросам автоматически. Это может быть удобно, чтобы гарантировать выборку только активных записей (по такому принципу работает `soft delete`). Но рекомендую прежде подумать, что будет выгоднее для твоего приложения, прописывать везде скоупы или `withoutGlobalScope()`. Также глобальные скоупы снижают очевидность запроса, т.е. сложно, посмотрев на QB, сказать какие записи выбираются без проверки глобальных скоупов и этот момент придётся держать в памяти.
***
## Хранение файлов в public
По непонятным причинам многие новички пишут пользовательские файлы в `public` директорию. 
Согласно документации, для хранения файлов есть директории `/storage/app` для приватных файлов и `storage/app/public` для общедоступных.
Технической проблемы "хранения в `public`" нет, кроме того факта, что все файлы будут всегда доступны напрямую, и нельзя программно контролировать их доступ, например, по временной ссылке. Также это не интуитивный момент для других разработчиков, они, как и сторонние пакеты, будут использовать `storage/app` и в проекте появится двойственность, одни файлы будут в `storage`, другие в `public`.
В Laravel есть мощная система по работе с файлами, с конфигурацией на `storage/app` и которую легко можно изменить под собственные нужды, вплоть до написания собственного драйвера.
Ещё одна популярная ошибка это использование хелпера `storage_path()` вместо фасада `Storage`.
***
## Нативная работа с датами
По умолчанию, в Laravel используется мощный пакет для работы с датами `Carbon`, который наследуется от нативного `DateTime`.
Поля модели такие как `created_at`, `updated_at`, `deleted_at` и другие, которые имеют каст, содержат не строку, а объект `\Carbon\Carbon`, поэтому нет смысла и необходимости повторно оборачивать в `\DateTime`.
``` php
/* Bad */

$date = new \DateTime($post->created_at);
$date->add(new \DateInterval('P1D'));
$date->format('Y-m-d');
```
Код выше работает, потому что `$post->created_at` имеет метод `toString()`. В результате объект конвертируется в строку, чтобы создать новый объект, но уже нативный, а не `Carbon`.
Вместо этого сразу можно работать с датой как с объектом:
``` php
/* Good */

$date = $post->created_at->addDay();
$date->format('Y-m-d');
```
***
## Настройка `document_root`
Чаще всего проблема вызвана попыткой завести лару на виртуальном хостинге (shared hosting), который не позволяет управлять корневой директорией web-сервера.
В результате начинающие перемещают `index.php` в корень фреймворка и/или размещают `.htaccess` в котором настраивают перенаправление в `public`.
``` conf
/* Bad */

# .htaccess

2RewriteRule ^$ public/index.php [L]

3RewriteRule ^(.*)$ public/$1 [L

4# or any variants
```
Это серьёзная проблема безопасности, а в случае с Apache ещё страдает и производительность. Приложение должно находиться за пределами публичной директории, а web-сервер "смотреть" в `public`.
``` conf
/* Good */

# Nginx
server {
  root /srv/example.com/public;
}

# Apache
<VirtualHost>
  DocumentRoot /srv/example.com/public
</VirtualHost>
```
Если нет возможности изменить document_root, то читай [«Как установить Laravel на хостинг»](https://laraway.github.io/shared-hosting/).
***
## Именования
Данная тема не относится к самому фреймворку и вроде всё понятно и нечего обсуждать. Но.
``` php
/* Bad */

$post = Post::get(); // Collection

$model = Post::first(); // Model

$user = Auth::id(); // integer

$data = $user->getPathArray(); // array
```
Принцип простой - названия переменных, методов, классов и т.п. должны отражать содержимое или предназначение. Обрати внимание, что **"содержимое" это не тип данных**.
Если переменная содержит много элементов (массив, коллекция), то имя во множественном числе, иначе в единственном.
``` php
/* Good */

$posts = Post::get(); // Collection

$post = Post::first(); // Model

$userId = Auth::id(); // integer

$userPhotos = $user->getPhotos(); // array
```
***
