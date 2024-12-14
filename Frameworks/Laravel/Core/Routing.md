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
