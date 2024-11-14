# Commands
***
## Basic
### Common Commands
``` bash
  docker run         # Create and run a new container from an image
  docker exec        # Execute a command in a running container
  docker ps          # List containers
  docker build       # Build an image from a Dockerfile
  docker pull        # Download an image from a registry
  docker push        # Upload an image to a registry
  docker images      # List images
  docker login       # Authenticate to a registry
  docker logout      # Log out from a registry
  docker search      # Search Docker Hub for images
  docker version     # Show the Docker version information
  docker info        # Display system-wide information
```
### Management Commands
``` bash
  docker builder     # Manage builds
  docker compose*    # Docker Compose
  docker container   # Manage containers
  docker context     # Manage contexts
  docker image       # Manage images
  docker manifest    # Manage Docker image manifests and manifest lists
  docker network     # Manage networks
  docker plugin      # Manage plugins
  docker system      # Manage Docker
  docker trust       # Manage trust on Docker images
  docker volume      # Manage volumes
```
### Swarm Commands
```bash
  docker swarm       # Manage Swarm
```
### Commands
``` bash
  docker attach      # Attach local standard input, output, and error streams to a running container
  docker commit      # Create a new image from a container's changes
  docker cp          # Copy files/folders between a container and the local filesystem
  docker create      # Create a new container
  docker diff        # Inspect changes to files or directories on a container's filesystem
  docker events      # Get real time events from the server
  docker export      # Export a container's filesystem as a tar archive
  docker history     # Show the history of an image
  docker import      # Import the contents from a tarball to create a filesystem image
  docker inspect     # Return low-level information on Docker objects
  docker kill        # Kill one or more running containers
  docker load        # Load an image from a tar archive or STDIN
  docker logs        # Fetch the logs of a container
  docker pause       # Pause all processes within one or more containers
  docker port        # List port mappings or a specific mapping for the container
  docker rename      # Rename a container
  docker restart     # Restart one or more containers
  docker rm          # Remove one or more containers
  docker rmi         # Remove one or more images
  docker save        # Save one or more images to a tar archive (streamed to STDOUT by default)
  docker start       # Start one or more stopped containers
  docker stats       # Display a live stream of container(s) resource usage statistics
  docker stop        # Stop one or more running containers
  docker tag         # Create a tag TARGET_IMAGE that refers to SOURCE_IMAGE
  docker top         # Display the running processes of a container
  docker unpause     # Unpause all processes within one or more containers
  docker update      # Update configuration of one or more containers
  docker wait        # Block until one or more containers stop, then print their exit codes
```
***
## Options
``` bash
      --config string      # Location of client config files (default "/home/potter/.docker")
  -c, --context string     # Name of the context to use to connect to the daemon (overrides DOCKER_HOST env var and default context set with "docker context use")
  -D, --debug              # Enable debug mode
  -H, --host list          # Daemon socket to connect to
  -l, --log-level string   # Set the logging level ("debug", "info", "warn", "error", "fatal") (default "info")
      --tls                # Use TLS; implied by --tlsverify
      --tlscacert string   # Trust certs signed only by this CA (default "/home/potter/.docker/ca.pem")
      --tlscert string     # Path to TLS certificate file (default "/home/potter/.docker/cert.pem")
      --tlskey string      # Path to TLS key file (default "/home/potter/.docker/key.pem")
      --tlsverify          # Use TLS and verify the remote
  -v, --version            # Print version information and quit
```
***
## Command list
### Basic commands
- `docker rm [container_id]` - команда удаляет указанный контейнер с локального устройства.
- `docker rmi [image_id|container_id]` - удаляет образ и контейнер с диска.
- `docker rmi -f $(docker images -a -q)` - скрипт для удаления всех образов на устройстве.
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
- `docker container port [container_name]` - выводит список портов указанного контейнера.
- `docker container logs [container_name]|docker logs [-f] [container_id]` - выводит в консоль лог контейнера.
- `docker container rename [container_name] [new_container_name]` - переименовывает контейнер.
### Service commands
- `docker service logs [service_name]` - выводит в консоль лог сервиса.
- `docker system prune` - удаляет все остановленные контейнеры.
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
- 
***
