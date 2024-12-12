# General
***
## Commands
- `find . -type d -exec chown bitrix:bitrix {} \;`
- `curl -X [запрос] [адрес]`
- `file -L -i`
- `ncal` - календарь.
- `neofetch` - вывод информации о системе в терминал.
- `bg` `fg` - возобновление остановленного процесса.
- `jobs` - просмотр остановленных команд.
- `ls | wc -l` - показывает количество строк в выводе файлов в текущей директории.
- `wc [file_name]` - считает количество слов в указанном файле.
- `wc -l [file_name]` - считает количество строк в указанном файле.
- `wc -c [file_name]` - считает количество символов в указанном файле.
- `cmp [first_file_name] [second_file_name]` - команда для сравнения двух файлов.
- `comm [first_file_name] [second_file_name]` - команда для сравнения двух отсортированных файлов.
- `fmt [file_name]` - команда для форматирования текста.
- `expand -i [file_name]`  - заменяет в указанном файле символы табуляции  на пробелы.
- `unexpand [filename]` - противоположна команде выше.
- `column [file_name]` - разбивает текст на несколько столбцов.
- `fold [file_name]` - выравнивает текст по правому краю.
- `head -n [number] [file_name]` - выводит первые строки указанного файла.
- `tail -n [number] [file_name]` - выводит последние строки указанного файла.
- `sort [files_name]` - сортирует указанные файлы.
- `split -[number] [file_name]` - разбивает указанный файл на несколько по указанным параметрам.
- `apropos [keyword]` - команда для поиска мануала и описания заданной bash инструкции.
- `man -k` - аналогична команде выше.
- `cd ~` - отправляет в домашнюю директорию пользователя.
- `dir` `vdir` - альтернативы `ls` и `ls -l`
- `uname –r` - узнать версию ядра системы.
- `rsync` - инструмент копирования файлов.
- `find . ` - выводит все файлы в текущей директории рекурсивно.
- `find место_поиска ключ-свойство значение_свойства` - команда поиска c шаблоном.
- `diff` - показывает различия между двумя файлами.
- `diff -r` - рекурсивное сравнение файлов.
- `dmesg` - вывод буфера сообщений ядра в стандартный поток.
- `fsck` - проверка на 
- `sudo blkid` - выводит информацию о устройствах.
- `stat [file_name]` - выводит информацию и дате создания и изменения указанного файла.
- `ldd [full_path_file]` - выводит зависимости указанного исполняемого файла.
- `poweroff` - завершение работы системы с выключением питания.
- `reboot` - перезагрузка системы.
- `shutdown -r now` - перезагрузка системы прямо сейчас.
- `shutdown -h [time]` - выключение системы в указанное время.
- `shutdown -c` - отмена запланированного выключения.
- `arch` - выводит информацию об архитектуре процессора.
- `cat /proc/cpuinfo` - выводит информацию о процессоре.
- `cat /etc/shells` - выводит список командных оболочек.
- `cat /etc/fstab` - выводит информацию о файловой системе.
- `free -h` - выводит информацию об использовании оперативной памяти.
- `free -hl` - выводит информацию об использовании оперативной памяти со статистикой нагрузки.
- `df -h` - выводит информацию об использовании дискового пространства.
- `cat /proc/net/dev` - выводит список сетевых интерфейсов.
- `date` - выводит системную дату.
- `cal [year_number]` - выводит календарь на указанный год.
- `lspci -tv` - выводит информацию о PSI устройствах
- `lsusb -tv` - выводит информацию о USB устройствах.
- `clear` - очищает терминал.
- `ifconfig [interface_name]` - показывает информацию о сетевом интерфейсе.
- `ifconfig [interface_name] up` - запуск интерфейса.
- `ifcinfig [interface_name] down` - остановка интерфейса.
- `ifup [interface_name]` - активация указанного интерфейса.
- `ifdown [interface_name]` - деактивация указанного интерфейса.
- `ifconfig [interface_name] [ip_address]` - показывает конфигурацию указанного интерфейса.
- `dhclient [interface_name]` - активация интерфейса в dhcp режиме.
- `route` - просмотр таблицы маршрутизации.
- `netstat -rn` - просмотр таблицы маршрутизации.
- `route -n` - выводит локальную таблицу маршрутизации.
- `route add -net gw [ip_address]` - задает IP-адрес шлюза по умолчанию.
- `hostname` - выводит имя компьютера.
- `host [domain_name]` - команда разрешает использовать хосту указанное доменное имя.
- `ip link show` - выводит состояние всех интерфейсов.
- `iwlist scan` - сканирует wi-fi точки.
- `iwconfig [interface_name]` - показывает конфигурацию беспроводного интерфейса.
- `tracepath` - трассировка пути следования пакетов.
- `traceroute [domain_name]` - трассировка маршрута до указанного хостинга.
- `ping [domain_name]` - проверяет доступность указанного домена.
- `ps -ely` - просмотр запущенных процессов в Linux.
- `nice -n [PID]` - изменение приоритета выполнения процесса.
- `renice` - противоположна команде выше.
- `kill -[number] [PID]` - завершает  указанный процесс.
- `kill -9 [PID]` - аварийное завершение указанного процесса.
- `kill -15 [PID]` - корректное завершение программы.
- `kill -19 [PID]` - остановка выполнения процесса.
- `kill -18 [PID]` - восстановление работы остановленного процесса.
- `kill -1 [PID]` - перезапуск процесса.
- `killall [process_name]` - завершение всех процессов указанной программы.
- `top` - диспетчер задач Linux.
- `sed` - потоковый текстовый редактор.
- `ssh-keygen -t rsa` - команда для генерации ключей для доступа по ssh.
- `adduser [user_name]` - добавляет пользователя в систему.
- `passwd [user_login]` - изменяет пароль пользователя.
- `usermod [user_name]` - модификация учетной записи.
- `visudo` - предоставляет возможность редактировать файл для выдачи прав root.
- `userdel [user]` - удаляет пользователя из системы.
- `chown [user]:[group] [file_name]`- меняет пользователя у файла.
- `chmod [rigths] [file_name]` - меняет права доступа у файла.
- `chattr` - команда для добавления атрибутов к файлу.
- `lsattr [attribute_argument] [file_name]` - просмотр атрибутов файла.
- `sudo vim /etc/hostname` - файл сетевой конфигурации устройства.
- `lsns` - выводит список namespace Linux. 
- `printenv` - выводит список всех переменных окружения bash.
- `lvm` - (Logical volume manager) - Диспетчер логических томов.
- `mount` - монтирование файловой системы. Выводит список всех смонтированных файловых систем.
- `umount [filesystem_name]` - обратна команде выше.
- `fdisk` - работает с разбивкой памяти на разделы.
- `mkfs` - создает файловую систему.
- `dd`- копирует и конввертирует файл.
- `ls /dev` - просмотр директории всех доступных устройств в системе.
- `lsblk -f` - просмотр смонтированных устройств в системе.
- `swapon` - подключает устройства и файлы.
- `swapoff` - обратна команде выше.
- `gzip` - программа архиватор. Архивирует файл.
- `gunzip` - извлекает файл из архива.
- `rsync` - проверяет состояние файлов с удаленной файловой системой.
- `tar c [file_path]` - классический архиватор Linux. Архивирует файлы.
- `tar x [file_path]` - извлекает файлы из архива.
- `tail -n 500 /var/log/auth.log | grep 'sshd` - просмотр лога подключений к серверу.
- `netstat` - показывает активные интернет соединения.
- `ss` - утилита для работы с сокетами.
- `iptables` - программа для проверки передачи пакетов через протоколы IPv4/IPv6.
- `chsh -s /bin/bash` - команда для смены консольной оболочки.
- `whereis [command_name]` - выводит путь к программе, её файлам конфигурации и мануалу.
- `sudo blkid | grep "ntfs"` - поиск устройств с форматированием ntfs.
- `dd bs=4M if=path/to/archlinux-version-x86_64.iso of=/dev/sdx conv=fsync oflag=direct status=progress` - команда для записи iso-образа на указанный диск.
***
## Tips
Абсолютная и относительная адресация. `/`
- `cat /etc/shells` - файли со списком доступных командных оболочек в системе.
- `echo $SHELL` - выводит текщую командную оболочку.
***
## Links
### Жесткие ссылки
**Имя файла, ссылающееся на его индексный дескриптор, называется жесткой ссылкой.**
**Удаление жесткой ссылки на файл не приводит к удалению самого файла из системы при наличии у этого файла других жестких ссылок (имен файла)**.
### Мягкие ссылки
**Символьные ссылки - они представляют собой файлы, указывающие не на индексные дескрипторы, а на имена файлов, то есть на жесткие ссылки**.
`ln` - создает жесткую ссылку.
`ln -s` - создает символьную ссылку.
***
## Permissions
`chmod [ключи] установка_прав имя_файла` 
`chmod -c` - узнать дату смены прав доступа.
Права доступа состоят из трех наборов:
- для владельца.
- для группы.
- для прочих пользователей.
В каждом наборе может быть три права:
- `r` - права на чтение.
- `w` - права на запись.
- `x` - право на выполнение.
Набор прав определяется в восьмеричном формате.
- `chmod u+s` - SGID (Set Group ID root) дает возможность запускать файл от пользователя root.
- `chattr` - команда для добавления атрибутов к файлу.
- `lsattr [attribute_argument] [file_name]` - просмотр атрибутов файла.
Атрибуты для установки в файл:
- `i` - запрещает изменение, переименование и удаление файла.
- `u` - при удалении файла, сохраняет содержимое на диске для восстановления.
- `c` - архивирует большие файлы.
- `S` - сразу сохраняет данные на диск.
- `s` - противоположен атрибуту `u` при удалении файла, сразу очищает данные с диска.
``` bash
chattr +i [file_name] # Добавление атрибута
chattr -i [file_name] # Удаление атрибута
```
***
## Bash shortcuts
### Moving

| command            | description                                                                         |
| ------------------ | ----------------------------------------------------------------------------------- |
| ctrl + a           | Goto BEGINNING of command line                                                      |
| ctrl + e           | Goto END of command line                                                            |
| ctrl + b           | move back one character                                                             |
| ctrl + f           | move forward one character                                                          |
| alt + f            | move cursor FORWARD one word                                                        |
| alt + b            | move cursor BACK one word                                                           |
| ctrl + xx          | Toggle between the start of line and current cursor position                        |
| ctrl + ] + x       | Where x is any character, moves the cursor forward to the next occurance of x       |
| alt + ctrl + ] + x | Where x is any character, moves the cursor backwards to the previous occurance of x |
### Edit / Other
| command           | description                                                                                                         |
| ----------------- | ------------------------------------------------------------------------------------------------------------------- |
| ctrl + d          | Delete the character under the cursor                                                                               |
| ctrl + h          | Delete the previous character before cursor                                                                         |
| ctrl + u          | Clear all / cut BEFORE cursor                                                                                       |
| ctrl + k          | Clear all / cut AFTER cursor                                                                                        |
| ctrl + w          | delete the word BEFORE the cursor                                                                                   |
| alt + d           | delete the word FROM the cursor                                                                                     |
| ctrl + y          | paste (if you used a previous command to delete)                                                                    |
| ctrl + i          | command completion like Tab                                                                                         |
| ctrl + l          | Clear the screen (same as clear command)                                                                            |
| ctrl + c          | kill whatever is running                                                                                            |
| ctrl + d          | Exit shell (same as exit command when cursor line is empty)                                                         |
| ctrl + z          | Place current process in background                                                                                 |
| ctrl + _          | Undo                                                                                                                |
| ctrl + x ctrl + u | Undo the last changes. ctrl+ _ does the same                                                                        |
| ctrl + t          | Swap the last two characters before the cursor                                                                      |
| esc + t           | Swap last two words before the cursor                                                                               |
| alt + t           | swap current word with previous                                                                                     |
| esc + .           |                                                                                                                     |
| esc + _           |                                                                                                                     |
| alt + [Backspace] | delete PREVIOUS word                                                                                                |
| alt + <           | Move to the first line in the history                                                                               |
| alt + >           | Move to the end of the input history, i.e., the line currently being entered                                        |
| alt + ?           | display the file/folder names in the current path as help                                                           |
| alt + *           | print all the file/folder names in the current path as parameter                                                    |
| alt + .           | print the LAST ARGUMENT (ie "vim file1.txt file2.txt" will yield "file2.txt")                                       |
| alt + c           | capitalize the first character to end of word starting at cursor (whole word if cursor is at the beginning of word) |
| alt + u           | make uppercase from cursor to end of word                                                                           |
| alt + l           | make lowercase from cursor to end of word                                                                           |
| alt + n           |                                                                                                                     |
| alt + p           | Non-incremental reverse search of history.                                                                          |
| alt + r           | Undo all changes to the line                                                                                        |
| alt + ctl + e     | Expand command line.                                                                                                |
| ~[TAB][TAB]       | List all users                                                                                                      |
| $[TAB][TAB]       | List all system variables                                                                                           |
| @[TAB][TAB]       | List all entries in your /etc/hosts file                                                                            |
| [TAB]             | Auto complete                                                                                                       |
| cd -              | change to PREVIOUS working directory                                                                                |

***
## Bash config
- `/etc/profile` - содержит глобальные настройки, влияющие на всю систему.
- `/.bash_profile` - обрабатывается при каждом входе в систему. Можно добавить команды, которые будут выполнены при каждом входе в систему. Сейчас практически не используется.
- `/.bashrc` - файл обрабатывается при каждом запуске дочерней оболочки. Как правило это происходит один только раз - при входе в систему. Пользователь может запустить свою дочернюю оболочку.
- `/.bash_logout` - обрабатывается при выходе пользователя их системы.
Переменная `PS1` отвечает за внешний вид командной строки.
#### Стандартное значение:
``` bash
PS1='\u@\h:\w$'
```
#### Модификаторы командной строки:
``` bash
\a  # ASCII-символ звонка
\d  # Дата в формате "День недели, мксяц, число"
\h  # Имя компьютера до первой точки
\H  # Полное имя компьютера
\j  # Количество задач, запущенных в данное время.
\l  # Название терминала
\n  # Символ новой строки
\r  # Возврат каретки
\s  # Название оболочки
\t  # Время в 24-часовом формате
\T  # Время в 12-часовом формате 
\u  # Имя пользователя
\v  # Версия bash
\V  # Полная версия bash
\w  # Текущий каталог, полный путь
\W  # Текущий каталог, короткий путь
\!  # Номер команды в истории
\#  # Системный номер команды
\\  # Обратный слеш
$() # Постановка внешней команды
```
***
