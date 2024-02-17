# Basics
***
## General
В проекте используются два файла
- `composer.json` - основной файл с зависимостями.
- `composer.lock` - файл с фиксированными зависимостями.
***
## Commands
- `composer install` - устанавливает зависимости в проекте.
- `composer update` - обновляет пакеты из файла `composer.json` в допустимых пределах.
- `composer update [package_name]` - обновляет один, заданный пакет.
- `composer require [package_name]` - устанавливает заданный пакет.
- `composer require [package_name] --dev` - устанавливает заданный пакет только для DEV окружения.
- `composer remove [package_name]` - удаляет заданный пакет.
- `composer dump-autoload` - обновляет подгрузку классов.
- `composer create-project [package_name] [project_name]` - создает новый проект с инициализацией и подгрузкой указанного пакета.
***
