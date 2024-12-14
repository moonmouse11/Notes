# Facade Class Reference
***
Список фасадов и связанных базовых классов.

| Фасад                 | Класс                                                                                                                                | Привязка в контейнере служб |
| --------------------- | ------------------------------------------------------------------------------------------------------------------------------------ | --------------------------- |
| App                   | [Illuminate\Foundation\Application](https://laravel.com/api/11.x/Illuminate/Foundation/Application.html)                             | `app`                       |
| Artisan               | [Illuminate\Contracts\Console\Kernel](https://laravel.com/api/11.x/Illuminate/Contracts/Console/Kernel.html)                         | `artisan`                   |
| Auth (Instance)       | [Illuminate\Contracts\Auth\Guard](https://laravel.com/api/11.x/Illuminate/Contracts/Auth/Guard.html)                                 | `auth.driver`               |
| Auth                  | [Illuminate\Auth\AuthManager](https://laravel.com/api/11.x/Illuminate/Auth/AuthManager.html)                                         | `auth`                      |
| Blade                 | [Illuminate\View\Compilers\BladeCompiler](https://laravel.com/api/11.x/Illuminate/View/Compilers/BladeCompiler.html)                 | `blade.compiler`            |
| Broadcast (Instance)  | [Illuminate\Contracts\Broadcasting\Broadcaster](https://laravel.com/api/11.x/Illuminate/Contracts/Broadcasting/Broadcaster.html)     |                             |
| Broadcast             | [Illuminate\Contracts\Broadcasting\Factory](https://laravel.com/api/11.x/Illuminate/Contracts/Broadcasting/Factory.html)             |                             |
| Bus                   | [Illuminate\Contracts\Bus\Dispatcher](https://laravel.com/api/11.x/Illuminate/Contracts/Bus/Dispatcher.html)                         |                             |
| Cache (Instance)      | [Illuminate\Cache\Repository](https://laravel.com/api/11.x/Illuminate/Cache/Repository.html)                                         | `cache.store`               |
| Cache                 | [Illuminate\Cache\CacheManager](https://laravel.com/api/11.x/Illuminate/Cache/CacheManager.html)                                     | `cache`                     |
| Config                | [Illuminate\Config\Repository](https://laravel.com/api/11.x/Illuminate/Config/Repository.html)                                       | `config`                    |
| Context               | [Illuminate\Log\Context\Repository](https://laravel.com/api/11.x/Illuminate/Log/Context/Repository.html)                             |                             |
| Cookie                | [Illuminate\Cookie\CookieJar](https://laravel.com/api/11.x/Illuminate/Cookie/CookieJar.html)                                         | `cookie`                    |
| Crypt                 | [Illuminate\Encryption\Encrypter](https://laravel.com/api/11.x/Illuminate/Encryption/Encrypter.html)                                 | `encrypter`                 |
| Date                  | [Illuminate\Support\DateFactory](https://laravel.com/api/11.x/Illuminate/Support/DateFactory.html)                                   | `date`                      |
| DB (Instance)         | [Illuminate\Database\Connection](https://laravel.com/api/11.x/Illuminate/Database/Connection.html)                                   | `db.connection`             |
| DB                    | [Illuminate\Database\DatabaseManager](https://laravel.com/api/11.x/Illuminate/Database/DatabaseManager.html)                         | `db`                        |
| Event                 | [Illuminate\Events\Dispatcher](https://laravel.com/api/11.x/Illuminate/Events/Dispatcher.html)                                       | `events`                    |
| Exceptions (Instance) | [Illuminate\Contracts\Debug\ExceptionHandler](https://laravel.com/api/11.x/Illuminate/Contracts/Debug/ExceptionHandler.html)         |                             |
| Exceptions            | [Illuminate\Foundation\Exceptions\Handler](https://laravel.com/api/11.x/Illuminate/Foundation/Exceptions/Handler.html)               |                             |
| File                  | [Illuminate\Filesystem\Filesystem](https://laravel.com/api/11.x/Illuminate/Filesystem/Filesystem.html)                               | `files`                     |
| Gate                  | [Illuminate\Contracts\Auth\Access\Gate](https://laravel.com/api/11.x/Illuminate/Contracts/Auth/Access/Gate.html)                     |                             |
| Hash                  | [Illuminate\Contracts\Hashing\Hasher](https://laravel.com/api/11.x/Illuminate/Contracts/Hashing/Hasher.html)                         | `hash`                      |
| Http                  | [Illuminate\Http\Client\Factory](https://laravel.com/api/11.x/Illuminate/Http/Client/Factory.html)                                   |                             |
| Lang                  | [Illuminate\Translation\Translator](https://laravel.com/api/11.x/Illuminate/Translation/Translator.html)                             | `translator`                |
| Log                   | [Illuminate\Log\LogManager](https://laravel.com/api/11.x/Illuminate/Log/LogManager.html)                                             | `log`                       |
| Mail                  | [Illuminate\Mail\Mailer](https://laravel.com/api/11.x/Illuminate/Mail/Mailer.html)                                                   | `mailer`                    |
| Notification          | [Illuminate\Notifications\ChannelManager](https://laravel.com/api/11.x/Illuminate/Notifications/ChannelManager.html)                 |                             |
| Password (Instance)   | [Illuminate\Auth\Passwords\PasswordBroker](https://laravel.com/api/11.x/Illuminate/Auth/Passwords/PasswordBroker.html)               | `auth.password.broker`      |
| Password              | [Illuminate\Auth\Passwords\PasswordBrokerManager](https://laravel.com/api/11.x/Illuminate/Auth/Passwords/PasswordBrokerManager.html) | `auth.password`             |
| Pipeline (Instance)   | [Illuminate\Pipeline\Pipeline](https://laravel.com/api/11.x/Illuminate/Pipeline/Pipeline.html)                                       |                             |
| Process               | [Illuminate\Process\Factory](https://laravel.com/api/11.x/Illuminate/Process/Factory.html)                                           |                             |
| Queue (Base Class)    | [Illuminate\Queue\Queue](https://laravel.com/api/11.x/Illuminate/Queue/Queue.html)                                                   |                             |
| Queue (Instance)      | [Illuminate\Contracts\Queue\Queue](https://laravel.com/api/11.x/Illuminate/Contracts/Queue/Queue.html)                               | `queue.connection`          |
| Queue                 | [Illuminate\Queue\QueueManager](https://laravel.com/api/11.x/Illuminate/Queue/QueueManager.html)                                     | `queue`                     |
| RateLimiter           | [Illuminate\Cache\RateLimiter](https://laravel.com/api/11.x/Illuminate/Cache/RateLimiter.html)                                       |                             |
| Redirect              | [Illuminate\Routing\Redirector](https://laravel.com/api/11.x/Illuminate/Routing/Redirector.html)                                     | `redirect`                  |
| Redis (Instance)      | [Illuminate\Redis\Connections\Connection](https://laravel.com/api/11.x/Illuminate/Redis/Connections/Connection.html)                 | `redis.connection`          |
| Redis                 | [Illuminate\Redis\RedisManager](https://laravel.com/api/11.x/Illuminate/Redis/RedisManager.html)                                     | `redis`                     |
| Request               | [Illuminate\Http\Request](https://laravel.com/api/11.x/Illuminate/Http/Request.html)                                                 | `request`                   |
| Response (Instance)   | [Illuminate\Http\Response](https://laravel.com/api/11.x/Illuminate/Http/Response.html)                                               |                             |
| Response              | [Illuminate\Contracts\Routing\ResponseFactory](https://laravel.com/api/11.x/Illuminate/Contracts/Routing/ResponseFactory.html)       |                             |
| Route                 | [Illuminate\Routing\Router](https://laravel.com/api/11.x/Illuminate/Routing/Router.html)                                             | `router`                    |
| Schedule              | [Illuminate\Console\Scheduling\Schedule](https://laravel.com/api/11.x/Illuminate/Console/Scheduling/Schedule.html)                   |                             |
| Schema                | [Illuminate\Database\Schema\Builder](https://laravel.com/api/11.x/Illuminate/Database/Schema/Builder.html)                           |                             |
| Session (Instance)    | [Illuminate\Session\Store](https://laravel.com/api/11.x/Illuminate/Session/Store.html)                                               | `session.store`             |
| Session               | [Illuminate\Session\SessionManager](https://laravel.com/api/11.x/Illuminate/Session/SessionManager.html)                             | `session`                   |
| Storage (Instance)    | [Illuminate\Contracts\Filesystem\Filesystem](https://laravel.com/api/11.x/Illuminate/Contracts/Filesystem/Filesystem.html)           | `filesystem.disk`           |
| Storage               | [Illuminate\Filesystem\FilesystemManager](https://laravel.com/api/11.x/Illuminate/Filesystem/FilesystemManager.html)                 | `filesystem`                |
| URL                   | [Illuminate\Routing\UrlGenerator](https://laravel.com/api/11.x/Illuminate/Routing/UrlGenerator.html)                                 | `url`                       |
| Validator (Instance)  | [Illuminate\Validation\Validator](https://laravel.com/api/11.x/Illuminate/Validation/Validator.html)                                 |                             |
| Validator             | [Illuminate\Validation\Factory](https://laravel.com/api/11.x/Illuminate/Validation/Factory.html)                                     | `validator`                 |
| View (Instance)       | [Illuminate\View\View](https://laravel.com/api/11.x/Illuminate/View/View.html)                                                       |                             |
| View                  | [Illuminate\View\Factory](https://laravel.com/api/11.x/Illuminate/View/Factory.html)                                                 | `view`                      |
| Vite                  | [Illuminate\Foundation\Vite](https://laravel.com/api/11.x/Illuminate/Foundation/Vite.html)                                           |                             |