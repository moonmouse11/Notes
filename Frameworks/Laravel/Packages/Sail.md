# Sail
***
## Basics
_**Laravel Sail**_ - это инструмент командной строки для взаимодействия со средой разработки Docker. Sail обеспечивает отправную точку для создания приложения Laravel с использованием PHP, MySQL и Redis.
По сути, Sail – это файл `docker-compose.yml`, который хранится в корне вашего проекта и набор скриптов `sail`, при помощи которых можно управлять docker-контейнерами, определёнными в `docker-compose.yml`.
- `php artisan sail:install`
- `composer require laravel/sail --dev`
- `./vendor/bin/sail up`
- `alias sail='[ -f sail ] && sh sail || sh vendor/bin/sail'` - установка псевдонима в bash.
***
## Commands
``` bash
docker-compose Commands:
  sail up        Start the application
  sail up -d     Start the application in the background
  sail stop      Stop the application
  sail restart   Restart the application
  sail ps        Display the status of all containers

Artisan Commands:
  sail artisan ...          Run an Artisan command
  sail artisan queue:work

PHP Commands:
  sail php ...   Run a snippet of PHP code
  sail php -v

Composer Commands:
  sail composer ...                       Run a Composer command
  sail composer require laravel/sanctum

Node Commands:
  sail node ...         Run a Node command
  sail node --version

NPM Commands:
  sail npm ...        Run a npm command
  sail npx            Run a npx command
  sail npm run prod

PNPM Commands:
  sail pnpm ...        Run a pnpm command
  sail pnpx            Run a pnpx command
  sail pnpm run prod

Yarn Commands:
  sail yarn ...        Run a Yarn command
  sail yarn run prod

Bun Commands:
  sail bun ...        Run a bun command
  sail bunx           Run a bunx command
  sail bun run prod

Database Commands:
  sail mysql     Start a MySQL CLI session within the 'mysql' container
  sail mariadb   Start a MySQL CLI session within the 'mariadb' container
  sail psql      Start a PostgreSQL CLI session within the 'pgsql' container
  sail redis     Start a Redis CLI session within the 'redis' container

Debugging:
  sail debug ...          Run an Artisan command in debug mode
  sail debug queue:work

Running Tests:
  sail test          Run the PHPUnit tests via the Artisan test command
  sail phpunit ...   Run PHPUnit
  sail pest ...      Run Pest
  sail pint ...      Run Pint
  sail dusk          Run the Dusk tests (Requires the laravel/dusk package)
  sail dusk:fails    Re-run previously failed Dusk tests (Requires the laravel/dusk package)

Container CLI:
  sail shell        Start a shell session within the application container
  sail bash         Alias for 'sail shell'
  sail root-shell   Start a root shell session within the application container
  sail root-bash    Alias for 'sail root-shell'
  sail tinker       Start a new Laravel Tinker session

Sharing:
  sail share   Share the application publicly via a temporary URL
  sail open    Open the site in your browser

Binaries:
  sail bin ...   Run Composer binary scripts from the vendor/bin directory

Customization:
  sail artisan sail:publish   Publish the Sail configuration files
  sail build --no-cache       Rebuild all of the Sail containers
```
***
