# Routing
***
## Basic Routing
Простейшие маршруты Laravel принимают `URI` и `callback`, обеспечивая нетрудоемкий и выразительный метод определения маршрутов и поведения без сложных конфигурационных файлов маршрутизации.
```php
/* Пример создания маршрута /greeting */

use Illuminate\Support\Facades\Route;

Route::get('/greeting', function () {
    return 'Hello World';
});
```
***
## The Default Route Files
Все маршруты Laravel должны быть определены в файлах маршрутов, находящихся в вашем каталоге `routes`. Эти файлы автоматически загружаются Laravel с использованием конфигурации, указанной в файле `bootstrap/app.php`.
Файл `routes/web.php` определяет маршруты для веб-интерфейса. Этим маршрутам назначается `middleware` `web`, которая обеспечивает такие функции, как состояние сессии и защита от `CSRF`.
 К маршрутам, определенным в `routes/web.php`, можно получить доступ, введя URL-адрес определенного маршрута в браузере.
 ```php
/* Пример создания маршрута http://example.com/user */

use App\Http\Controllers\UserController;

Route::get('/user', [UserController::class, 'index']);
```
***
## API Routes
- `php artisan install:api` - команда для подключения маршрутов API в Laravel.
Команда `install:api` устанавливает пакет `Laravel Sanctum`, который обеспечивает надежную, но простую защиту аутентификации токена API, которую можно использовать для аутентификации сторонних потребителей API, SPA, или мобильных приложений, и так же создает файл `routes/api.php`.
```php
/* Пример кода скрипта routes/api.php */

Route::get('/user', function (Request $request) {
    return $request->user();
})->middleware('auth:sanctum');
```
Маршруты в `routes/api.php` не сохраняют состояние и назначаются `api` группе `middleware`. Кроме того, к этим маршрутам автоматически применяется префикс URI `/api`, изменить префикс можно в файле `bootstrap/app.php`.
```php
/* Код поключения routes/api.php в bootstrap/app.php */

->withRouting(
    api: __DIR__.'/../routes/api.php',
    apiPrefix: 'api/admin',
    // ...
)
```
***
## Available Router Methods
Фасад `Route` позволяет регистрировать маршруты, отвечающие на любой HTTP-метод.
```php
/* Методы фасада Route */

Route::get($uri, $callback);
Route::post($uri, $callback);
Route::put($uri, $callback);
Route::patch($uri, $callback);
Route::delete($uri, $callback);
Route::options($uri, $callback);

Route::match(['get', 'post'], '/', function () {
    // Регистрирует маршрут с несколькими HTTP методами.
});

Route::any('/', function () {
    // Регистрирует маршрут с любым HTTP методов.
});
```
> При определении нескольких маршрутов, которые используют один и тот же `URI`, маршруты, использующие методы `get`, `post`, `put`, `patch`, `delete` и `options`, должны быть определены перед маршрутами, использующими методы `any`, `match` и `redirect`. Это гарантирует, что входящий запрос соответствует правильному маршруту.
***
## Dependency Injection
Можно объявить любые зависимости (DI), необходимые для маршрута, в сигнатуре замыкания маршрута. Объявленные зависимости будут автоматически извлечены и внедрены в замыкание с помощью Service Container Laravel.
```php
/* DI класса Request в callback мвршрута */

use Illuminate\Http\Request;

Route::get('/users', function (Request $request) {
    // Callback code.
});
```
***
## CSRF Protection
Любые HTML-формы, указывающие на маршруты `POST`, `PUT`, `PATCH` или `DELETE`, которые определены в файле маршрутов `web`, должны включать поле токена `CSRF`. В противном случае запрос будет отклонен с ошибкой.
```php
/* Пример добавления CSRF токена в форму Blade */

<form method="POST" action="/profile">
    @csrf
    ...
</form>
```
***
## Redirect Routes
- `Route::redirect` - статический метод для перенасправления на другой `URI`.
```php
/* Пример использования redirect (по умолчанию 302) */

Route::redirect('/here', '/there');

/* Пример использования redirect с добавлением кода состояния */

Route::redirect('/here', '/there', 301);

/* Метод выполняет редирект с 301 кодом по умолячнию */
Route::permanentRedirect('/here', '/there');
```
> При использовании параметров маршрута в маршрутах перенаправления, параметры `destination` и `status` зарезервированы Laravel и не могут быть использованы.
***
## View Routes
- `Route::view` - метод для возвращения `view` HTML-шаблона.
Метод `view` принимает URI в качестве первого аргумента и имя шаблона в качестве второго аргумента. Кроме того, можно указать массив данных для передачи в шаблон в качестве необязательного третьего аргумента.
```php
/* Пример использования view */

Route::view('/welcome', 'welcome');

/* Пример использования view с передачей параметров */

Route::view('/welcome', 'welcome', ['name' => 'Taylor']);
```
> При использовании параметров маршрута в маршрутах представлений, параметры `view`, `data`, `status` и `headers` зарезервированы Laravel и не могут быть использованы.
***
## Listing Your Routes
- `php artisan route:list` - выводит список всех маршрутов приложения.
- `php artisan route:list [-v|-vv]` - выводит список маршрутов с разделением на группы.
- `php artisan route:list --path=api` - выводит  список маршрутов указанной группы.
- `php artisan route:list --except-vendor` - выводит список маршрутов, скрывая маршруты `vendor`.
- `php artisan route:list --only-vendor` - выводит список только `vendor` маршрутов.
***
## Routing Customization
По умолчанию маршруты Laravel настраиваются и загружаются в файле `bootstrap/app.php`.
```php
/* Пример подключения routes/web.php в bootstrap/app.php */

use Illuminate\Foundation\Application;

return Application::configure(basePath: dirname(__DIR__))
    ->withRouting(
        web: __DIR__.'/../routes/web.php',
        commands: __DIR__.'/../routes/console.php',
        health: '/up',
    )->create();
```
Однако может потребоваться определить совершенно новый файл, содержащий подмножество других маршрутов приложения. Для этого можно предоставить замыкание `then` для метода `withRouting` и в рамках этого замыкания прописать любые дополнительные маршруты.
```php
/* Пример подключения routes/webhooks.php в callback then bootstrap/app.php */

use Illuminate\Support\Facades\Route;

->withRouting(
    web: __DIR__.'/../routes/web.php',
    commands: __DIR__.'/../routes/console.php',
    health: '/up',
    then: function () {
        Route::middleware('api')
            ->prefix('webhooks')
            ->name('webhooks.')
            ->group(base_path('routes/webhooks.php'));
    },
)
```
Так же можно получить полный контроль над регистрацией маршрута, предоставив замыкание `using` для метода `withRouting` **(При передаче этого аргумента платформа не будет регистрировать HTTP-маршруты)**.
```php
/* Пример using в bootstrap/app.php */

use Illuminate\Support\Facades\Route;

->withRouting(
    commands: __DIR__.'/../routes/console.php',
    using: function () {
        Route::middleware('api')
            ->prefix('api')
            ->group(base_path('routes/api.php'));

        Route::middleware('web')
            ->group(base_path('routes/web.php'));
    },
)
```
***
## Route Parameters
### Required Parameters
```php
/* Пример обязательного параметра id маршрута */

Route::get('/user/{id}', function (string $id) {
    return 'User '.$id;
});
```
Параметр `{id}` осслеживает идентификатор пользователя из URL-адреса.
```php
/* Пример двух обязательных параметров post и comment */

Route::get('/posts/{post}/comments/{comment}', function (string $postId, string $commentId) {
    // ...
});
```
Параметры маршрута всегда заключаются в фигурные скобки `{}` и должны состоять из буквенных символов. Подчеркивание (`_`) также допускается в именах параметров маршрута. 
Параметры маршрута будут внедрены в замыкания маршрута / контроллеры в зависимости от их порядка, т.е. имена аргументов замыкания маршрута / контроллера не имеют значения.
#### Parameters and Dependency Injection
Если у  маршрута есть зависимости, которые необходимо, чтобы Service Container Laravel автоматически внедрил в замыкание маршрута, то нужно указать эти зависимости **перед** параметрами маршрута. 
```php
/* Пример DI в параметрах маршрута */

use Illuminate\Http\Request;

Route::get('/user/{id}', function (Request $request, string $id) {
    return 'User '.$id;
});
```
### Optional Parameters
```php
/* Пример необязательного параметра name маршрута */

Route::get('/user/{name?}', function (?string $name = null) {
    return $name;
});

Route::get('/user/{name?}', function (?string $name = 'John') {
    return $name;
});
```
Для создания необязательного параметра нужно добавить `?` в конце его имени и так же присвоить соответствующей переменной маршрута значение по умолчанию.
### Regular Expression Constraints
Можно ограничить формат параметров вашего маршрута, используя метод `where`. Метод `where` принимает имя параметра и регулярное выражение, определяющее, как параметр должен быть ограничен.
```php
/* Пример использования метода where */

Route::get('/user/{name}', function (string $name) {
    // ...
})->where('name', '[A-Za-z]+');

Route::get('/user/{id}', function (string $id) {
    // ...
})->where('id', '[0-9]+');

Route::get('/user/{id}/{name}', function (string $id, string $name) {
    // ...
})->where(['id' => '[0-9]+', 'name' => '[a-z]+']);
```
Для часто используемых шаблонов регулярных выражений есть соответствующие вспомогательные методы, позволяющие быстро добавлять их к маршрутам.
```php
/* Пример использования вспомогательных методов where */

Route::get('/user/{id}/{name}', function (string $id, string $name) {
    // ...
})->whereNumber('id')->whereAlpha('name');

Route::get('/user/{name}', function (string $name) {
    // ...
})->whereAlphaNumeric('name');

Route::get('/user/{id}', function (string $id) {
    // ...
})->whereUuid('id');

Route::get('/user/{id}', function (string $id) {
    // ...
})->whereUlid('id');

Route::get('/category/{category}', function (string $category) {
    // ...
})->whereIn('category', ['movie', 'song', 'painting']);

Route::get('/category/{category}', function (string $category) {
    // ...
})->whereIn('category', CategoryEnum::cases());
```
Если входящий запрос не соответствует ограничениям, то будет возвращен `404` HTTP-ответ.
#### Global Constraints
Если требуется, чтобы параметр маршрута всегда ограничивался конкретным регулярным выражением, то можно использовать метод `pattern`. Требуется определить эти шаблоны в методе `boot` класса `App\Providers\AppServiceProvider`.
```php
/* Пример Route::pattern в методе boot AppServiceProvider */

use Illuminate\Support\Facades\Route;

/**
 * Запуск любых служб приложения.
 */
public function boot(): void
{
    Route::pattern('id', '[0-9]+');
}
```
Как только шаблон определен, он автоматически применяется ко всем маршрутам, использующим это имя параметра.
```php
Route::get('/user/{id}', function (string $id) {
    // Выполнится, только если параметр `{id}` имеет числовое значение...
});
```
#### Encoded Forward Slashes
Компонент маршрутизации Laravel позволяет всем символам, кроме обратного слеша (`/`), присутствовать в значениях параметров маршрута. Необходимо явно разрешить `/` быть частью заполнителя `{}`, используя регулярное выражение условия `where`.
```php
/* Пример where с обратным слешем? */

Route::get('/search/{search}', function (string $search) {
    return $search;
})->where('search', '.*');
```
_**Обратные слеши поддерживаются только в рамках последнего сегмента маршрута.**_
***
## Named Routes
**Именованные маршруты** позволяют легко создавать URL-адреса или перенаправления для определенных маршрутов.
Можно указать имя для маршрута, связав метод `name` с определением маршрута.
```php
/* Пример использования метода name */

Route::get('/user/profile', function () {
    // ...
})->name('profile');
```
Также указываются имена маршрутов для действий контроллера.
```php
/* Пример использования метода name для Controller Action */

Route::get(
    '/user/profile',
    [UserProfileController::class, 'show']
)->name('profile');
```
> **Имена маршрутов всегда должны быть уникальными.**
### Generating URLs to Named Routes
После присвоения имени указанному маршруту, можно использовать имя маршрута при генерации URL-адресов или перенаправлений с помощью вспомогательных глобальных функций `route` и `redirect`.
```php
// Создание URL-адреса...
$url = route('profile');

// Создание перенаправления...
return redirect()->route('profile');

return to_route('profile');
```
Если именованный маршрут определяет параметры, можно передать параметры в качестве второго аргумента функции `route`. Указанные параметры будут автоматически подставлены в сгенерированный URL в соответствующие места.
```php
/* Пример передачи параметров вторым аргументом в route */

Route::get('/user/{id}/profile', function (string $id) {
    // ...
})->name('profile');

$url = route('profile', ['id' => 1]);
```
Если передать дополнительные параметры в массиве, то эти пары ключ / значение будут автоматически добавлены в строку запроса сгенерированного URL-адреса.
```php
/* Пример передачи нескольких параметров вторым аргументом в route */

Route::get('/user/{id}/profile', function (string $id) {
    // ...
})->name('profile');

$url = route('profile', ['id' => 1, 'photos' => 'yes']);

// /user/1/profile?photos=yes
```
> Иногда требуется указать значение по умолчанию для параметров URL запроса, например, текущий язык.  Для этого подойдет метод `URL::defaults`.
#### Inspecting the Current Route
Если требуется определить, был ли текущий запрос направлен на конкретный именованный маршрут, то можно использовать метод `named` экземпляра `Route`.
```php
/* Получение инфоормации о текущем маршруте из middleware */

use Closure;
use Illuminate\Http\Request;
use Symfony\Component\HttpFoundation\Response;

/**
 * Обработка входящего запроса.
 *
 * @param  \Closure(\Illuminate\Http\Request): (\Symfony\Component\HttpFoundation\Response)  $next
 */
public function handle(Request $request, Closure $next): Response
{
    if ($request->route()->named('profile')) {
        // ...
    }

    return $next($request);
}
```
***
## Route Groups
**Группы маршрутов** позволяют совместно использовать атрибуты маршрута (например, посредники) для большого количества маршрутов без необходимости определять эти атрибуты для каждого маршрута отдельно.
Вложенные группы пытаются разумно «объединить» атрибуты со своей родительской группой. Посредники и условия `where` объединяются, а имена и префиксы добавляются. Разделители пространства имен и слеши в префиксах `URI` автоматически добавляются там, где это необходимо.
### Middleware
Чтобы назначить `middleware` всем маршрутам в группе, можно использовать метод `middleware` перед определением группы (посредники(`middleware`) будут выполняться в том порядке, в котором они перечислены в массиве).
```php
/* Пример группировки middleware */

Route::middleware(['first', 'second'])->group(function () {
    Route::get('/', function () {
        // Использует посредники `first` и `second`...
    });

    Route::get('/user/profile', function () {
        // Использует посредники `first` и `second`...
    });
});
```
### Controllers
Если группа маршрутов использует один и тот же контроллер - подойдет метод `controller` для определения общего контроллера всех маршрутов в группе. Затем при определении маршрутов необходимо будет указать только метод вызываемого контроллера.
```php
/* Пример группировки controller */

use App\Http\Controllers\OrderController;

Route::controller(OrderController::class)->group(function () {
    Route::get('/orders/{id}', 'show');
    Route::post('/orders', 'store');
});
```
### Subdomain Routing
Группы маршрутов также могут использоваться для управления маршрутизацией поддоменов (sub-domain). Поддоменам могут быть назначены параметры маршрута так же, как и `URI` маршрута, что позволяет отследить сегмент с поддоменом для использования его маршруте или контроллере. Поддомен можно указать, вызвав метод `domain` перед определением группы.
```php
/* Пример группировки domain с поддоменом */

Route::domain('{account}.example.com')->group(function () {
    Route::get('/user/{id}', function (string $account, string $id) {
        // ...
    });
});
```
> **Чтобы обеспечить доступность маршрутов поддоменов, требуется зарегистрировать маршруты поддоменов перед регистрацией маршрутов корневого домена. Это предотвратит перезапись маршрутами корневого домена маршрутов поддоменов, имеющих одинаковый путь `URI`.**
### Route Prefixes
Метод `prefix` используется для подстановки указанного `URI` в качестве префикса каждому маршруту в группе.
```php
/* Пример prefix admin в маршрутах */

Route::prefix('admin')->group(function () {
    Route::get('/users', function () {
        // Соответствует URL-адресу `/admin/users`
    });
});
```
### Route Name Prefixes
Метод `name` может быть использован для добавления префикса к каждому имени маршрута в группе с использованием заданной строки. Заданная строка добавляется к имени маршрута точно так, как она указана (поэтому обязательно ставить знак `.` в конце префикса).
```php
/* Пример префикса имени маршрута */

Route::name('admin.')->group(function () {
    Route::get('/users', function () {
        // Маршруту присвоено имя `admin.users`...
    })->name('users');
});
```
***
## Route Model Binding
При внедрении идентификатора модели в маршрут или действие контроллера - идет запрос в базу данных, чтобы получить модель с соответствующим идентификатором.
Привязка модели к маршруту Laravel обеспечивает удобный способ автоматического внедрения экземпляров модели непосредственно маршруты.
### Implicit Binding
Laravel автоматически извлечет модели `Eloquent`, определенные в маршрутах или действиях контроллера, чьи имена переменных объявленного типа соответствуют имени сегмента маршрута.
```php
/* Пример неявной привазки с моделью User */

use App\Models\User;

Route::get('/users/{user}', function (User $user) {
    return $user->email;
});
```
Так как переменная `$user` типизирована как модель `App\Models\User` Eloquent и имя переменной соответствует сегменту `{user}` `URI`, то Laravel автоматически внедрит экземпляр модели с идентификатором, совпадающим со значением `URI` из запроса. Если соответствующий экземпляр модели не найден в базе данных, то автоматически будет сгенерирован `404` HTTP-ответ.
Неявная привязка также возможна при использовании методов контроллера. Сегмент `{user}` `URI`  должен соответсвовать переменной `$user` в контроллере, которая типизирована как `App\Models\User`.
```php
/* Пример неявной привазки с моделью User в контроллере */

use App\Http\Controllers\UserController;
use App\Models\User;

// Определение маршрута...
Route::get('/users/{user}', [UserController::class, 'show']);

// Определение метода контроллера...
public function show(User $user)
{
    return view('user.profile', ['user' => $user]);
}
```
#### Soft Deleted Models
Как правило, неявная привязка модели не будет извлекать модели, которые были удалены программно (Soft Deleted).Но можно указать неявной привязке извлекать эти модели, привязав метод `withTrashed` к маршруту.
```php
/* Привязка метода withTrashed к маршруту */

use App\Models\User;

Route::get('/users/{user}', function (User $user) {
    return $user->email;
})->withTrashed();
```
#### Customizing the Key
По желанию можно извлекать модели `Eloquent`, используя столбец, отличный от `id`. Для этого требуется указать столбец в определении параметра маршрута.
```php
/* Привязка модели Post через параметр slug */

use App\Models\Post;

Route::get('/posts/{post:slug}', function (Post $post) {
    return $post;
});
```
Если необходимо, чтобы при извлечении класса связанной модели всегда использовался столбец базы данных, отличный от `id`, то нужно переопределить метод `getRouteKeyName` модели `Eloquent`.
```php
/* Неявная привязка по полю - указанному в модели */

/**
 * Получить ключ маршрута для модели.
 */
public function getRouteKeyName(): string
{
    return 'slug';
}
```
#### Custom Keys and Scoping
При неявном связывании нескольких моделей `Eloquent` в одном определении маршрута бывает необходимо ограничить вторую модель `Eloquent` так, чтобы она была дочерней по отношению к предыдущей модели `Eloquent`.
```php
/* Пример неявной привязки двух моделей */

use App\Models\Post;
use App\Models\User;

Route::get('/users/{user}/posts/{post:slug}', function (User $user, Post $post) {
    return $post;
});
```
При использовании неявной привязки с измененным ключом в качестве параметра вложенного маршрута, Laravel автоматически задает ограничение запроса для получения вложенной модели своим родителем, используя соглашения, чтобы угадать имя отношения родительской модели.
В этом случае предполагается, что модель `User` имеет отношение с именем `posts` (форма множественного числа имени параметра маршрута), которое можно использовать для получения модели `Post`.
Так же можно указать Laravel охватывать “дочерние” привязки, даже если пользовательский ключ не предоставлен. Для этого необходимо вызвать метод `scopeBindings` при определении маршрута.
```php
/* Пример использования метода scopeBindings */

use App\Models\Post;
use App\Models\User;

Route::get('/users/{user}/posts/{post}', function (User $user, Post $post) {
    return $post;
})->scopeBindings();
```
Или указать целой группе определений маршрутов использовать привязки с заданной областью действия.
```php
/* Пример использования метода scopeBindings на группу*/

Route::scopeBindings()->group(function () {
    Route::get('/users/{user}/posts/{post}', function (User $user, Post $post) {
        return $post;
    });
});
```
Точно так же можно явно указать Laravel не использовать ограничение области действия привязок, вызвав метод `withoutScopedBindings`.
```php
/* Пример использования метода withoutScopedBindings */

Route::get('/users/{user}/posts/{post:slug}', function (User $user, Post $post) {
    return $post;
})->withoutScopedBindings();
```
#### Customizing Missing Model Behavior
Обычно, если неявно связанная модель не найдена, то генерируется `404` HTTP-ответ. Но можно изменить это поведение, вызвав метод `missing` при определении маршрута. Метод `missing` принимает замыкание, которое будет вызываться, если неявно связанная модель не может быть найдена.
```php
/* Пример использования метода missing */

use App\Http\Controllers\LocationsController;
use Illuminate\Http\Request;
use Illuminate\Support\Facades\Redirect;

Route::get('/locations/{location:slug}', [LocationsController::class, 'show'])
        ->name('locations.view')
        ->missing(function (Request $request) {
            return Redirect::route('locations.index');
        });
```
#### Implicit Enum Binding
 Laravel позволяет указывать `Enum` в определении маршрута, и Laravel будет вызывать маршрут только в том случае, если сегмент маршрута соответствует допустимому значению `Enum`. В противном случае автоматически будет возвращен ответ `HTTP 404`.
 ```php
/* Пример Enum для неявной привязки */

namespace App\Enums;

enum Category: string
{
    case Fruits = 'fruits';
    case People = 'people';
}
```
Можно определить маршрут, который будет вызываться только в случае, если сегмент `{category}` маршрута является `fruits` или `people`.
```php
/* Пример неявной привязки для Category Enum */

use App\Enums\Category;
use Illuminate\Support\Facades\Route;

Route::get('/categories/{category}', function (Category $category) {
    return $category->value;
});
```
### Explicit Binding
Yеобязательно использовать неявные привязки модели на основе соглашений Laravel, чтобы использовать привязку модели. Можно явно определить, как параметры маршрута должны быть сопоставлены моделям.
Чтобы зарегистрировать явную привязку есть метод маршрутизатора `model`, чтобы указать класс для переданного параметра. _**Необходимо определить явные привязки модели в начале метода `boot`  класса `AppServiceProvider`**_
```php
/* Определение явной привязки в методе boot AppServiceProvider */

use App\Models\User;
use Illuminate\Support\Facades\Route;

/**
 * Запуск любых служб приложения.
 */
public function boot(): void
{
    Route::model('user', User::class);
}
```
Затем определить маршрут, содержащий параметр `{user}`.
```php
/* Пример явной привязки модели */

use App\Models\User;

Route::get('/users/{user}', function (User $user) {
    // ...
});
```
Поскольку все параметры `{user}` связаны с моделью `App\Models\User`, экземпляр этого класса будет внедрен в маршрут.
Если соответствующий экземпляр модели не найден в базе данных, то автоматически будет сгенерирован `404` HTTP-ответ.
#### Customizing the Resolution Logic
Если необходимо определить свою собственную логику связывания модели, можно использовать метод `Route::bind`.
Замыкание, которое передается методу `bind`, получит значение сегмента `URI` и должно вернуть экземпляр класса, который должен быть внедрен в маршрут.
_**Эти изменения должны выполняться в методе `boot` поставщика `AppServiceProvider`**_
```php
/* Добавление custom привязки в методе boot AppServiceProvider */

use App\Models\User;
use Illuminate\Support\Facades\Route;

/**
 * Запуск любых служб приложения.
 */
public function boot(): void
{
    Route::bind('user', function (string $value) {
        return User::where('name', $value)->firstOrFail();
    });
}
```
В качестве альтернативы можно переопределить метод `resolveRouteBinding` модели `Eloquent`. Этот метод получит значение сегмента `URI` и должен вернуть экземпляр класса, который должен быть внедрен в маршрут.
```php
/**
 * Получить модель для привязанного к маршруту значения параметра.
 *
 * @param  mixed  $value
 * @param  string|null  $field
 * @return \Illuminate\Database\Eloquent\Model|null
 */
public function resolveRouteBinding($value, $field = null)
{
    return $this->where('name', $value)->firstOrFail();
}
```
Если в маршруте используется ограничения неявной привязки модели, то для получения связанной дочерней модели будет использоваться метод `resolveChildRouteBinding` родительской модели.
```php
/**
 * Получить дочернюю модель для привязанного к маршруту значения параметра.
 *
 * @param  string  $childType
 * @param  mixed  $value
 * @param  string|null  $field
 * @return \Illuminate\Database\Eloquent\Model|null
 */
public function resolveChildRouteBinding($childType, $value, $field)
{
    return parent::resolveChildRouteBinding($childType, $value, $field);
}
```
***
## Fallback Routes
Используя метод `Route::fallback`, можно определить маршрут, который будет выполняться, когда ни один другой маршрут не соответствует входящему запросу.
Как правило, необработанные запросы автоматически отображают страницу `404` через обработчик исключений приложения.
Но при определении резервного маршрута в  файле `routes/web.php`, все посредники группы `web` будет применены к этому маршруту. При необходимости можно добавить дополнительный посредник для этого маршрута.
```php
/* Пример метода fallback фасада Route */

Route::fallback(function () {
    // ...
});
```
***
## Rate Limiting
### Defining Rate Limiters
Laravel включает в себя мощные и настраиваемые сервисы для ограничения частоты запросов, которые можно использовать для ограничения объема трафика для определенного маршрута или группы маршрутов.
Ограничители скорости (`RateLimiter`) могут быть определены в методе `boot` класса `App\Providers\AppServiceProvider`.
```php
/* Пример RateLimiter в методе boot AppServiceProvider */

use Illuminate\Cache\RateLimiting\Limit;
use Illuminate\Http\Request;
use Illuminate\Support\Facades\RateLimiter;

/**
 * Запуск любых служб приложения.
 */
protected function boot(): void
{
    RateLimiter::for('api', function (Request $request) {
        return Limit::perMinute(60)->by($request->user()?->id ?: $request->ip());
    });
}
```
Ограничители определяются с помощью метода `for` фасада `RateLimiter`. Метод `for` принимает имя ограничителя и замыкание, которое возвращает конфигурацию ограничений, применяемых к назначенным маршрутам. Конфигурация ограничений – это экземпляры класса `Illuminate\Cache\RateLimiting\Limit`.
```php
/* Пример RateLimiter с именем global в методе boot AppServiceProvider */

use Illuminate\Cache\RateLimiting\Limit;
use Illuminate\Http\Request;
use Illuminate\Support\Facades\RateLimiter;

/**
 * Запуск любых служб приложения.
 */
protected function boot(): void
{
    RateLimiter::for('global', function (Request $request) {
        return Limit::perMinute(1000);
    });
}
```
Если входящий запрос превышает указанный лимит, то Laravel автоматически вернет ответ с `429` кодом состояния HTTP. Если необходимо определить другой ответ, который должен возвращаться, можно  использовать метод `response`.
```php
/* Использование response в RateLimiter */

RateLimiter::for('global', function (Request $request) {
    return Limit::perMinute(1000)->response(function (Request $request, array $headers) {
        return response('Custom response...', 429, $headers);
    });
});
```
Поскольку замыкание получает экземпляр входящего HTTP-запроса, то есть возможность динамически создать ограничение на основе входящего запроса или статуса аутентификации пользователя.
```php
/* Ограничение на основе входящего запроса */

RateLimiter::for('uploads', function (Request $request) {
    return $request->user()->vipCustomer()
                ? Limit::none()
                : Limit::perMinute(100);
});
```
### Segmenting Rate Limits
Иногда может потребоваться сегментация ограничений по некоторым произвольным значениям (Например разрешить пользователям получать доступ к указанному маршруту 100 раз в минуту на каждый IP-адрес). Для этого можно использовать метод `by` при построении лимита.
```php
/* Пример использования метода by фасада Limit */

RateLimiter::for('uploads', function (Request $request) {
    return $request->user()->vipCustomer()
                ? Limit::none()
                : Limit::perMinute(100)->by($request->ip());
});

/* Похожий пример с другими значениями */

RateLimiter::for('uploads', function (Request $request) {
    return $request->user()
                ? Limit::perMinute(100)->by($request->user()->id)
                : Limit::perMinute(10)->by($request->ip());
});
```
### Multiple Rate Limits
При необходимости можно вернуть массив ограничений при указании конфигурации ограничителя. Каждое ограничение будет оцениваться для маршрута в зависимости от порядка, в котором они размещены в массиве.
```php
/* Пример использования массива ограничений Limit */

RateLimiter::for('login', function (Request $request) {
    return [
        Limit::perMinute(500),
        Limit::perMinute(3)->by($request->input('email')),
    ];
});
```
Если назначается несколько ограничений скорости, сегментированных по одинаковым значениям `by`, следует убедиться, что каждое значение `by` уникально. Самый простой способ добиться этого — добавить префикс к значениям, заданным методом `by`.
```php
/* Пример использования массива Limit с двумя ограничениями скорости */

RateLimiter::for('uploads', function (Request $request) {
    return [
        Limit::perMinute(10)->by('minute:'.$request->user()->id),
        Limit::perDay(1000)->by('day:'.$request->user()->id),
    ];
});
```
***
## Attaching Rate Limiters to Routes
Ограничители могут быть закреплены за маршрутами или группами маршрутов с помощью `middleware` `throttle`. Посредник `throttle` принимает имя ограничителя, которое необходимо назначить маршруту.
```php
/* Пример middleware throttle */

Route::middleware(['throttle:uploads'])->group(function () {
    Route::post('/audio', function () {
        // ...
    });

    Route::post('/video', function () {
        // ...
    });
});
```
### Throttling With Redis
По умолчанию посредник `throttle` сопоставлен классу `Illuminate\Routing\Middleware\ThrottleRequests`. 
Eсли вы используется Redis в качестве драйвера кэша приложения, можно поручить Laravel использовать Redis для управления ограничением скорости. Для этого следует использовать метод `throttleWithRedis` в файле `bootstrap/app.php`. Этот метод сопоставляет `middleware` `throttle` с классом `middleware` `Illuminate\Routing\Middleware\ThrottleRequestsWithRedis`.
```php
/* Пример middleware throttleWithRedis */

->withMiddleware(function (Middleware $middleware) {
    $middleware->throttleWithRedis();
    // ...
})
```
***
## Form Method Spoofing
HTML-формы не поддерживают действия `PUT`, `PATCH` или `DELETE`. Таким образом, при определении маршрутов `PUT`, `PATCH` или `DELETE`, которые вызываются из HTML-формы, нужно добавить в форму скрытое поле `_method`. Значение, отправленное с полем `_method`, будет использоваться как метод HTTP-запроса.
```php
/* Добавление _method в HTML форму */

<form action="/example" method="POST">
    <input type="hidden" name="_method" value="PUT">
    <input type="hidden" name="_token" value="{{ csrf_token() }}">
</form>
```
Так же можно использовать директиву `@method` в шаблонизаторе `Blade`.
```php
/* Добавление _method в Blade форму */

<form action="/example" method="POST">
    @method('PUT')
    @csrf
</form>
```
***
## Accessing the Current Route
Можно использовать методы `current`, `currentRouteName` и `currentRouteAction` фасада `Route` для доступа к информации о маршруте, обрабатывающем входящий запрос.
```php
/* Пример получения информации о текущем маршруте */

use Illuminate\Support\Facades\Route;

$route = Route::current(); // Illuminate\Routing\Route
$name = Route::currentRouteName(); // string
$action = Route::currentRouteAction(); // string
```
***
## Cross-Origin Resource Sharing (`CORS`)
Laravel может автоматически отвечать на HTTP-запросы `CORS` `OPTIONS`  сконфигурируемыми значениями. Запросы `OPTIONS` будут автоматически обрабатываться `HandleCors` `middleware`, который автоматически включается в глобальный стек промежуточного программного обеспечения приложения.
- `php artisan config:publish cors` - команда для конфигурации `CORS`. Поместит файл конфигурации `cors.php` в каталог `config`.
***
## Route Caching
При развертывании приложения на рабочем веб-сервере, необходимо воспользоваться кешем маршрутов Laravel. Использование кеша маршрутов резко сократит время, необходимое для регистрации всех маршрутов приложения.
- `php artisan route:cache` - команда для кэширования маршрутов.
После выполнения команды файл кеша маршрутов будет загружаться при каждом запросе. При добавлении новых маршрутов, необходимо будет сгенерировать новый кеш маршрутов. По этой причине запускать команду `route:cache` только во время развертывания проекта.
- `php artisan route:clear` - команда для очиски кэша маршрутов.
***
