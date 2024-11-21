# Config Structure
***
## Config Example
``` conf
user www www;
worker_processes 2;

error_log /var/log/nginx-error.log info;

events {
    use kqueue;
    worker_connections 2048;
}
```
***
## Directives
- `accept_mutex` - 

***
