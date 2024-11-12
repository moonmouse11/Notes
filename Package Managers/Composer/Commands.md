# Commands
***
## Basic
- `composer install` - устанавливает зависимости в проекте.
- `composer install --no-dev -o` - устанавливает только production зависимости и кэширует их.
- `composer update` - обновляет пакеты из файла `composer.json` в допустимых пределах.
- `composer update [package_name]` - обновляет один, заданный пакет.
- `composer require [package_name]` - устанавливает заданный пакет.
- `composer require [package_name] --dev` - устанавливает заданный пакет только для `DEV`(development) окружения.
- `composer remove [package_name]` - удаляет заданный пакет.
- `composer dump-autoload` - обновляет подгрузку классов.
- `composer dump-autoload -o` - обновляет подгрузку классов с оптимизацией.
- `composer create-project [package_name] [project_name]` - создает новый проект с инициализацией и подгрузкой указанного пакета.
- `composer audit` - команда ищет уязвимости в проекте.

``` bash
composer about                # Выводит краткую информацию о Composer.
composer archive              # Создает архив указаного пакета.
composer audit                # Проверяет уязвимости установленных пакетов.
composer browse               # [home] Открывает URL-адрес хранилища пакета или домашнюю страницу в вашем браузере.
composer bump                 # Увеличивает нижний предел ваших требований к composer.json по сравнению с текущими установленными версиями/
composer check-platform-reqs  # Проверяет, что требования к платформе выполнены.
composer clear-cache          # [clearcache|cc] Очищает внутренний кэш пакетов composer.
composer completion           # Выгружает сценарий завершения оболочки.
composer config               # Устанавливает параметры конфигурации.
composer create-project       # Создает новый проект из пакета в заданной директории
composer depends              # [why] Показывает зависимости пакета.
composer diagnose             # Диагностирует систему для выявления распространенных ошибок
composer dump-autoload        # [dumpautoload] Сбрасывает автозагрузчик.
composer exec                 # Запускает скрипт из vendor
composer fund                 # Как задонатить разработчикам пакетом.
composer global               # Позволяет запускать команды в глобальном каталоге composer ($COMPOSER_HOME).
composer help                 # Выводит помощь по команде
composer init                 # Создает базовый файл composer.json в текущую директорию.
composer install              # [i] Устанавливает зависимости проекта из composer.lock, если он присутствует, или устанавливает из composer.json.
composer licenses             # Выводит лицензии зависимостей проекта.
composer list                 # Список комманд composer.
composer outdated             # Показывает список установленных пакетов, для которых доступны обновления, включая их последнюю версию.
composer prohibits            # [why-not] Показывает, какие пакеты препятствуют установке данного пакета.
composer reinstall            # Удаляет и переустанавливает указанные пакеты.
composer remove               # [rm|uninstall] Удаляет пакет из проекта.
composer require              # [r] Устанавливает указанный пакет в проект.
composer run-script           # [run] Запускает скрипты, определенные в  composer.json.
composer search               # Поиск пакетов composer.
composer self-update          # [selfupdate] Обновляет composer.phar до последней версии.
composer show                 # [info] Выводит информаию о пакете.
composer status               # Показывает список локально измененных пакетов.
composer suggests             # Показывает предложения по пакетам.
composer update               # [u|upgrade] Обновляет ваши зависимости до последней версии в соответствии с composer.json и обновляет composer.lock.
composer validate             # Проверяет composer.json и composer.lock
```
***
## Options
``` bash
  -h, --help                     # Выводит мануал по указанной команде.
  -q, --quiet                    # Не выводит сообщений.
  -V, --version                  # Выводит версию Composer.
      --ansi|--no-ansi           # Управление выводом ANSI.
  -n, --no-interaction           # Не задает проверочных вопросов.
      --profile                  # Отображение информации о времени и использовании памяти.
      --no-plugins               # Отключение плагинов.
      --no-scripts               # Пропускает выполнение всех скриптов, определенных в файле composer.json.
  -d, --working-dir=WORKING-DIR  # Использует указанную директорию в качестве рабочей.
      --no-cache                 # Запрещает использовение кэша.
  -v|vv|vvv, --verbose           # Увеличивает детализацию сообщений: 1 для обычного вывода, 2 для более подробного вывода и 3 для отладки.
``` 
***
