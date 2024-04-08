# Basics
***
Работа Nginx и его модулей определяется в концигурационном файле. 
`nginx.conf` - конфигурационный файл по умолчанию.
``` bash
# Установим NGINX
sudo apt install nginx

# Проверяем что запущен
systemctl status nginx

# Основные команды
sudo systemctl stop nginx
sudo systemctl start nginx
sudo systemctl restart nginx
sudo systemctl reload nginx

# Создаем папку для сайта
sudo mkdir -p /var/www/you-domain/html

# Даем себе права на папку
sudo chown -R $USER:$USER /var/www/you-domain/html

# Создаем тестовый сайт
nano /var/www/you-domain/html/index.html

# Создаем файл настроек
sudo nano /etc/nginx/sites-available/you-domain

# Вставляем туда следующий блок:
server {
        listen 80;
        listen [::]:80;

        root /var/www/you-domain/html;
        index index.html index.htm index.nginx-debian.html;

        server_name you-domain www.you-domain;

        location / {
                try_files $uri $uri/ =404;
        }
}

# Активируем наш файл настроек
sudo ln -s /etc/nginx/sites-available/you-domain /etc/nginx/sites-enabled/

# Корректируем файл
sudo nano /etc/nginx/nginx.conf
# раскоментируем строчку server_names_hash_bucket_size 64;

# Проверяем корректность настроек
sudo nginx -t

# Перезапускаем NGINX
sudo systemctl restart nginx
```
###### Базовые команды.
``` bash
nginx
Options:
  -?,-h         # this help
  -v            # show version and exit
  -V            # show version and configure options then exit
  -t            # test configuration and exit
  -T            # test configuration, dump it and exit
  -q            # suppress non-error messages during configuration testing
  -s signal     # send signal to a master process: stop, quit, reopen, reload
  -p prefix     # set prefix path (default: /usr/share/nginx/)
  -c filename   # set configuration file (default: /etc/nginx/nginx.conf)
  -g directives # set global directives out of configuration file


nginx -s stop   # Быстрое заврешенеие работы сервера.
nginx -s quit   # Плавное завершение работы сервера.
nginx -s reload # Перезагрузка конфигурационного файла сервера.
nginx -s reopen # Переоткрытие лог файлов сервера.
ps -ax | grep nginx # Команда для просмотра запущенных процесоов Nginx.
```
***
## Config Structure
Nginx состоит из модулей, которые настраиваются директивами в конфигруационном файле. 
Директивы делятся на **простые** и **блочные**. 
`#` - символ для комментирования в конфигурационном файле.
``` conf
# Список основных директив Nginx Core
     accept_mutex
     accept_mutex_delay
     daemon
     debug_connection
     debug_points
     env
     error_log
     events
     include
     load_module
     lock_file
     master_process
     multi_accept
     pcre_jit
     pid
     ssl_engine
     thread_pool
     timer_resolution
     use
     user
     worker_aio_requests
     worker_connections
     worker_cpu_affinity
     worker_priority
     worker_processes
     worker_rlimit_core
     worker_rlimit_nofile
     worker_shutdown_timeout
     working_directory

# Список директив модуля ngx_http_core_module
     absolute_redirect
     aio
     aio_write
     alias
     auth_delay
     chunked_transfer_encoding
     client_body_buffer_size
     client_body_in_file_only
     client_body_in_single_buffer
     client_body_temp_path
     client_body_timeout
     client_header_buffer_size
     client_header_timeout
     client_max_body_size
     connection_pool_size
     default_type
     directio
     directio_alignment
     disable_symlinks
     error_page
     etag
     http
     if_modified_since
     ignore_invalid_headers
     internal
     keepalive_disable
     keepalive_requests
     keepalive_time
     keepalive_timeout
     large_client_header_buffers
     limit_except
     limit_rate
     limit_rate_after
     lingering_close
     lingering_time
     lingering_timeout
     listen
     location
     log_not_found
     log_subrequest
     max_ranges
     merge_slashes
     msie_padding
     msie_refresh
     open_file_cache
     open_file_cache_errors
     open_file_cache_min_uses
     open_file_cache_valid
     output_buffers
     port_in_redirect
     postpone_output
     read_ahead
     recursive_error_pages
     request_pool_size
     reset_timedout_connection
     resolver
     resolver_timeout
     root
     satisfy
     send_lowat
     send_timeout
     sendfile
     sendfile_max_chunk
     server
     server_name
     server_name_in_redirect
     server_names_hash_bucket_size
     server_names_hash_max_size
     server_tokens
     subrequest_output_buffer_size
     tcp_nodelay
     tcp_nopush
     try_files
     types
     types_hash_bucket_size
     types_hash_max_size
     underscores_in_headers
     variables_hash_bucket_size
     variables_hash_max_size
```
Подробнее: 
	[[Config Structure]]
***
## Static Data
