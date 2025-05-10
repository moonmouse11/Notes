# journalctl
***
## Basic
**journalctl** - выводит записи журналов (логов) из `systemd`.
***
## Tips
- `journalctl` - выводит весь лог `systemd`.
- `journalctl -f` - выводит лог в реальном времени. Аналог `tail -f`.
- `journalctl --no-page` - вывод логов без пагинации.
- `journalctl -u nginx.service` - выводи лог указанного сервиса.
- `journalctl --since "xxx" --until "xxx"` - выводит лог за указанный промежуток времени.
- `journalctl _UID=number --since today` - фильтр по дате и идентификатору пользователя.
- `journalctl _PID=number` - фильтр по идентификатору родительского процесса.
- `journalctl _EXE=/path_to_executable` - фильттр по исполняемому файлу.
- `joutnalctl -k` - вывод логов ядра системы.
- `journalctl -r` - выводит лог в обратном порядке.
- `journalctl -p err` - выводит лог по типу записи (`err`, `warning`, `info`, `debug`).
- `journalctl --list-boots` - выводит лог сессии системы.
- `journalctl -b` - выводит лог текущего запуска системы.
- `journalctl -b -1` - выводит лог предыдущего запуска системы.
- `journalctl --grep "pattern"` - фильтр логов по подстроке.
- `journalctl -o json-pretty` - выводи лог в формате `JSON`.
- `journalctl --dist-usage` - выводи размер файлов логов.
- `journalctl --rotate` - 
- `journalctl --vacuum-time=2weeks` - ограничение записи логов на 2 недели.
- `journalctl --vacuum-size=500M` - ограничение записи логов по размеру файла.
- `journalctl --vacuum-file=10` - ограничение заиси логов на количество файлов.
- `journalctl -o export > /path/to/out.journal` - экспорт журнала в другой файл.
***
