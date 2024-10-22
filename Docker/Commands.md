# Commands
***
- `docker rmi [image_id]` - удаляет образ и контейнер с диска.
- `docker rmi -f $(docker images -a -q)` - скрипт для удаления всех образов на устройстве.
- `docker run -p 8080:8080 -v [path to local code]:/app[path to container code] -w /app[create file/directory] [container_name] [command]`
- `docker run -d` - запуск image в отвязке от консоли.
- `docker ps` - запущенные контейнеры.
- `docker ps -a` - показывает все контейнеры и историю запусков.
- `docker run -d --name=[conatainer name]` - задает имя контейнера. Можно использовать вместо id.
- `docker start [container_name]` - запускает созданный контейнер в detach.
- `docker stop [comtainer_id]` - останавливает рабочий контейнер.
- `docker images`|`docker image -ls`|`docker image list` - просмотр всех images на устройстве.
- `docker container list` - выводит список всех запущенных контейнеров.
- `docker container stats` - мониторинг использования ресурсов контейнерами.
- `docker container rename [container_name] [new_container_name]` - переименовывает контейнер.
- `docker system prune` - удаляет все скачанные контейнеры.
- `docker attach` - подключиться к контейнеру.
- `docker network ls` - выводит список сетей.
- `docker context ls` - выводит список контекстов.
- `docker context inspect [context_name]` - выводит конфигурацию указанного контекста.
- `docker context export [context_name]` - экспортирует указанный контекст.
- `docker context import [context_name] [context_export_file]` - импортирует указанный контекст из файла.
- `docker tag` - смена тега у образа.
- `docker image inspect` - log образа.
- `docker logs [-f] [container_id]` - получает и выводит журнал работы контейнера.
- `docker volume` - 
- `docker volume inspect [container_id]` - выводит информацию о volume указанного контейнера.
- `docker exec -it [container_name] [command]` - запускает указанную команду в указанном контейнере.
***
