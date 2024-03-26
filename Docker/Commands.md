# Commands
_**image - образ**_
***
- `docker rmi` - удаляет образ и контейнер с диска.
- `docker run -p 8080:8080 -v [path to local code]:/app[path to container code] -w /app[create file/directory] [container_name] [command]`
- `docker run -d` - запуск image в отвязке от консоли.
- `docker ps` - запущенные контейнеры.
- `docker ps -a` - показывает все контейнеры и историю запусков.
- `docker run -d --name=[conatainer name]` - задает имя контейнера. Можно использовать вместо id.
- `docker start [container_name]` - запускает созданный контейнер в detach.
- `docker stop [comtainer_id]` - останавливает рабочий контейнер.
- `docker images` `docker image -ls` - просмотр всех images на устройстве.
- `docker system prune` - удаляет все скачанные контейнеры.
- `docker attach` - подключиться к контейнеру.
- `docker tag` - смена тега у образа.
- `docker image inspect` - log образа.
- `docker logs [-f] [container_id]` - получает и выводит журнал работы контейнера.
- `docker volume` 
- `docker volume inspect` 
- `docker exec -it [container_name] [command]` - запускает указанную команду в указанном контейнере.