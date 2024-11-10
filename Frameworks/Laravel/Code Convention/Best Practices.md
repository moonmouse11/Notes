# Best Practices
***
## Принцип единственной ответственности (Single responsibility principle)
Каждый класс должен иметь только одну обязанность.
**Плохо:**
```php
public function update(Request $request): string
{
    $validated = $request->validate([
        'title' => 'required|max:255',
        'events' => 'required|array:date,type'
    ]);

    foreach ($request->events as $event) {
        $date = $this->carbon->parse($event['date'])->toString();

        $this->logger->log('Update event ' . $date . ' :: ' . $);
    }

    $this->event->updateGeneralEvent($request->validated());

    return back();
}
```
**Хорошо:**
```php
public function update(UpdateRequest $request): string
{
    $this->logService->logEvents($request->events);

    $this->event->updateGeneralEvent($request->validated());

    return back();
}
```
***
## Методы должны делать что-то одно
Функция должна делать что-то одно и делать это хорошо.
**Плохо:**
```php
public function getFullNameAttribute(): string
{
    if (auth()->user() && auth()->user()->hasRole('client') && auth()->user()->isVerified()) {
        return 'Mr. ' . $this->first_name . ' ' . $this->middle_name . ' ' . $this->last_name;
    } else {
        return $this->first_name[0] . '. ' . $this->last_name;
    }
}
```
**Хорошо:**
```php
public function getFullNameAttribute(): string
{
    return $this->isVerifiedClient() ? $this->getFullNameLong() : $this->getFullNameShort();
}

public function isVerifiedClient(): bool
{
    return auth()->user() && auth()->user()->hasRole('client') && auth()->user()->isVerified();
}

public function getFullNameLong(): string
{
    return 'Mr. ' . $this->first_name . ' ' . $this->middle_name . ' ' . $this->last_name;
}

public function getFullNameShort(): string
{
    return $this->first_name[0] . '. ' . $this->last_name;
}
```
***
## Тонкие контроллеры, толстые модели
Выносите работу с данными в модели.
**Плохо:**
```php
public function index()
{
    $clients = Client::verified()
        ->with(['orders' => function ($q) {
            $q->where('created_at', '>', Carbon::today()->subWeek());
        }])
        ->get();

    return view('index', ['clients' => $clients]);
}
```
**Хорошо:**
```php
public function index()
{
    return view('index', ['clients' => $this->client->getWithNewOrders()]);
}

class Client extends Model
{
    public function getWithNewOrders(): Collection
    {
        return $this->verified()
            ->with(['orders' => function ($q) {
                $q->where('created_at', '>', Carbon::today()->subWeek());
            }])
            ->get();
    }
}
```
***
## Валидация
Следуя принципам тонкого контроллера и `SRP`, выносите валидацию из контроллера в `Request` классы.
**Плохо:**
```php
public function store(Request $request)
{
    $request->validate([
        'title' => 'required|unique:posts|max:255',
        'body' => 'required',
        'publish_at' => 'nullable|date',
    ]);

    ...
}
```
**Хорошо:**
```php
public function store(PostRequest $request)
{
    ...
}

class PostRequest extends Request
{
    public function rules(): array
    {
        return [
            'title' => 'required|unique:posts|max:255',
            'body' => 'required',
            'publish_at' => 'nullable|date',
        ];
    }
}
```
***
## Бизнес логика в сервис-классах
Контроллер должен выполнять только свои прямые обязанности, поэтому выносите всю бизнес логику в отдельные классы и сервис классы.
**Плохо:**
```php
public function store(Request $request)
{
    if ($request->hasFile('image')) {
        $request->file('image')->move(public_path('images') . 'temp');
    }
    
    ...
}
```
**Хорошо:**
```php
public function store(Request $request)
{
    $this->articleService->handleUploadedImage($request->file('image'));

    ...
}

class ArticleService
{
    public function handleUploadedImage($image): void
    {
        if (!is_null($image)) {
            $image->move(public_path('images') . 'temp');
        }
    }
}
```
***
## Не повторяйся (DRY)
Этот принцип призывает вас переиспользовать код везде, где это возможно. Если вы следуете принципу `SRP`, вы уже избегаете повторений, но Laravel позволяет вам также переиспользовать представления, части Eloquent запросов и т.д.
**Плохо:**
```php
public function getActive()
{
    return $this->where('verified', 1)->whereNotNull('deleted_at')->get();
}

public function getArticles()
{
    return $this->whereHas('user', function ($q) {
            $q->where('verified', 1)->whereNotNull('deleted_at');
        })->get();
}
```
**Хорошо:**
```php
public function scopeActive($q)
{
    return $q->where('verified', true)->whereNotNull('deleted_at');
}

public function getActive(): Collection
{
    return $this->active()->get();
}

public function getArticles(): Collection
{
    return $this->whereHas('user', function ($q) {
            $q->active();
        })->get();
}
```
***
## Предпочитайте Eloquent конструктору запросов (query builder) и сырым запросам в БД. Предпочитайте работу с коллекциями работе с массивами
Eloquent позволяет писать максимально читаемый код, а изменять функционал приложения несоизмеримо легче. У Eloquent также есть ряд удобных и мощных инструментов. 
**Плохо:**
```php
SELECT *
FROM `articles`
WHERE EXISTS (SELECT *
              FROM `users`
              WHERE `articles`.`user_id` = `users`.`id`
              AND EXISTS (SELECT *
                          FROM `profiles`
                          WHERE `profiles`.`user_id` = `users`.`id`) 
              AND `users`.`deleted_at` IS NULL)
AND `verified` = '1'
AND `active` = '1'
ORDER BY `created_at` DESC
```
**Хорошо:**
```php
Article::has('user.profile')->verified()->latest()->get();
```
***
## Используйте массовое заполнение (mass assignment)
**Плохо:**
```php
$article = new Article;
$article->title = $request->title;
$article->content = $request->content;
$article->verified = $request->verified;

// Привязать статью к категории.
$article->category_id = $category->id;
$article->save();
```
**Хорошо:**
```php
$category->article()->create($request->validated());
```
***
## Не выполняйте запросы в представлениях и используйте нетерпеливую загрузку (проблема N + 1)
**Плохо (будет выполнен 101 запрос в БД для 100 пользователей):**
```php
@foreach (User::all() as $user)
    {{ $user->profile->name }}
@endforeach
```
**Хорошо (будет выполнено 2 запроса в БД для 100 пользователей):**
```php
$users = User::with('profile')->get();

@foreach ($users as $user)
    {{ $user->profile->name }}
@endforeach
```
***
## Используйте метод chunk при работе с большим количеством данных
**Плохо:**
```php
$users = $this->get();

foreach ($users as $user) {
    ...
}
```
**Хорошо:**
```php
$this->chunk(500, function ($users) {
    foreach ($users as $user) {
        ...
    }
});
```
***
## Предпочитайте читаемые имена переменных и методов комментариям
**Плохо:**
```php
// Determine if there are any joins
if (count((array) $builder->getQuery()->joins) > 0)
```
**Хорошо:**
```php
if ($this->hasJoins())
```
***
## Выносите JS и CSS из шаблонов Blade и HTML из PHP кода
**Плохо:**
```js
let article = `{{ json_encode($article) }}`;
```
**Лучше:**
```html
<input id="article" type="hidden" value='@json($article)'>
Или
<button class="js-fav-article" data-article='@json($article)'>{{ $article->name }}<button>
```
**В Javascript файле:**
```js
let article = $('#article').val();
```
Еще лучше использовать специализированный пакет для передачи данных из бэкенда во фронтенд.
***
## Конфиги, языковые файлы и константы вместо текста в коде
Непосредственно в коде не должно быть никакого текста.
**Плохо:**
```php
public function isNormal()
{
    return $article->type === 'normal';
}

return back()->with('message', 'Ваша статья была успешно добавлена');
```
**Хорошо:**
```php
public function isNormal(): bool
{
    return $article->type === Article::TYPE_NORMAL;
}

return back()->with('message', __('app.article_added'));
```
***
## Используйте инструменты и практики принятые сообществом
Laravel имеет встроенные инструменты для решения часто встречаемых задач. Предпочитайте пользоваться ими использованию сторонних пакетов и инструментов. Laravel разработчику, пришедшему в проект после вас, придется изучать и работать с новым для него инструментом, со всеми вытекающими последствиями. Получить помощь от сообщества будет также гораздо труднее. Не заставляйте клиента или работодателя платить за ваши велосипеды.

| Задача                           | Стандартные инструмент                | Нестандартные инструмент                            |
| -------------------------------- | ------------------------------------- | --------------------------------------------------- |
| Авторизация                      | Политики                              | Entrust, Sentinel и др. пакеты, собственное решение |
| Работа с JS, CSS и пр.           | Laravel Mix, Vite                     | Grunt, Gulp, сторонние пакеты                       |
| Среда разработки                 | Laravel Sail, Homestead               | Docker                                              |
| Разворачивание приложений        | Laravel Forge                         | Deployer и многие другие                            |
| Тестирование                     | Phpunit, Mockery                      | Phpspec, Pest                                       |
| e2e тестирование                 | Laravel Dusk                          | Codeception                                         |
| Работа с БД                      | Eloquent                              | SQL, построитель запросов, Doctrine                 |
| Шаблоны                          | Blade                                 | Twig                                                |
| Работа с данными                 | Коллекции Laravel                     | Массивы                                             |
| Валидация форм                   | Request классы                        | Сторонние пакеты, валидация в контроллере           |
| Аутентификация                   | Встроенный функционал                 | Сторонние пакеты, собственное решение               |
| Аутентификация API               | Laravel Passport, Laravel Sanctum     | Сторонние пакеты, использующие JWT, OAuth           |
| Создание API                     | Встроенный функционал                 | Dingo API и другие пакеты                           |
| Работа со структурой БД          | Миграции                              | Работа с БД напрямую                                |
| Локализация                      | Встроенный функционал                 | Сторонние пакеты                                    |
| Обмен данными в реальном времени | Laravel Echo, Pusher                  | Пакеты и работа с веб сокетами напрямую             |
| Генерация тестовых данных        | Seeder классы, фабрики моделей, Faker | Ручное заполнение и пакеты                          |
| Планирование задач               | Планировщик задач Laravel             | Скрипты и сторонние пакеты                          |
| БД                               | MySQL, PostgreSQL, SQLite, SQL Server | MongoDb                                             |
***
## Соблюдайте соглашения сообщества об именовании
Следуйте [стандартам PSR](https://www.php-fig.org/psr/psr-12/) при написании кода.
Также, соблюдайте другие cоглашения об именовании:

| Что                                                                   | Правило                                                                     | Принято                                 | Не принято                                                      |
| --------------------------------------------------------------------- | --------------------------------------------------------------------------- | --------------------------------------- | --------------------------------------------------------------- |
| Контроллер                                                            | ед. ч.                                                                      | ArticleController                       | ~~ArticlesController~~                                          |
| Маршруты                                                              | мн. ч.                                                                      | articles/1                              | ~~article/1~~                                                   |
| Имена маршрутов                                                       | snake_case                                                                  | users.show_active                       | ~~users.show-active, show-active-users~~                        |
| Модель                                                                | ед. ч.                                                                      | User                                    | ~~Users~~                                                       |
| Отношения hasOne и belongsTo                                          | ед. ч.                                                                      | articleComment                          | ~~articleComments, article_comment~~                            |
| Все остальные отношения                                               | мн. ч.                                                                      | articleComments                         | ~~articleComment, article_comments~~                            |
| Таблица                                                               | мн. ч.                                                                      | article_comments                        | ~~article_comment, articleComments~~                            |
| Pivot таблица                                                         | имена моделей в алфавитном порядке в ед. ч.                                 | article_user                            | ~~user_article, articles_users~~                                |
| Столбец в таблице                                                     | snake_case без имени модели                                                 | meta_title                              | ~~MetaTitle; article_meta_title~~                               |
| Свойство модели                                                       | snake_case                                                                  | $model->created_at                      | ~~$model->createdAt~~                                           |
| Внешний ключ                                                          | имя модели ед. ч. и _id                                                     | article_id                              | ~~ArticleId, id_article, articles_id~~                          |
| Первичный ключ                                                        | -                                                                           | id                                      | ~~custom_id~~                                                   |
| Миграция                                                              | -                                                                           | 2017_01_01_000000_create_articles_table | ~~2017_01_01_000000_articles~~                                  |
| Метод                                                                 | camelCase                                                                   | getAll                                  | ~~get_all~~                                                     |
| Метод в контроллере ресурсов                                          | [таблица](https://laravel.com/docs/master/controllers#resource-controllers) | store                                   | ~~saveArticle~~                                                 |
| Метод в тесте                                                         | camelCase                                                                   | testGuestCannotSeeArticle               | ~~test_guest_cannot_see_article~~                               |
| Переменные                                                            | camelCase                                                                   | $articlesWithAuthor                     | ~~$articles_with_author~~                                       |
| Коллекция                                                             | описательное, мн. ч.                                                        | $activeUsers = User::active()->get()    | ~~$active, $data~~                                              |
| Объект                                                                | описательное, ед. ч.                                                        | $activeUser = User::active()->first()   | ~~$users, $obj~~                                                |
| Индексы в конфиге и языковых файлах                                   | snake_case                                                                  | articles_enabled                        | ~~ArticlesEnabled; articles-enabled~~                           |
| Представление                                                         | kebab-case                                                                  | show-filtered.blade.php                 | ~~showFiltered.blade.php, show_filtered.blade.php~~             |
| Конфигурационный файл                                                 | snake_case                                                                  | google_calendar.php                     | ~~googleCalendar.php, google-calendar.php~~                     |
| Контракт (интерфейс)                                                  | прилагательное или существительное                                          | AuthenticationInterface                 | ~~Authenticatable, IAuthentication~~                            |
| Трейт                                                                 | прилагательное                                                              | Notifiable                              | ~~NotificationTrait~~                                           |
| Trait [(PSR)](https://www.php-fig.org/bylaws/psr-naming-conventions/) | adjective                                                                   | NotifiableTrait                         | ~~Notification~~                                                |
| Enum                                                                  | единственное число                                                          | UserType                                | ~~UserTypes~~, ~~UserTypeEnum~~                                 |
| FormRequest                                                           | singular                                                                    | UpdateUserRequest                       | ~~UpdateUserFormRequest~~, ~~UserFormRequest~~, ~~UserRequest~~ |
| Seeder                                                                | singular                                                                    | UserSeeder                              | ~~UsersSeeder~~                                                 |
***
## Приоритет соглашений над конфигурацией
Пока вы следуете принятым соглашениям, вам не нужно добавлять в код дополнительную конфигурацию.
**Плохо:**
```php
// Название таблицы 'Customer'
// Первичный ключ 'customer_id'
class Customer extends Model
{
    const CREATED_AT = 'created_at';
    const UPDATED_AT = 'updated_at';

    protected $table = 'Customer';
    protected $primaryKey = 'customer_id';

    public function roles(): BelongsToMany
    {
        return $this->belongsToMany(Role::class, 'role_customer', 'customer_id', 'role_id');
    }
}
```
**Хорошо:**
```php
// Название таблицы 'customers'
// Первичный ключ 'id'
class Customer extends Model
{
    public function roles(): BelongsToMany
    {
        return $this->belongsToMany(Role::class);
    }
}
```
***
## Короткий и читаемый синтаксис там, где это возможно
**Плохо:**
```php
$request->session()->get('cart');
$request->input('name');
```
**Хорошо:**
```php
session('cart');
$request->name;
```
**Еще примеры:**

|Часто используемый синтаксис|Более короткий и читаемый синтаксис|
|---|---|
|`Session::get('cart')`|`session('cart')`|
|`$request->session()->get('cart')`|`session('cart')`|
|`Session::put('cart', $data)`|`session(['cart' => $data])`|
|`$request->input('name'), Request::get('name')`|`$request->name, request('name')`|
|`return Redirect::back()`|`return back()`|
|`is_null($object->relation) ? null : $object->relation->id`|`optional($object->relation)->id`|
|`return view('index')->with('title', $title)->with('client', $client)`|`return view('index', compact('title', 'client'))`|
|`$request->has('value') ? $request->value : 'default';`|`$request->get('value', 'default')`|
|`Carbon::now(), Carbon::today()`|`now(), today()`|
|`App::make('Class')`|`app('Class')`|
|`->where('column', '=', 1)`|`->where('column', 1)`|
|`->orderBy('created_at', 'desc')`|`->latest()`|
|`->orderBy('age', 'desc')`|`->latest('age')`|
|`->orderBy('created_at', 'asc')`|`->oldest()`|
|`->select('id', 'name')->get()`|`->get(['id', 'name'])`|
|`->first()->name`|`->value('name')`|
***
## Используйте IoC или фасады вместо new Class
Внедрение классов через синтаксис new Class создает сильное сопряжение между частями приложения и усложняет тестирование. Используйте контейнер или фасады.
**Плохо:**
```php
$user = new User;
$user->create($request->validated());
```
**Хорошо:**
```php
public function __construct(User $user)
{
    $this->user = $user;
}

...

$this->user->create($request->validated());
```
***
## Не работайте с данными из файла `.env` напрямую
Передайте данные из `.env` файла в кофигурационный файл и используйте `config()` в приложении, чтобы использовать эти данными.
**Плохо:**
```php
$apiKey = env('API_KEY');
```
**Хорошо:**
```php
// config/api.php
'key' => env('API_KEY'),

// Используйте данные в приложении
$apiKey = config('api.key');
```
***
## Храните даты в стандартном формате. Используйте читатели и преобразователи для преобразования формата
**Плохо:**
```php
{{ Carbon::createFromFormat('Y-d-m H-i', $object->ordered_at)->toDateString() }}
{{ Carbon::createFromFormat('Y-d-m H-i', $object->ordered_at)->format('m-d') }}
```
**Хорошо:**
```php
// Модель
protected $casts = [
    'ordered_at' => 'datetime',
];
// Читатель (accessor)
public function getSomeDateAttribute($date)
{
    return $date->format('m-d');
}

// Шаблон
{{ $object->ordered_at->toDateString() }}
{{ $object->ordered_at->some_date }}
```
***
## Не используйте DocBlock
DocBlock ухудшают читаемость кода. Вместо них используйте хорошие именя для методов и современный синтаксис PHP, например описание возвращаемых типов (return type hints).
**Плохо:**
```php
/**
 * Функция проверяет есть ли в строке символы, которые остутствуют в ASCII.
 *
 * @param string $string Строка, которую мы получаем с фронтенда и которая
 *                       может содержать символы, не входящие в ASCII.
 *                       Возвращает True, если таких символов в строке нет.
 *
 * @return bool
 * @author  Василий Иванов
 *
 * @license GPL
 */

public function checkString($string)
{
}
```
**Хорошо:**
```php
public function isValidAsciiString(string $string): bool
{
}
```
***
## Другие советы и практики
Не используйте паттерны и инструменты чужеродные по отношению к Laravel и подобным фреймворкам (RoR, Django). Если вам нравятся подходы, используемые в Symfony (Spring и др.), использовать эти фреймворки для создания веб приложений будет намного разумнее.
Не размещайте логику в маршрутах.
Старайтесь не использовать "сырой" PHP в шаблонах Blade.
Используйте базу данных, размещенную в памяти (in-memory DB) при тестировании.
Не меняйте стандартные инструменты фреймворка, иначе у вас могут возникнуть проблемы при обновлении фреймворка и другие сложности.
Используйте современный синтаксис PHP, но при этом не забывайте, что читаемость важнее.
Используйте такие инструменты, как View Composers, с большой осторожностью. В большинстве случаев, есть возможность найти другое решение проблемы.
***
