# Service Providers
***
## Introduction
**Сервис-провайдеры** – это центральное место начальной загрузки всех приложений Laravel. Пользовательское приложение, а также все основные службы и сервисы Laravel загружаются через Service-Provider.
Начальная загрузка - **регистрация** элементов, включая регистрацию связываний контейнера служб (service container), слушателей событий (event listener), посредников (middleware) и даже маршрутов (route). 
**Сервис-провайдеры** являются центральным местом для конфигурирования приложения.
Laravel использует десятки поставщиков услуг внутренне для инициализации своих основных сервисов (почтовый сервис, очереди, кэш и другие). Многие из этих поставщиков являются “*отложенными*”, что означает, что они не будут загружены при каждом запросе, а только когда нужны фактические сервисы, которые они предоставляют.
- `bootstrap/providers.php` - скрипт для подключения пользовательских сервисов.
***
## Writing Service Providers
Все сервис-провайдеры расширяют класс `Illuminate\Support\ServiceProvider`. Большинство сервис-провайдеров содержат метод `register` и `boot`.
В рамках метода `register` следует только связывать (`bind`) сущности в `Service Container`.
> **Никогда не следует пытаться зарегистрировать каких-либо слушателей событий, маршруты или что-то другое в методе `register`. В противном случае можно случайно воспользоваться подсистемой, чей сервис-провайдер еще не загружен** 
- `php artisan make:provider [new_provider_name]` - команда для создания и рестрации нового пользовательского Service Provider. 
Laravel автоматически зарегистрирует нового сервис-провайдера в файле `bootstrap/providers.php`
### The Register Method
Как упоминалось ранее, в рамках метода `register` следует только связывать сущности в `Service Container`.
```php
/* Пример Service Provider */

namespace App\Providers;

use App\Services\Riak\Connection;
use Illuminate\Contracts\Foundation\Application;
use Illuminate\Support\ServiceProvider;

class RiakServiceProvider extends ServiceProvider
{
    /**
     * Регистрация любых служб приложения.
     */
    public function register(): void
    {
        $this->app->singleton(Connection::class, function (Application $app) {
            return new Connection(config('riak'));
        });
    }
}
```
Этот сервис-провайдер определяет только метод `register` и использует этот метод для указания, какая именно реализация `App\Services\Riak\Connection` будет применена в нашем приложении – при помощи Service Container.
#### The bindings and singletons Properties
Если Service Provider регистрирует много простых связываний, можно использовать свойства `bindings` и `singletons` вместо ручной регистрации каждого связывания контейнера.
Когда сервис-провайдер загружается фреймворком, он автоматически проверяет эти свойства и регистрирует их связывания.
```php
/* Пример общей регистрации Service Provider */

namespace App\Providers;

use App\Contracts\DowntimeNotifier;
use App\Contracts\ServerProvider;
use App\Services\DigitalOceanServerProvider;
use App\Services\PingdomDowntimeNotifier;
use App\Services\ServerToolsProvider;
use Illuminate\Support\ServiceProvider;

class AppServiceProvider extends ServiceProvider
{
    /**
     * Все связывания контейнера, которые должны быть зарегистрированы.
     *
     * @var array
     */
    public $bindings = [
        ServerProvider::class => DigitalOceanServerProvider::class,
    ];

    /**
     * Все синглтоны контейнера, которые должны быть зарегистрированы.
     *
     * @var array
     */
    public $singletons = [
        DowntimeNotifier::class => PingdomDowntimeNotifier::class,
        ServerProvider::class => ServerToolsProvider::class,
    ];
}
```
### The Boot Method
**Этот метод вызывается после регистрации всех остальных сервис-провайдеров**, что означает, что в этом месте у вас уже есть доступ ко всем другим службам, которые были зарегистрированы фреймворком.
```php
/* Пример использования boot в Service Provider */

namespace App\Providers;

use Illuminate\Support\Facades\View;
use Illuminate\Support\ServiceProvider;

class ComposerServiceProvider extends ServiceProvider
{
    /**
     * Загрузка любых служб приложения.
     */
    public function boot(): void
    {
        View::composer('view', function () {
            // ...
        });
    }
}
```
#### Boot Method Dependency Injection
Можно указывать тип зависимостей в методе `boot` в Service Provider.
Service Container автоматически внедрит любые необходимые зависимости.
```php
/* DI в методе boot */

use Illuminate\Contracts\Routing\ResponseFactory;

/**
 * Загрузка любых служб приложения.
 */
public function boot(ResponseFactory $response): void
{
    $response->macro('serialized', function (mixed $value) {
        // ...
    });
}
```
***
## Registering Providers
Все поставщики услуг (Service Provider) регистрируются в файле конфигурации `bootstrap/providers.php`.
Этот файл возвращает массив, который содержит имена классов поставщиков услуг приложения.
```php
/* Пример массива providers.php */

return [
    App\Providers\AppServiceProvider::class,
];
```
При вызове команды `make:provider` в Artisan, Laravel автоматически добавит сгенерированный провайдер в файл `bootstrap/providers.php`. Однако, при создании класса провайдера вручную, необходимо вручную добавить класс провайдера в массив.
```php
/* Добавление Sercive Provider в массив providers.php */

return [
    App\Providers\AppServiceProvider::class,
    App\Providers\ComposerServiceProvider::class, // [tl! add]
];
```
***
## Deferred Providers
Если `Service Provider` регистрирует **только** связывания в `Service Container`. Можно отложить его регистрацию до тех пор, пока одно из зарегистрированных связываний не понадобится.
Отсрочка загрузки такого сервис-провайдера повысит производительность приложения, так как он не загружается из файловой системы при каждом запросе.
Laravel составляет и сохраняет список всех служб, предоставляемых отложенными сервис-провайдерами, а также имя класса сервис-провайдера. Laravel загрузит сервис-провайдер только при необходимости в одной из этих служб.
Чтобы отложить загрузку сервис-провайдера, реализуйте интерфейс `\Illuminate\Contracts\Support\DeferrableProvider`, описав метод `provides`. Метод `provides` должен вернуть связывания контейнера службы, регистрируемые данным классом.
```php
/* Реализация интерфейса DeferrableProvider для отложенной загрузки */

namespace App\Providers;

use App\Services\Riak\Connection;
use Illuminate\Contracts\Foundation\Application;
use Illuminate\Contracts\Support\DeferrableProvider;
use Illuminate\Support\ServiceProvider;

class RiakServiceProvider extends ServiceProvider implements DeferrableProvider
{
    /**
     * Регистрация любых служб приложения.
     */
    public function register(): void
    {
        $this->app->singleton(Connection::class, function (Application $app) {
            return new Connection($app['config']['riak']);
        });
    }

    /**
     * Получить службы, предоставляемые поставщиком.
     *
     * @return array<int, string>
     */
    public function provides(): array
    {
        return [Connection::class];
    }
}
```
***
