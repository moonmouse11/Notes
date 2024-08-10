# Commands
***
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
- 