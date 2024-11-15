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
docker compose attach      # Attach local standard input, output, and error streams to a service's running container
docker compose build       # Build or rebuild services
docker compose config      # Parse, resolve and render compose file in canonical format
  cp          # Copy files/folders between a service container and the local filesystem
docker compose create      # Creates containers for a service
docker compose down        # Stop and remove containers, networks
docker compose events      # Receive real time events from containers
docker compose exec        # Execute a command in a running container
docker compose export      # Export a service container's filesystem as a tar archive
docker compose images      # List images used by the created containers
docker compose kill        # Force stop service containers
docker compose logs        # View output from containers
docker compose ls          # List running compose projects
docker compose pause       # Pause services
docker compose port        # Print the public port for a port binding
docker compose ps          # List containers
docker compose pull        # Pull service images
docker compose push        # Push service images
docker compose restart     # Restart service containers
docker compose rm          # Removes stopped service containers
docker compose run         # Run a one-off command on a service
docker compose scale       # Scale services 
docker compose start       # Start services
docker compose stats       # Display a live stream of container(s) resource usage statistics
docker compose stop        # Stop services
docker compose top         # Display the running processes
docker compose unpause     # Unpause services
docker compose up          # Create and start containers
docker compose version     # Show the Docker Compose version information
docker compose wait        # Block until containers of all (or specified) services stop.
docker compose watch       # Watch build context for service and rebuild/refresh containers when files are updated
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
