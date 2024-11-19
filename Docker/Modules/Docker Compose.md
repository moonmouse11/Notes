# Docker Compose
***
## Basic
`docker-compose.yml` - исполняемый файл для запуска контейнеров.
###### Example
```yml
version: '3' // версия Docker Compose

services: // начало сервисов
  php: // название
    image: // контейнер
    command: // команда контенеру
    volumes: // пути файлов
    ports: // порты
    build: // сборка
      context: //
      dockerfile: // путь к Dockerfile
    working_dir: // указание рабочей директории контейнера
    enviroment: // окружение
    depends_on: // зависимость от другого контейнера
    container_name: // название контейнера
```

`docker compose up --build -d`
***
## Commands
``` bash
docker compose attach      # Подключает локальные стандартные потоки ввода, вывода и ошибок к запущенному контейнеру службы.
docker compose build       # Собирает или пересобирает сервис Compose.
docker compose config      # Вывод файла compose в yml формате.
docker compose cp          # Копирует файлы/папки между контейнером и локальной файловой системой.
docker compose create      # Создает контейнеры для сервиса.
docker compose down        # Останаливает и удаляет запущенные сети и контейнеры.
docker compose events      # Получать события в режиме реального времени из контейнеров.
docker compose exec        # Выполяет коменду в запущенном контейнере.
docker compose export      # Экспорт файловой системы сервисного контейнера в виде tar-архива.
docker compose images      # Список образов, используемы контейнерами.
docker compose kill        # Убивает процесс сервис-контейнера.
docker compose logs        # Выводит логов контейнера.
docker compose ls          # Список контейнеров.
docker compose pause       # Останавливает сервис.
docker compose port        # Выводит открытые порты сервиса.
docker compose ps          # Список запущенных контейнеров.
docker compose pull        # Загружает локально образы сервиса.
docker compose push        # Загружает в удаленный хаб образы сервиса.
docker compose restart     # Перезапускает сервис контейнеров.
docker compose rm          # Удаляет остановленные контейнеры.
docker compose run         # Запускет одноразовую команду для службы.
docker compose scale       # Масштабировение сервиса.
docker compose start       # Запуск сервис контейнерв.
docker compose stats       # Выводит статистику использования ресурсов.
docker compose stop        # Остановка сервис контейнера.
docker compose top         # Отображает запущенные процессы сервис контейнера.
docker compose unpause     # Возобнавляет работу сервиса.
docker compose up          # Создает и запускает сервис контейнер.
docker compose version     # Выводит версию Docker Compose.
docker compose wait        # Режим ожидания сервиса.
docker compose watch       # Выводит контекст сборки для обслуживания и обновления контейнера.
```
***
## Command list
- `docker compose up` - старт всех сервисов.
- `docker compose down` - остановка всех сервисов.
- `docker compose services` - список сервисов.
- `docker compose images` - образ для сборки контейнера.
- `docker compose build [path_to_dockerfile]` - собирает образ из Dockerfile.
- `docker compose build --no-cache` - собирает контейнеры заново, сбрасывая весь cache.
- `docker compose depends_on` - указание на последовательность запуска сервисов.
- `docker compose envoronment` - переменные окружения.
- `docker compose volumes` - маппинг папок и томов.
- `docker compose ports` - маппинг портов.
- `docker compose command` - команда для запуска внутри контейнера.
***
## Usage

***
