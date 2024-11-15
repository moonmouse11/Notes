# Docker Service
***
## Basic

***
## Commands
``` bash
docker service create      # Create a new service
docker service inspect     # Display detailed information on one or more services
docker service logs        # Fetch the logs of a service or task
docker service ls          # List services
docker service ps          # List the tasks of one or more services
docker service rm          # Remove one or more services
docker service rollback    # Revert changes to a service's configuration
docker service scale       # Scale one or multiple replicated services
docker service update      # Update a service
```
***
## Command list
- `docker service -h` - справка по командам Docker Service.
- `docker service ps` - выводит список запущенных сервисов.
- `docker service logs [service_name]` - выводит в консоль лог сервиса.
- `docker service ls` - выводит список сервисов хоста.
- `docker service scale [service_name|service_id]` - 
- `docker service update [service_name|service_id]` - обновляет указанный сервис.
***
