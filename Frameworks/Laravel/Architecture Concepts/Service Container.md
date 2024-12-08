# Service Container
***
## Basic
**Контейнер служб (service container, сервис-контейнер)** – это мощный инструмент для управления зависимостями классов и выполнения **внедрения зависимостей (DI)**.
**Внедрение зависимостей** – зависимости классов «вводятся» в класс через конструктор в виде аргументов или, в некоторых случаях, через методы-сеттеры. При создании класса или вызове методов фреймворк смотрит на список аргументов и, если нужно, создаёт экземпляры необходимых классов и сам подаёт их на вход конструктора или метода.
```php
/* Пример внедрения зависимостей */

namespace App\Http\Controllers;

use App\Services\AppleMusic;
use Illuminate\View\View;

final class PodcastController extends Controller
{
    /**
     * Создать новый экземпляр контроллера.
     */
    public function __construct(
        protected AppleMusic $apple,
    ) {}

    /**
     * Показать информацию о данном подкасте.
     */
    public function show(string $id): View
    {
        return view('podcasts.show', [
            'podcast' => $this->apple->findPodcast($id)
        ]);
    }
}
```
Глубокое понимание **контейнера служб** Laravel необходимо для создания большого, мощного приложения.
***
## Zero Configuration Resolution
Если класс не имеет зависимостей или зависит только от других конкретных классов (не интерфейсов), контейнер не нужно инструктировать о том, как создавать этот класс.
```php
/* Пример внедрения зависимости без конфигурации */

final class Service
{
    // ...
}

Route::get('/', function (Service $service) {
    die($service::class);
});

/* В этом примере, при посещении `/` вашего приложения, маршрут автоматически получит класс `Service` и внедрит его в обработчике вашего маршрута.
Это означает, что можно разработать приложение и воспользоваться преимуществами внедрения зависимостей, не беспокоясь о раздутых файлах конфигурации. */
```
Многие классы в приложения Laravel, автоматически получают свои зависимости через контейнер (`Controllers`, `EventListeners`, `Middlerware`).
Можно указать зависимости в методе `handle` обработки заданий в очереди (`Queues`).
***
## When to Utilize the Container
Благодаря автоматическому внедрению зависимостей и фасадам, можно разрабатывать приложения Laravel без необходимости **когда-либо** вручную связывать или извлекать что-либо из контейнера.
Когда придется лезть вручную.
1.  Если вы пишете класс, реализующий интерфейс, и хотите объявить тип этого интерфейса в конструкторе маршрута или класса.
2. Если вы пишете пакет Laravel. 
***
## Binding
### Binding Basics
#### Simple Bindings (Простое связывание)
Почти все связывания в контейнере служб будут зарегистрированы в **поставщиках служб (Providers)**, поэтому в большинстве этих примеров будет продемонстрировано использование контейнера в этом контексте.
Внутри **поставщика служб (Service Provider)** всегда есть доступ к контейнеру через свойство `$this->app`. Можно зарегистрировать связывание, используя метод `bind`, передав имя класса или интерфейса, которые необходимо зарегистрировать, вместе с замыканием, возвращающим экземпляр класса.
```php
/* Пример простого связывания в Service Provider */

use App\Services\Transistor;
use App\Services\PodcastParser;
use Illuminate\Contracts\Foundation\Application;

$this->app->bind(Transistor::class, function (Application $app) {
    return new Transistor($app->make(PodcastParser::class));
});
```
В итоге, получаем сам контейнер в качестве аргумента. Затем необходимо использовать контейнер для извлечения под-зависимостей объекта, который создаем.
Обычно взаимодействие с контейнером происходит внутри поставщиков служб; однако, если нужно взаимодействовать с контейнером в других частях приложения, можно сделать это через фасад `App`
```php
/* Пример простого связывания c помощью фасада App */

use App\Services\Transistor;
use Illuminate\Contracts\Foundation\Application;
use Illuminate\Support\Facades\App;

App::bind(Transistor::class, function (Application $app) {
    // ...
});
```
Можно использовать метод `bindIf` для регистрации привязки контейнера _**(только в том случае, если привязка уже не была зарегистрирована для данного типа)**_
```php
/* Пример простого связывания c помощью bindOf */

$this->app->bindIf(Transistor::class, function (Application $app) {
    return new Transistor($app->make(PodcastParser::class));
});
```
> **Нет необходимости привязывать классы в контейнере, если они не зависят от каких-либо интерфейсов. Контейнеру не нужно указывать, как создавать эти объекты, поскольку он может автоматически извлекать эти объекты с помощью рефлексии.**
#### Binding A Singleton
Метод `singleton` связывает в контейнере класс или интерфейс, который должен быть извлечен только один раз. При последующих обращениях к этому классу из контейнера будет возвращен полученный ранее экземпляр объекта.
```php
/* Пример singleton связывания в Service Provider */

use App\Services\Transistor;
use App\Services\PodcastParser;
use Illuminate\Contracts\Foundation\Application;

$this->app->singleton(Transistor::class, function (Application $app) {
    return new Transistor($app->make(PodcastParser::class));
});
```
Можно использовать метод `singletonIf` для регистрации синглтон-привязки контейнера _**(только в том случае, если привязка уже не была зарегистрирована для данного типа)**_
```php
/* Пример singleton связывания в Service Provider с помощью singletonIf */

$this->app->singletonIf(Transistor::class, function (Application $app) {
    return new Transistor($app->make(PodcastParser::class));
});
```
#### Binding Scoped Singletons
Метод `scoped` связывает в контейнере класс или интерфейс, который должен быть извлечен только один раз в течение данного жизненного цикла запроса / задания Laravel.
 Хотя этот метод похож на метод `singleton` похож на метод `scoped`, экземпляры, зарегистрированные с помощью метода `scoped`, будут сбрасываться всякий раз, когда приложение Laravel запускает новый «жизненный цикл».
```php
/* Пример scoped singleton связывания */

use App\Services\Transistor;
use App\Services\PodcastParser;
use Illuminate\Contracts\Foundation\Application;

$this->app->scoped(Transistor::class, function (Application $app) {
    return new Transistor($app->make(PodcastParser::class));
});
```
Можно использовать метод `scopedIf` для регистрации привязки контейнера с ограниченной областью действия _**(только в том случае, если привязка уже не была зарегистрирована для данного типа)**_.
```php
/* Пример scoped singleton связывания методом scopedIf*/

$this->app->scopedIf(Transistor::class, function (Application $app) {
    return new Transistor($app->make(PodcastParser::class));
});
```
#### Binding Instances
Также можно привязать существующий экземпляр объекта в контейнере, используя метод `instance`. Переданный экземпляр всегда будет возвращен из контейнера при последующих вызовах.
```php
/* Пример привязки экземпляра класса */

use App\Services\Transistor;
use App\Services\PodcastParser;

$service = new Transistor(new PodcastParser);

$this->app->instance(Transistor::class, $service);
```
### Binding Interfaces to Implementations
Связывание интерфейса с конкретной реализацией.
```php
/* Пример привязки экземпляра класса к интерфейсу */

use App\Contracts\EventPusher;
use App\Services\RedisEventPusher;

$this->app->bind(EventPusher::class, RedisEventPusher::class);
```
Эта запись сообщает контейнеру, что он должен внедрить `RedisEventPusher`, когда классу требуется реализация `EventPusher`. Теперь мы можем указать интерфейс `EventPusher` в конструкторе класса, который будет извлечен контейнером.
**Контроллеры**, **слушатели событий**, **посредники** и некоторые другие типы классов в приложениях Laravel всегда выполняются с помощью контейнера.
```php
/* Пример использования DI */

use App\Contracts\EventPusher;

/**
 * Создать новый экземпляр класса.
 */
public function __construct(
    protected EventPusher $pusher,
) {}
```
### Contextual Binding
Иногда может быть два класса, которые используют один и тот же интерфейс, но требуется внедрить разные реализации в каждый класс.
```php
/* Пример регистрации контекста */

use App\Http\Controllers\PhotoController;
use App\Http\Controllers\UploadController;
use App\Http\Controllers\VideoController;
use Illuminate\Contracts\Filesystem\Filesystem;
use Illuminate\Support\Facades\Storage;

$this->app->when(PhotoController::class)
          ->needs(Filesystem::class)
          ->give(function () {
              return Storage::disk('local');
          });

$this->app->when([VideoController::class, UploadController::class])
          ->needs(Filesystem::class)
          ->give(function () {
              return Storage::disk('s3');
          });
```
### Contextual Attributes
Поскольку контекстная привязка часто используется для внедрения реализаций драйверов или значений конфигурации, Laravel предлагает множество атрибутов контекстной привязки, которые позволяют внедрять эти типы значений без ручного определения контекстных привязок у ваших поставщиков услуг.
```php
/* Пример атрибута Storage */

namespace App\Http\Controllers;

use Illuminate\Container\Attributes\Storage;
use Illuminate\Contracts\Filesystem\Filesystem;

class PhotoController extends Controller
{
    public function __construct(
        #[Storage('local')] protected Filesystem $filesystem
    )
    {
        // ...
    }
}
```
Список контекстуальных аттрибутов.
- `Storage`
- `Auth`
- `Cache`
- `Config`
- `DB`
- `Log`
- `RouteParameter`
- `Tag`
- `CurrentUser`
```php
/* Пример использования атрибутов */

namespace App\Http\Controllers;

use App\Models\Photo;
use Illuminate\Container\Attributes\Auth;
use Illuminate\Container\Attributes\Cache;
use Illuminate\Container\Attributes\Config;
use Illuminate\Container\Attributes\DB;
use Illuminate\Container\Attributes\Log;
use Illuminate\Container\Attributes\RouteParameter;
use Illuminate\Container\Attributes\Tag;
use Illuminate\Contracts\Auth\Guard;
use Illuminate\Contracts\Cache\Repository;
use Illuminate\Database\Connection;
use Psr\Log\LoggerInterface;

class PhotoController extends Controller
{
    public function __construct(
        #[Auth('web')] protected Guard $auth,
        #[Cache('redis')] protected Repository $cache,
        #[Config('app.timezone')] protected string $timezone,
        #[DB('mysql')] protected Connection $connection,
        #[Log('daily')] protected LoggerInterface $log,
        #[RouteParameter('photo')] protected Photo $photo,
        #[Tag('reports')] protected iterable $reports,
    )
    {
        // ...
    }
}
```
**CurrentUser** - атрибут для добавления текущего аутентифицированного пользователя в заданный маршрут или класс.
```php
/* Пример использования атрибута CurrentUser */

use App\Models\User;
use Illuminate\Container\Attributes\CurrentUser;

Route::get('/user', function (#[CurrentUser] User $user) {
    return $user;
})->middleware('auth');
```
#### Defining Custom Attributes
Можно создавать свои собственные контекстные атрибуты, реализуя контракт `Illuminate\Contracts\Container\ContextualAttribute`. 
Контейнер вызовет метод `resolve` нового атрибута, который должен разрешить значение, которое должно быть введено в класс, использующий атрибут.
```php
/* Пример реализации пользовательского атрибута Config */

namespace App\Attributes;

use Illuminate\Contracts\Container\ContextualAttribute;

#[Attribute(Attribute::TARGET_PARAMETER)]
class Config implements ContextualAttribute
{
    /**
     * Create a new attribute instance.
     */
    public function __construct(public string $key, public mixed $default = null)
    {
    }

    /**
     * Resolve the configuration value.
     *
     * @param  self  $attribute
     * @param  \Illuminate\Contracts\Container\Container  $container
     * @return mixed
     */
    public static function resolve(self $attribute, Container $container)
    {
        return $container->make('config')->get($attribute->key, $attribute->default);
    }
}
```
### Binding Primitives
Иногда внедренный класс может нуждаться в примитевном типе создании экземпляра.
```php
/* Пример внедрения примитива variableName в Service Container */

use App\Http\Controllers\UserController;

$this->app->when(UserController::class)
          ->needs('$variableName')
          ->give($value);
```
Иногда класс может зависеть от массива экземпляров, объединенных **меткой**. Для реализации использовать метод `giveTagged`.
```php
/* Пример внедрения массивов с помощью giveTagged */

$this->app->when(ReportAggregator::class)
    ->needs('$reports')
    ->giveTagged('reports');
```
Можно внедрить примитовное значение из файла конфигурации с помощью метода `giveConfig`.
```php
/* Пример внедрения примитива из конфига */

$this->app->when(ReportAggregator::class)
    ->needs('$timezone')
    ->giveConfig('app.timezone');
```
### Binding Typed Variadics
Контекстная привязка переменного количества аргументов-объектов при внедрении зависмостей с помощью метода `give`.
```php
/* Пример класса с переменным количеством зависимостей */

use App\Models\Filter;
use App\Services\Logger;

class Firewall
{
    /**
     * Экземпляры фильтра.
     *
     * @var array
     */
    protected $filters;

    /**
     * Создать новый экземпляр класса.
     */
    public function __construct(
        protected Logger $logger,
        Filter ...$filters,
    ) {
        $this->filters = $filters;
    }
}

/* Реализация в Service Provider с помощью метода give */

$this->app->when(Firewall::class)
          ->needs(Filter::class)
          ->give(function (Application $app) {
                return [
                    $app->make(NullFilter::class),
                    $app->make(ProfanityFilter::class),
                    $app->make(TooLongFilter::class),
                ];
          });

/* Реализация в Service Provider с помощью метода give без замыкания */

$this->app->when(Firewall::class)
          ->needs(Filter::class)
          ->give([
              NullFilter::class,
              ProfanityFilter::class,
              TooLongFilter::class,
          ]);
```
#### Variadic Tag Dependencies
Иногда класс может иметь вариативную зависимость, указывающую на тип как переданный класс (`Report ...$reports`). Используя методы `needs` и `giveTagged`, можно легко внедрить все привязки контейнера с этой **метокой** (tag) для управления зависимости.
```php
/* Пример привязки Service Container с указанным tag */

$this->app->when(ReportAggregator::class)
    ->needs(Report::class)
    ->giveTagged('reports');
```
### Tagging
Иногда может потребоваться получить все привязки определенной «категории». После регистрации реализаций можно назначить им метку с помощью метода `tag`.
```php
/* Пример создания метки (tag) при привязке зависимостей */

$this->app->bind(CpuReport::class, function () {
    // ...
});

$this->app->bind(MemoryReport::class, function () {
    // ...
});

$this->app->tag([CpuReport::class, MemoryReport::class], 'reports');
```
После того как службы помечены,  их можно получить с помощью метода `tagged`.
```php
/* Пример получения зависимосте по метке (tag) */

$this->app->bind(ReportAnalyzer::class, function (Application $app) {
    return new ReportAnalyzer($app->tagged('reports'));
});
```
### Extending Bindings
Метод `extend` позволяет модифицировать извлеченные службы. Когда **служба** (service) получена, вы можете выполнить дополнительный код для декорирования или конфигурирования **службы**.
Метод `extend` принимает два аргумента: класс службы, который вы расширяете, и замыкание, которое должно возвращать модифицированную службу.
```php
/* Пример модификации Service методом extend */

$this->app->extend(Service::class, function (Service $service, Application $app) {
    return new DecoratedService($service);
});
```
***
## Resolving
### Метод `make`
