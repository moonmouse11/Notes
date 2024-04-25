# Tarantool
***
**Tarantool** позиционируется как быстрая резидентная база данных.
`tarantoolctl` — основной инструмент для управления экземплярами Tarantool;  
`/etc/tarantool` — директория для хранения всей конфигурации;  
`/var/log/tarantool` — директория для хранения журналов;  
`/var/lib/tarantool`— директория для хранения данных, которые распределяются между экземплярами.
``` bash
# Tarantool manual
Usage:
    tarantoolctl start INSTANCE
    tarantoolctl stop INSTANCE
    tarantoolctl status INSTANCE
    tarantoolctl restart INSTANCE
    tarantoolctl logrotate INSTANCE
    tarantoolctl check INSTANCE
    tarantoolctl enter INSTANCE [--language=language]
    tarantoolctl eval INSTANCE FILE
    COMMAND | tarantoolctl eval INSTANCE
    tarantoolctl connect URI
    COMMAND | tarantoolctl connect URI
    tarantoolctl cat FILE.. [--space=space_no ..] [--show-system] [--from=from_lsn] [--to=to_lsn] [--replica=replica_id ..]
    tarantoolctl play URI FILE.. [--space=s
```
