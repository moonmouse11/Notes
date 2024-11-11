# Commands
***
## Basic
- `composer install` - устанавливает зависимости в проекте.
- `composer install --no-dev -o` - устанавливает только production зависимости и кэширует их.
- `composer update` - обновляет пакеты из файла `composer.json` в допустимых пределах.
- `composer update [package_name]` - обновляет один, заданный пакет.
- `composer require [package_name]` - устанавливает заданный пакет.
- `composer require [package_name] --dev` - устанавливает заданный пакет только для DEV окружения.
- `composer remove [package_name]` - удаляет заданный пакет.
- `composer dump-autoload` - обновляет подгрузку классов.
- `composer dump-autoload -o` - обновляет подгрузку классов с оптимизацией.
- `composer create-project [package_name] [project_name]` - создает новый проект с инициализацией и подгрузкой указанного пакета.
- `composer audit` - команда ищет уязвимости в проекте.

``` bash
composer about                # Выводит краткую информацию о Composer.
composer archive              # Создает архив указаного пакета.
composer audit                # Проверяет уязвимости установленных пакетов.
composer browse               # [home] Opens the package's repository URL or homepage in your browser
composer bump                 # Increases the lower limit of your composer.json requirements to the currently installed versions
composer check-platform-reqs  # Check that platform requirements are satisfied
composer clear-cache          # [clearcache|cc] Clears composer's internal package cache
composer completion           # Dump the shell completion script
composer config               # Sets config options
composer create-project       # Creates new project from a package into given directory
composer depends              # [why] Shows which packages cause the given package to be installed
composer diagnose             # Diagnoses the system to identify common errors
composer dump-autoload        # [dumpautoload] Dumps the autoloader
composer exec                 # Executes a vendored binary/script
composer fund                 # Discover how to help fund the maintenance of your dependencies
composer global               # Allows running commands in the global composer dir ($COMPOSER_HOME)
composer help                 # Display help for a command
composer init                 # Creates a basic composer.json file in current directory
composer install              # [i] Installs the project dependencies from the composer.lock file if present, or falls back on the composer.json
composer licenses             # Shows information about licenses of dependencies
composer list                 # List commands
composer outdated             # Shows a list of installed packages that have updates available, including their latest version
composer prohibits            # [why-not] Shows which packages prevent the given package from being installed
composer reinstall            # Uninstalls and reinstalls the given package names
composer remove               # [rm|uninstall] Removes a package from the require or require-dev
composer require              # [r] Adds required packages to your composer.json and installs them
composer run-script           # [run] Runs the scripts defined in composer.json
composer search               # Searches for packages
composer self-update          # [selfupdate] Updates composer.phar to the latest version
composer show                 # [info] Shows information about packages
composer status               # Shows a list of locally modified packages
composer suggests             # Shows package suggestions
composer update               # [u|upgrade] Updates your dependencies to the latest version according to composer.json, and updates the composer.lock file
composer validate             # Validates a composer.json and composer.lock
```
***
## Options
``` bash
  -h, --help                     Display help for the given command. When no command is given display help for the list command
  -q, --quiet                    Do not output any message
  -V, --version                  Display this application version
      --ansi|--no-ansi           Force (or disable --no-ansi) ANSI output
  -n, --no-interaction           Do not ask any interactive question
      --profile                  Display timing and memory usage information
      --no-plugins               Whether to disable plugins.
      --no-scripts               Skips the execution of all scripts defined in composer.json file.
  -d, --working-dir=WORKING-DIR  If specified, use the given directory as working directory.
      --no-cache                 Prevent use of the cache
  -v|vv|vvv, --verbose           Increase the verbosity of messages: 1 for normal output, 2 for more verbose output and 3 for debug
``` 
***
