# Commands
***
## Basic
### Common Commands
``` bash
  docker run         # Создайте и запустите новый контейнер из образа.
  docker exec        # Выполните команду в запущенном контейнере.
  docker ps          # Список запущенных контейнеров.
  docker build       # Создайте образ из файла Dockerfile.
  docker pull        # Загрузка образа из реестра.
  docker push        # Загрузить образ в реестр.
  docker images      # Список образов.
  docker login       # Пройти аутентификацию в реестре.
  docker logout      # Выйдите из реестра.
  docker search      # Поиск образа в Docker Hub.
  docker version     # Выводит информацию о версии Docker.
  docker info        # Вывод общесистемной информации.
```
### Management Commands
``` bash
  docker builder     # Manage builds
  docker compose*    # Manage Docker Compose
  docker container   # Manage containers
  docker context     # Manage contexts
  docker image       # Manage images
  docker manifest    # Manage Docker image manifests and manifest lists
  docker network     # Manage networks
  docker plugin      # Manage plugins
  docker system      # Manage Docker
  docker trust       # Manage trust on Docker images
  docker volume      # Manage volumes
  docker swarm       # Manage Swarm
```
### Commands
``` bash
  docker attach      # Подключает локальные стандартные потоки ввода, вывода и ошибок к запущенному контейнеру.
  docker commit      # Создает новый образ на основе изменений, внесенных в контейнер.
  docker cp          # Копирование файлов/папок между контейнером и локальной файловой системой
  docker create      # Создает новый контейнер.
  docker diff        # Проверяет изменения в файлах или каталогах файловой системы контейнера.
  docker events      # Получает события в режиме реального времени с сервера.
  docker export      # Экспорт файловой системы контейнера в виде tar-архива
  docker history     # Выводит историю образа.
  docker import      # Импортирует содержимое из архиватора, чтобы создать образ.
  docker inspect     # Возвращает низкоуровневую информацию об объектах Docker.
  docker kill        # Убивает один или несколько запущенных контейнеров.
  docker load        # Загружает образ из архива tar или стандартного ввода-вывода
  docker logs        # Извлекает логи из контейнера.
  docker pause       # Останавливает все процессы в одном или нескольких контейнерах.
  docker port        # Выводит сопоставления портов или конкретное сопоставление для контейнера.
  docker rename      # Переименовывает контейнер.
  docker restart     # Перезапускает один и более контейнеров.
  docker rm          # Удаляет один или несколько контейнеров.
  docker rmi         # Удаляет один или несколько образов.
  docker save        # Сохраняет один или несколько образов в архив tar (по умолчанию они передаются в стандартный вывод).
  docker start       # Запускает один или несколько остановленных контейнеров.
  docker stats       # Выводит статистику использования ресурсов контейнером (контейнерами) в режиме реального времени.
  docker stop        # Останавливает один или несколько остановленных контейнеров.
  docker tag         # Создает тег TARGET_IMAGE, который ссылается на SOURCE_IMAGE.
  docker top         # Выводит список запущенных процессов внутри контейнера.
  docker unpause     # Возобновить все процессы в одном или нескольких контейнерах.
  docker update      # Обновляет конфигурацию одного или нескольких контейнеров.
  docker wait        # Блокирует контейнер до тех пор, пока один или несколько контейнеров не остановятся, затем выведите их коды выхода.
```
***
## Options
``` bash
      --config string      # Путь до конфигуционного файла (default "/home/potter/.docker").
  -c, --context string     # Имя контекста, используемого для подключения к демону (переопределяет переменную DOCKER_HOST env и контекст по умолчанию, установленный с помощью "docker context use".)
  -D, --debug              # Включить режим отладки.
  -H, --host list          # Демои сокет для подключения.
  -l, --log-level string   # Устанавливает уровень логирования ("debug", "info", "warn", "error", "fatal") (default "info")
      --tls                # Использовать TLS; подразумевается --tlsverify
      --tlscacert string   # Сертификаты доверия, подписанные только этим центром сертификации (default "/home/potter/.docker/ca.pem")
      --tlscert string     # Путь к файлу сертификата TLS (default "/home/potter/.docker/cert.pem")
      --tlskey string      # Путь к ключевому файлу TLS (default "/home/potter/.docker/key.pem")
      --tlsverify          # Используйте TLS и проверьте удаленный сертификат.
  -v, --version            # Выводит информацию о версии и завершает работу.
```
***
## Command list
### Basic commands
- `docker rm [container_id]` - команда удаляет указанный контейнер с локального устройства.
- `docker rmi [image_id|container_id]` - удаляет образ и контейнер с диска.
- `docker rmi -f $(docker images -a -q)` - скрипт для удаления всех образов на устройстве.
- `docker system prune -a` - скрипт для удаления всех образов, контейнеров и сетей с устройства.
- `docker run hello-world` - запускает образ и создает контейнер Hello World. Для проверки после установки.
- `docker run -p 8080:8080 -v [path to local code]:/app[path to container code] -w /app[create file/directory] [container_name] [command]`
- `docker run -d [image_name] [command = ''] [parameters = '']` - запуск указанного образа  в отвязке от консоли с созданием контейнера. (detached mode).
- `docker start [container_id]` - повторный запуск созданного контейнера.
- `docker ps` - выводит список запущенных контейнеров.
- `docker ps -a` - показывает все контейнеры и историю запусков.
- `docker run -d --name=[conatainer name]` - задает имя контейнера. Можно использовать вместо id.
- `docker start [container_name|container_id]` - запускает созданный контейнер в detach.
- `docker pause [container_name|container_id]` - останавливает работу указанного контейнера.
- `docker unpause [container_name|container_id]` - восстанавливает работу указанного остановленного контейнера.
- `docker stop [container_name|container_id]` - выключает указанный контейнер `SIGTERM`.
- `docker kill [container_name|container_id]` - выключает указанный контейнер `SIGKILL`.
- `docker stats [container_id]` - выводит данные об указанном контейнере.
- `docker exec -it [container_name] [command]` - запускает указанную команду в указанном контейнере.
- `docker search [image_name]` - поиск образа в глобальной сети.
- `docker pull [image_name]:[tag = latest]` - загуржает указанный образ из сети.
### Image commands
- `docker image -h` - выводит в консоль справку по командам образов Docker.
- `docker image build [path_to_dockerfile]|docker build|docker builder build` - собирает image из указанного Dockerfile.
- `docker image history [image_name|image_id]|docker history [image_name|image_id]` - показывает историю образа.
- `docker image inspect [image_name|image_id]` - выводит подробную информацию образа.
- `docker images|docker image -ls|docker image list` - просмотр всех образов на локальном устройстве.
- `docker image -ls -a` - список всех образов на хосте (в том числе промежуточных).
- `docker image import [path_to_file|url]|docker import [path_to_file|url]` - команда импортирует image из указанного источника.
- `docker image load [path_to_tar]|docker load [path_to_tar]` - звгружает образ из `tar` архива или из `STDIN`.
- `docker image prune` - удаляет неиспользуемые образы с хоста.
- `docker image prune -f` - удаляет неиспользуемые образы с хоста без запроса подтверждения.
- `docker image pull [image_name]` - загружает образ из репозитория.
- `docker image push [image_name]` - загружает образ в репозиторий.
- `docker image rm[image_name|image_id]|docker image remove[image_name|image_id]| docker rmi [image_name|image_id]` - удаляет указанный образ с хоста.
- `docker image rm[image_name|image_id] -f` - удаляет указанный образ с хоста без запроса подтверждения.
- `docker image save [image_name|image_id]|docker save [image_name|image_id]` - команда сохраняет образ в `tar` архив.
- `docker image tag[source_image[:tag] target_image[:tag]]|docker tag [source_image[:tag] target_image[:tag]]` - добавляет тэг образу.
### Container commands
- `docker container list` - выводит список всех запущенных контейнеров.
- `docker container stats` - мониторинг использования ресурсов контейнерами.
- `docker container top [container_name|container_id]` - просмотр процессов контейнера. Запуск команды `top`.
- `docker container port [container_name]` - выводит список портов указанного контейнера.
- `docker container stop [container_name|container_id]` - остановка работающего контейнера. 
- `docker container logs [container_name]|docker logs [-f] [container_id]` - выводит в консоль лог контейнера.
- `docker container rename [container_name] [new_container_name]` - переименовывает контейнер.
### Service commands
- `docker service logs [service_name]` - выводит в консоль лог сервиса.
- `docker attach` - подключиться к контейнеру.
### Network commands
- `docker network -h` - выводит справку по сетевым командам.
- `docker network ls` - выводит список сетей.
- `docker network inspect [network_name|network_id]` - выводит конфигурацию указанной сети.
- `docker network create [new_network_name]` - команда создает новую сеть.
- `docker network create --subnet [ip_address:port] --gateway [ip_address:port] [new_network_name]` - создает новую сеть с подсетью и шлюзом.
- `docker network connect [network_name] [container_name]` - подключает контейнер к сети.
- `docker network disconnect [network_name] [container_name]` - подключает контейнер к сети.
- `docker network rm [networks_name|networks_id]` - удаляет указанные сети.
- `docker network prune` - удаляет все неиспользуемые сети.
### Context commands
- `docker context ls` - выводит список контекстов.
- `docker context inspect [context_name]` - выводит конфигурацию указанного контекста.
- `docker context export [context_name]` - экспортирует указанный контекст.
- `docker context import [context_name] [context_export_file]` - импортирует указанный контекст из файла.
- `docker tag` - смена тега у образа.
### Volume commands
- `docker volume -h` - выводит в консоль справку по томам.
- `docker volume ls` - выводит список всех томов хоста.
- `docker volume create -d [path_to_volume] [new_volume_name]` - команда создает том между контейнером и хостом.
- `docker volume inspect [volume_id|volume_name]` - выводит информацию о volume указанного контейнера.
- `docker volume rm [volume_id|volume_name]` - удаляет указанный том.
- 
### Build commands
- `docker build [dockerfile_path]` - собирает образ по `Dockerfile`.
- `docker build -t [image_tag] [dockerfile_path]` - собирает образ по `Dockerfile` и дает ему указанный тег.
## System commands
- `docker system df` - 
- `docker system prune` - удаляет все неиспользуемые данные.
***
