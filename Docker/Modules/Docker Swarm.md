# Docker Swarm
***
## Basic

***
## Commands
### Swarm
``` bash
docker swarm init        # Инициализирует Docker Swarm
docker swarm join        # Join a swarm as a node and/or manager
```
### Node
``` bash
docker node demote      # Demote one or more nodes from manager in the swarm
docker node inspect     # Display detailed information on one or more nodes
docker node ls          # List nodes in the swarm
docker node promote     # Promote one or more nodes to manager in the swarm
docker node ps          # List tasks running on one or more nodes, defaults to current node
docker node rm          # Remove one or more nodes from the swarm
docker node update      # Update a node
```
***
## Command list
### Basic commands
- `docker swarm join --token SWMTKN-1-1r7nd8fglku1h10p785ertdg8inrvs3q0fr8ji2tyg9om1ra5b-14sdrdyeenqocvdp7zfvuabhx 192.168.31.194:2377`
- `docker swarm leave --force` - выход из узлов Docker Swarm.
### Node commands
- `docker node ls` - выводит список узлов.
- `docker node inspect` - выводит конфиг узла.
***
