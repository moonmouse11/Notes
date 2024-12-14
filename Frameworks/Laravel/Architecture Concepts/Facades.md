# Facades
***
## Introduction
**Фасады** предоставляют «статический» интерфейс для классов, доступных в Service Container приложения. 
Laravel из коробки включает множество фасадов, обеспечивающих доступ почти ко всему функционалу Laravel.
**Фасады** Laravel служат «статическими прокси» для базовых классов в контейнере служб, обеспечивая преимущества краткого, выразительного синтаксиса при сохранении большей тестируемости и гибкости, чем традиционные статические методы.
Все фасады Laravel определены в пространстве имён `Illuminate\Support\Facades`.
```php
/* Пример использования Фасадов */

use Illuminate\Support\Facades\Cache;
use Illuminate\Support\Facades\Route;

Route::get('/cache', function () {
    return Cache::get('key');
});
```
***
## Helper Functions
В дополнении к фасадам, Laravel предлагает множество глобальных «вспомогательных функций», которые упрощают взаимодействие с общими функциями Laravel - это `view()`, `response()`, `url()`, `config()` и т.д.
Например, вместо использования фасада `Illuminate\Support\Facades\Response` для генерации ответа `JSON`,  можно использовать функцию `response()`. Поскольку помощники доступны глобально, то нет необходимости импортировать какие-либо классы, чтобы использовать их.
```php
/* Разница использования фасадов и функций хелперов */

use Illuminate\Support\Facades\Response;

Route::get('/users', function () {
    return Response::json([
        // ...
    ]);
});

Route::get('/users', function () {
    return response()->json([
        // ...
    ]);
});
```
***
## When to Utilize Facades
**Фасады** предоставляют краткий, запоминающийся синтаксис, позволяющий использовать функции Laravel, не запоминая длинные имена классов, которые необходимо вводить или конфигурировать вручную.
**Основная опасность фасадов** – «разрастание» класса. Фасады просты в использовании и не требуют внедрений, что легко сказывается на разрастании класса и использовании множества фасадов в одном классе.
***
## Facades vs. Dependency Injection
Основное преимущество внедрения зависимостей является возможность изменения реализации внедренного класса.
Как правило, невозможно имитировать или заглушить действительно статический метод класса. Однако, поскольку фасады используют динамические методы для проксирования вызовов методов к объектам, извлекаемым из контейнера служб, мы фактически можем изменять реализацию фасада так же, как изменили бы внедренный экземпляр класса.
```php
/* Пример тестирования фасада Route */

use Illuminate\Support\Facades\Cache;

Route::get('/cache', function () {
    return Cache::get('key');
});

/* Пример теста PEST */

use Illuminate\Support\Facades\Cache;

test('basic example', function () {
    Cache::shouldReceive('get')
         ->with('key')
         ->andReturn('value');

    $response = $this->get('/cache');

    $response->assertSee('value');
});

/* Пример теста PHPUnit */

use Illuminate\Support\Facades\Cache;

/**
 * A basic functional test example.
 */
public function test_basic_example(): void
{
    Cache::shouldReceive('get')
         ->with('key')
         ->andReturn('value');

    $response = $this->get('/cache');

    $response->assertSee('value');
}
```
***
## Facades vs. Helper Functions
Помимо фасадов, Laravel включает в себя множество «вспомогательных» функций, которые могут выполнять общие задачи (генерация шаблонов, запуск событий, запуск заданий или отправка HTTP-ответов).
Многие из этих вспомогательных функций выполняют ту же функцию, что и соответствующий фасад.
```php
/* Пример использования Фасада и хелпера */

return Illuminate\Support\Facades\View::make('profile');

return view('profile');
```
Практической разницы между фасадами и глобальными помощниками нет абсолютно никакой.
***
## How Facades Work
В Laravel **фасад** – это класс, который обеспечивает доступ к объекту из контейнера. Техника, которая выполняет эту работу, относится к классу `Facade`. Фасады Laravel и любые пользовательские фасады, которые вы создаете, будут расширять базовый класс `Illuminate\Support\Facades\Facade`.
Базовый класс `Facade` использует магический метод `__callStatic()`, чтобы делегировать вызовы с вашего фасада объекту, извлеченному из контейнера.
```php
/* Пример использования Cache фасада */

namespace App\Http\Controllers;

use App\Http\Controllers\Controller;
use Illuminate\Support\Facades\Cache;
use Illuminate\View\View;

class UserController extends Controller
{
    /**
     * Показать профиль конкретного пользователя.
     */
    public function showProfile(string $id): View
    {
        $user = Cache::get('user:'.$id);

        return view('profile', ['user' => $user]);
    }
}
```
Фасад `Cache` импортируется в начале скрипта. Этот фасад служит прокси для доступа к базовой реализации интерфейса `Illuminate\Contracts\Cache\Factory`. Любые вызовы, с использованием фасада, будут переданы в базовый экземпляр службы кеширования Laravel.
В классе `Illuminate\Support\Facades\Cache` статического метода `get` не существует.
Вместо этого фасад `Cache` расширяет базовый класс `Facade` и определяет метод `getFacadeAccessor()`. Задача этого метода – вернуть имя привязки контейнера службы. Когда пользователь ссылается на любой статический метод фасада `Cache`, Laravel извлекает объект из `Service Container` привязанный к `cache` и запускает запрошенный метод (в данном случае `get`) этого объекта.
```php
/* Пример реализации Cache фасада */

class Cache extends Facade
{
    /**
     * Получить зарегистрированное имя компонента.
     */
    protected static function getFacadeAccessor(): string
    {
        return 'cache';
    }
}
```
***
## Real-Time Facades
Используя фасады в реальном времени, можно рассматривать любой класс в приложении, как если бы он был фасадом.
Используя фасады в реальном времени, мы можем поддерживать такую же изменяемость реализации, при этом не требуя явной передачи экземпляра.
Чтобы сгенерировать фасад в реальном времени, добавьте к пространству имен импортируемого класса префикс `Facades`
```php
/* Пример реализации real-time фасада Publisher */

namespace App\Models;

// use App\Contracts\Publisher; // [tl! remove]
use Facades\App\Contracts\Publisher; // [tl! add]
use Illuminate\Database\Eloquent\Model;

class Podcast extends Model
{
    /**
     * Опубликовать подкаст.
     */
    // public function publish(Publisher $publisher): void // [tl! remove]
    public function publish(): void // [tl! add]
    {
        $this->update(['publishing' => now()]);

        $publisher->publish($this); // [tl! remove]
        Publisher::publish($this); // [tl! add]
    }
}
```
Когда используется real-time Facade, реализация `Publisher` будет получена из `Service Container` с использованием той части интерфейса или имени класса, которая расположена после префикса `Facades`.
***
