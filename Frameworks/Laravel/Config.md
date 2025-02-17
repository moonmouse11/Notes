# Config
***
## Basic
Все конфигурационные файлы фреймворка Laravel хранятся в каталоге `config`.
Конфигурационные файлы позволяют настраивать такие вещи, как информация о подключении к базе данных, информация о почтовом сервере, а также другие основные параметры, например, часовой пояс приложения и ключ шифрования.
- `php artisan about` - выводит параметры конфигурации приложения.
- `php artisan about --only=environment` - выводит указанный параметр конфигурации.
- `php artisan config:show database` - выводит параметры указанного раздела.
***
## Environment Configuration
Laravel использует библиотеку `DotEnv` PHP. В корневом каталоге приложения будет содержаться файл `.env.example`, определяющий множество основных переменных окружения. Этот файл будет автоматически скопирован в `.env` в процессе установки Laravel.
Перед загрузкой переменных окружения приложения Laravel определяет, была ли переменная среды `APP_ENV` предоставлена извне или указан аргумент CLI `--env`. Если это так, Laravel попытается загрузить файл `.env.[APP_ENV]`. Если он не существует, будет загружен `.env` файл по умолчанию.
### Environment Variable Types
Все переменные в файлах `.env` обычно анализируются как строки, поэтому были созданы некоторые зарезервированные значения, позволяющие вам возвращать более широкий диапазон типов из функции `env()`

| Значение `.env` | Значение `env()` |
| --------------- | ---------------- |
| true            | (bool) true      |
| (true)          | (bool) true      |
| false           | (bool) false     |
| (false)         | (bool) false     |
| empty           | (string) ''      |
| (empty)         | (string) ''      |
| null            | (null) null      |
| (null)          | (null) null      |
***
## Retrieving Environment Configuration
Все переменные, перечисленные в этом файле, будут загружены в суперглобальную переменную `$_ENV` PHP. 
-  `env()` - хелпер для получения значений из переменных конфигурационных файлов.
```php
/* Пример использования функции env */

'debug' => env('APP_DEBUG', false),
```
***
## Determining the Current Environment
Текущее окружение приложения определяется с помощью переменной `APP_ENV` из файла `.env`.
```php
/* Пример получения переменных окружения фасадом App */

use Illuminate\Support\Facades\App;

$environment = App::environment();
```

Можно передать аргументы методу `environment`, чтобы определить, соответствует ли окружение переданному значению. Метод вернет `true`, если окружение соответствует любому из указанных значений.
```php
/* Пример получения переменных окружения фасадом App */

if (App::environment('local')) {
    // Локальное окружение ...
}

if (App::environment(['local', 'staging'])) {
    // Окружение либо локальное, либо промежуточное ...
}
```
***
## Encrypting Environment Files
- `php artisan env:encrypt` - команда для шифрования конфига.
Запуск команды `env:encrypt` зашифрует ваш файл `.env` и поместит зашифрованное содержимое в файл `.env.encrypted`. Ключ для расшифровки будет представлен в выводе команды и должен храниться в безопасном менеджере паролей.
- `php artisan env:encrypt --key=3UVsEgGVK36XN82KKeyLFMhvosbZN1aF` - шифрует файл `.env` по указанному ключу.
Длина указанного ключа должна соответствовать длине ключа, требуемой используемым шифром шифрования. По умолчанию Laravel использует шифр `AES-256-CBC`.
- `php artisan env:decrypt` - команда для расшифровки файла конфигурации. 
- `php artisan env:decrypt --key=3UVsEgGVK36XN82KKeyLFMhvosbZN1aF` - команда для расшифровки с указанным ключом.
- `php artisan env:decrypt --force` - команда переписывает текущий `.env` файл.
***
## Accessing Configuration Values
Доступ к значениям конфигурации можно получить используя фасад `Config` или глобальную функцию `config` из любого места приложения.
Доступ к значениям конфигурации можно получить с помощью «точечной нотации», включающую имя файла и параметр, к которому вы хотите получить доступ. Также может быть указано значение по умолчанию, которое будет возвращено, если параметр конфигурации отсутствует:
```php
/* Пример получения конфига с помощью фасада */

use Illuminate\Support\Facades\Config;

$value = Config::get('app.timezone');

$value = config('app.timezone');

// Получить значение по умолчанию, если значение конфигурации не существует ...
$value = config('app.timezone', 'Asia/Seoul');
```
Установить значения конфигурации во время выполнения скрипта тоже можно фасадом `Config`.
Для облегчения статического анализа фасад `Config` также предоставляет методы получения типизированной конфигурации.
```php
/* Пример изменения конфигурации */

Config::set('app.timezone', 'America/Chicago');

config(['app.timezone' => 'America/Chicago']);

/* Пример изменения конфигурации с указанием типа данных */

Config::string('config-key');
Config::integer('config-key');
Config::float('config-key');
Config::boolean('config-key');
Config::array('config-key');
```
***
## Configuration Caching
Чтобы ускорить работу приложения, необходимо кешировать все конфигурационные файлы в один файл с помощью команды `config:cache` Artisan. Это объединит все конфигурационные параметры приложения в один файл, который может быть быстро загружен фреймворком.
- `php artisan config:cache` - команда для кэширования конфигурации.
После кэширования конфигурации файл `.env` не будет загружен фреймворком во время запросов или команд Artisan. Поэтому функция `env` будет возвращать только внешние переменные окружения на системном уровне.
Необходимо вызывать функцию `env` только из файлов конфигурации (`config`). После кэширования файл `.env` будет недоступен и функция `env()` будет возвращать только внешние переменные окружения на системном уровне. Значения конфигурации могут быть получены из любого места приложения с помощью функции `config`.
- `php artisan config:clear` - команда для очистки кэша параметров конфигурации.
***
## Configuration Publishing
Большинство файлов конфигурации Laravel уже опубликованы в каталоге `config` ; однако некоторые файлы конфигурации, такие как `cors.php` и `view.php`, не публикуются по умолчанию.
- `php artisan config:publish` - команда для публикации конфигурационного файла.
- `php artisan config:publish --all` - команда для публикации всех конфигурационных файлов.
***
## Debug Mode
Параметр `debug` в конфигурационном файле `config/app.php` определяет, сколько информации об ошибках фактически отображается конечному пользователю. По умолчанию этот параметр установлен с учетом значения переменной `APP_DEBUG` окружения, расположенной в вашем файле `.env`.
> Для локальной разработки вы должны установить для переменной `APP_DEBUG` окружения значение `true`. **В эксплуатационном режиме это значение всегда должно быть `false`. Если для этой переменной будет установлено значение `true`, то вы рискуете раскрыть конфиденциальные значения конфигурации конечным пользователям вашего приложения.**
***
## Maintenance Mode
Когда приложение находится в режиме обслуживания, то для всех запросов к приложению будет отображаться специальная страница. Это позволяет легко «отключить» ваше приложение во время его обновления или технического обслуживания. Проверка режима обслуживания включена в стек посредников по умолчанию. Если приложение находится в режиме обслуживания, то будет выброшено исключение `Symfony\Component\HttpKernel\Exception\HttpException` с `503` кодом состояния.
- `php artisan down` - команда для включения режима обслуживания.
- `php artisan down --refresh=15` - включения заглушки с таймером обновления.  Заголовок `Refresh`.
- `php artisan down --retry=60` - включения заглушки с таймером обновления.  Заголовок `Retry-After`.
### Bypassing Maintenance Mode
Находясь в режиме обслуживания, можно использовать параметр `secret`, чтобы указать токен для обхода режима обслуживания.
- `php artisan down --secret="1630542a-246b-4b66-afa1-dd72a4c43515"` - вклюючение режима заглушки с помощью токена `secret`.
После перевода приложения в режим обслуживания, можно перейти по URL-адресу приложения, с учетом этого токена, и Laravel выдаст браузеру куки для обхода режима обслуживания.
```shell
https://example.com/1630542a-246b-4b66-afa1-dd72a4c43515
```
> Параметр `secret` режима обслуживания должен состоять из буквенно-цифровых символов и, при необходимости, тире. Cледует избегать использования в URL-адресах символов, имеющих особое значение, таких как `?`или `&`.

### Maintenance Mode on Multiple Servers
По умолчанию Laravel определяет, находится ли приложение в режиме обслуживания, используя файловую систему. Это означает, что для активации режима обслуживания необходимо выполнить команду `php artisan down` на каждом сервере, на котором размещено приложение.
Альтернативно, Laravel предлагает метод на основе кеша для работы в режиме обслуживания. Этот метод требует запуска команды `php artisan down` только на одном сервере. Чтобы использовать этот подход, измените настройку `driver` в файле `config/app.php` вашего приложения на `cache`. Затем выберите хранилище кэша, доступное для всех ваших серверов. Это гарантирует, что статус режима обслуживания постоянно поддерживается на каждом сервере.
```php
/* Изменение настройки driver */

'maintenance' => [
    'driver' => 'cache',
    'store' => 'database',
],
```
### Pre-Rendering the Maintenance Mode View
Если вы используете команду `php artisan down` во время развертывания, то пользователи могут иногда сталкиваться с ошибками, если они обращаются к приложению во время обновления ваших зависимостей Composer или других компонентов фреймворка. Это происходит потому, для определения режима обслуживания и отображения шаблона режима обслуживания с помощью движка шаблонов должна быть загружена значительная часть фреймворка Laravel.
По этой причине Laravel позволяет в самом начале цикла запроса отобразить шаблон режима обслуживания. Этот шаблон отображается перед загрузкой любых зависимостей вашего приложения.
- `php artisan down --render="errors::503"` - команда для запуска обслуживания с указанным шаблоном.
### Redirecting Maintenance Mode Requests
В режиме обслуживания Laravel будет отображать шаблон режима обслуживания для всех URL-адресов приложения, к которым пользователь попытается получить доступ.
- `php artisan down --redirect=/` - включение режима обслуживания с редиректом.
### Disabling Maintenance Mode
- `php artisan up` - команда для отключения режима обслуживания Laravel.
### Maintenance Mode and Queues
> Пока приложение находится в режиме обслуживания, поставленные в очередь задания обрабатываться не будут. Задания продолжат обрабатываться в обычном режиме после выхода приложения из режима обслуживания.
### Alternatives to Maintenance Mode
Поскольку режим обслуживания требует, чтобы ваше приложение простаивало несколько секунд, то рассмотрите альтернативы, например, Laravel Vapor и Envoyer для выполнения развертывания с нулевым временем простоя.
***
