# Deployment
***
## Basic
Системные требования Laravel.
- PHP >= 8.2
- Расширение PHP Ctype
- Расширение PHP cURL
- Расширение PHP DOM
- Расширение PHP Fileinfo
- Расширение PHP Filter
- Расширение PHP Hash
- Расширение PHP Mbstring
- Расширение PHP OpenSSL
- Расширение PHP PCRE
- Расширение PHP PDO
- Расширение PHP Session
- Расширение PHP Tokenizer
- Расширение PHP XML
***
## Server Configuration
### Nginx
Веб-сервер должен направлять все запросы в файл `public/index.php`
```nginx
# Пример конфигурационного файла Nginx

server {
    listen 80;
    listen [::]:80;
    server_name example.com;
    root /srv/example.com/public;

    add_header X-Frame-Options "SAMEORIGIN";
    add_header X-Content-Type-Options "nosniff";

    index index.php;

    charset utf-8;

    location / {
        try_files $uri $uri/ /index.php?$query_string;
    }

    location = /favicon.ico { access_log off; log_not_found off; }
    location = /robots.txt  { access_log off; log_not_found off; }

    error_page 404 /index.php;

    location ~ \.php$ {
        fastcgi_pass unix:/var/run/php/php8.2-fpm.sock;
        fastcgi_param SCRIPT_FILENAME $realpath_root$fastcgi_script_name;
        include fastcgi_params;
        fastcgi_hide_header X-Powered-By;
    }

    location ~ /\.(?!well-known).* {
        deny all;
    }
}
```
_**Никогда не перемешать файл `index.php` в корень проекта, поскольку обслуживание приложения из корня проекта откроет доступ ко многим конфиденциальным файлам конфигурации из общедоступной сети Интернет**_
### FrankenPHP
**FrankenPHP** — это современный сервер приложений PHP, написанный на Go.
- `frankenphp php-server -r public/` - команда для запуска серваера FrankenPHP.
_**Laravel потребуется разрешение на запись в каталоги `bootstrap/cache` и `storage`**_
***
## Optimization
При развертывании приложения в рабочей среде необходимо кэшировать различные файлы, включая конфигурацию, события, маршруты и представления.
- `php artisan optimize` - команда для кеширования всех файлов.
- `php artisan optimize:clear` - команда для удаления всех файлов кэша.
### Caching Configuration
- `php artisan config:cache` - комнада для кэширования файлов конфигурации.
Команда объединит все файлы конфигурации Laravel в один кешированный файл, что значительно сократит количество обращений, которые фреймворк должен совершить к файловой системе при загрузке значений конфигурации.
_**При выполнении команды `config:cache` в процессе развертывания, необходимо вызывать функцию `env` только из файлов конфигурации. После кеширования конфигурации файл `.env` не будет загружаться, и все вызовы функции `env` для переменных файла `.env` вернут `null`.**_
### Caching Events
- `php artisan event:cache` - команда для кэширования событий. 
### Caching Routes
- `php artisan route:cache` - команда для кэширования маршрутов.
Эта команда сокращает регистрации всех маршрутов до одного вызова метода в кешированном файле, повышая производительность при регистрации сотен маршрутов.
### Caching Views
- `php artisan view:cache` - команда для кэширования представленний.
Эта команда предварительно скомпилирует все шаблоны Blade, чтобы они не компилировались во время запроса, повышая производительность каждого запроса, возвращающего шаблоном.
***
## Debug Mode
Параметр отладки в файле конфигурации `config/app.php` определяет, сколько информации об ошибке фактически отображается пользователю. По умолчанию для этого параметра задано значение переменной среды `APP_DEBUG`, которая хранится в файле `.env`.
_**В эксплуатационном окружении (Production) это значение всегда должно быть `false`. Если значение для переменной `APP_DEBUG` установлено как `true`, то есть риск раскрыть конфиденциальные значения конфигурации конечным пользователям вашего приложения.**_
***
## The Health Route
Laravel включает встроенный маршрут проверки работоспособности, который можно использовать для отслеживания статуса вашего приложения. В проде этот маршрут можно использовать для сообщения о состоянии вашего приложения монитору работоспособности, балансировщику нагрузки или системе оркестрации.
По умолчанию маршрут проверки работоспособности обслуживается по адресу `/up` и возвращает HTTP-ответ 200, если приложение загрузилось без исключений. В противном случае будет возвращен HTTP-ответ 500. Настроить URI для этого маршрута в файле `bootstrap/app`.
```php
/* Пример конфигурации Health Route */

->withRouting(
    web: __DIR__.'/../routes/web.php',
    commands: __DIR__.'/../routes/console.php',
    health: '/up', // [tl! remove]
    health: '/status', // [tl! add]
)
```
Когда HTTP-запросы отправляются по этому маршруту, Laravel также отправляет событие `Illuminate\Foundation\Events\DiagnosingHealth`, позволяя выполнять дополнительные проверки работоспособности.
***
