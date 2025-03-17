# php.ini
***
## Directives
`mbstring.func_overload` -
`default_socket_timeout` -
### Configuration options

| **Name**                          | **Default**           | **Changeable** | **Changelog**                                                                                |
| --------------------------------- | --------------------- | -------------- | -------------------------------------------------------------------------------------------- |
| `allow_url_fopen`                 | "1"                   | `INI_SYSTEM`   |                                                                                              |
| `allow_url_include`               | "0"                   | `INI_SYSTEM`   | Deprecated as of PHP 7.4.0.                                                                  |
| `arg_separator.input`             | "&"                   | `INI_PERDIR`   |                                                                                              |
| `arg_separator.output`            | "&"                   | `INI_ALL`      |                                                                                              |
| `assert.active`                   | "1"                   | `INI_ALL`      |                                                                                              |
| `assert.bail`                     | "0"                   | `INI_ALL`      |                                                                                              |
| `assert.callback`                 | null                  | `INI_ALL`      |                                                                                              |
| `assert.exception`                | "0"                   | `INI_ALL`      |                                                                                              |
| `assert.quiet_eval`               | "0"                   | `INI_ALL`      | Removed as of PHP 8.0.0                                                                      |
| `assert.warning`                  | "1"                   | `INI_ALL`      |                                                                                              |
| `auto_append_file`                | null                  | `INI_PERDIR`   |                                                                                              |
| `auto_detect_line_endings`        | "0"                   | `INI_ALL`      |                                                                                              |
| `auto_globals_jit`                | "1"                   | `INI_PERDIR`   |                                                                                              |
| `auto_prepend_file`               | null                  | `INI_PERDIR`   |                                                                                              |
| `bcmath.scale`                    | "0"                   | `INI_ALL`      |                                                                                              |
| `browscap`                        | null                  | `INI_SYSTEM`   |                                                                                              |
| `cgi.check_shebang_line`          | "1"                   | `INI_SYSTEM`   |                                                                                              |
| `cgi.discard_path`                | "0"                   | `INI_SYSTEM`   |                                                                                              |
| `cgi.fix_pathinfo`                | "1"                   | `INI_SYSTEM`   |                                                                                              |
| `cgi.force_redirect`              | "1"                   | `INI_SYSTEM`   |                                                                                              |
| `cgi.nph`                         | "0"                   | `INI_ALL`      |                                                                                              |
| `cgi.redirect_status_env`         | null                  | `INI_SYSTEM`   |                                                                                              |
| `cgi.rfc2616_headers`             | "0"                   | `INI_ALL`      |                                                                                              |
| `child_terminate`                 | "0"                   | `INI_ALL`      |                                                                                              |
| `cli.pager`                       | ""                    | `INI_ALL`      |                                                                                              |
| `cli.prompt`                      | "\\b \\> "            | `INI_ALL`      |                                                                                              |
| `cli_server.color`                | "0"                   | `INI_ALL`      |                                                                                              |
| `com.allow_dcom`                  | "0"                   | `INI_SYSTEM`   |                                                                                              |
| `com.autoregister_typelib`        | "0"                   | `INI_ALL`      |                                                                                              |
| `com.autoregister_verbose`        | "0"                   | `INI_ALL`      |                                                                                              |
| `com.autoregister_casesensitive`  | "1"                   | `INI_ALL`      |                                                                                              |
| `com.code_page`                   | ""                    | `INI_ALL`      |                                                                                              |
| `com.dotnet_version`              | ""                    | `INI_SYSTEM`   | As of PHP 8.0.0                                                                              |
| `com.typelib_file`                | ""                    | `INI_SYSTEM`   |                                                                                              |
| `curl.cainfo`                     | NULL                  | `INI_SYSTEM`   |                                                                                              |
| `date.default_latitude`           | "31.7667"             | `INI_ALL`      |                                                                                              |
| `date.default_longitude`          | "35.2333"             | `INI_ALL`      |                                                                                              |
| `date.sunrise_zenith`             | "90.833333"           | `INI_ALL`      | Prior to PHP 8.0.0, the default was "90.583333"                                              |
| `date.sunset_zenith`              | "90.833333"           | `INI_ALL`      | Prior to PHP 8.0.0, the default was "90.583333"                                              |
| `date.timezone`                   | "UTC"                 | `INI_ALL`      | From PHP 8.2, a warning is emitted when setting this to an invalid value or an empty string. |
| `dba.default_handler`             | DBA_DEFAULT           | `INI_ALL`      |                                                                                              |
| `default_charset`                 | "UTF-8"               | `INI_ALL`      |                                                                                              |
| `input_encoding`                  | ""                    | `INI_ALL`      |                                                                                              |
| `output_encoding`                 | ""                    | `INI_ALL`      |                                                                                              |
| `internal_encoding`               | ""                    | `INI_ALL`      |                                                                                              |
| `default_mimetype`                | "text/html"           | `INI_ALL`      |                                                                                              |
| `default_socket_timeout`          | "60"                  | `INI_ALL`      |                                                                                              |
| `disable_classes`                 | ""                    | php.ini only   |                                                                                              |
| `disable_functions`               | ""                    | php.ini only   |                                                                                              |
| `display_errors`                  | "1"                   | `INI_ALL`      |                                                                                              |
| `display_startup_errors`          | "1"                   | `INI_ALL`      | Prior to PHP 8.0.0, the default value was "0".                                               |
| `docref_ext`                      | ""                    | `INI_ALL`      |                                                                                              |
| `docref_root`                     | ""                    | `INI_ALL`      |                                                                                              |
| `doc_root`                        | null                  | `INI_SYSTEM`   |                                                                                              |
| `enable_dl`                       | "1"                   | `INI_SYSTEM`   | This deprecated feature will certainly be removed in the future.                             |
| `enable_post_data_reading`        | "On"                  | `INI_PERDIR`   |                                                                                              |
| `engine`                          | "1"                   | `INI_ALL`      |                                                                                              |
| `error_append_string`             | null                  | `INI_ALL`      |                                                                                              |
| `error_log`                       | null                  | `INI_ALL`      |                                                                                              |
| `error_log_mode`                  | 0o644                 | `INI_ALL`      | Available as of PHP 8.2.0                                                                    |
| `error_prepend_string`            | null                  | `INI_ALL`      |                                                                                              |
| `error_reporting`                 | null                  | `INI_ALL`      |                                                                                              |
| `exif.encode_unicode`             | "ISO-8859-15"         | `INI_ALL`      |                                                                                              |
| `exif.decode_unicode_motorola`    | "UCS-2BE"             | `INI_ALL`      |                                                                                              |
| `exif.decode_unicode_intel`       | "UCS-2LE"             | `INI_ALL`      |                                                                                              |
| `exif.encode_jis`                 | ""                    | `INI_ALL`      |                                                                                              |
| `exif.decode_jis_motorola`        | "JIS"                 | `INI_ALL`      |                                                                                              |
| `exif.decode_jis_intel`           | "JIS"                 | `INI_ALL`      |                                                                                              |
| `exit_on_timeout`                 | ""                    | `INI_ALL`      |                                                                                              |
| `expect.timeout`                  | "10"                  | `INI_ALL`      |                                                                                              |
| `expect.loguser`                  | "1"                   | `INI_ALL`      |                                                                                              |
| `expect.logfile`                  | ""                    | `INI_ALL`      |                                                                                              |
| `expect.match_max`                | ""                    | `INI_ALL`      |                                                                                              |
| `expose_php`                      | "1"                   | php.ini only   |                                                                                              |
| `extension`                       | null                  | php.ini only   |                                                                                              |
| `extension_dir`                   | "/path/to/php"        | `INI_SYSTEM`   |                                                                                              |
| `fastcgi.impersonate`             | "0"                   | `INI_SYSTEM`   |                                                                                              |
| `fastcgi.logging`                 | "1"                   | `INI_SYSTEM`   |                                                                                              |
| `file_uploads`                    | "1"                   | `INI_SYSTEM`   |                                                                                              |
| `filter.default`                  | "unsafe_raw"          | `INI_PERDIR`   | Deprecated as of PHP 8.1.0.                                                                  |
| `filter.default_flags`            | NULL                  | `INI_PERDIR`   |                                                                                              |
| `from`                            | ""                    | `INI_ALL`      |                                                                                              |
| `gd.jpeg_ignore_warning`          | "1"                   | `INI_ALL`      |                                                                                              |
| `geoip.custom_directory`          | ""                    | `INI_ALL`      |                                                                                              |
| `hard_timeout`                    | "2"                   | `INI_SYSTEM`   | Available as of PHP 7.1.0.                                                                   |
| `highlight.comment`               | "#FF8000"             | `INI_ALL`      |                                                                                              |
| `highlight.default`               | "#0000BB"             | `INI_ALL`      |                                                                                              |
| `highlight.html`                  | "#000000"             | `INI_ALL`      |                                                                                              |
| `highlight.keyword`               | "#007700"             | `INI_ALL`      |                                                                                              |
| `highlight.string`                | "#DD0000"             | `INI_ALL`      |                                                                                              |
| `html_errors`                     | "1"                   | `INI_ALL`      |                                                                                              |
| `ibase.allow_persistent`          | "1"                   | `INI_SYSTEM`   |                                                                                              |
| `ibase.max_persistent`            | "-1"                  | `INI_SYSTEM`   |                                                                                              |
| `ibase.max_links`                 | "-1"                  | `INI_SYSTEM`   |                                                                                              |
| `ibase.default_db`                | NULL                  | `INI_SYSTEM`   |                                                                                              |
| `ibase.default_user`              | NULL                  | `INI_ALL`      |                                                                                              |
| `ibase.default_password`          | NULL                  | `INI_ALL`      |                                                                                              |
| `ibase.default_charset`           | NULL                  | `INI_ALL`      |                                                                                              |
| `ibase.timestampformat`           | "%Y-%m-%d %H:%M:%S"   | `INI_ALL`      |                                                                                              |
| `ibase.dateformat`                | "%Y-%m-%d"            | `INI_ALL`      |                                                                                              |
| `ibase.timeformat`                | "%H:%M:%S"            | `INI_ALL`      |                                                                                              |
| `ibm_db2.binmode`                 | "1"                   | `INI_ALL`      |                                                                                              |
| `ibm_db2.i5_all_pconnect`         | "0"                   | `INI_SYSTEM`   | Available as of ibm_db2 1.6.5.                                                               |
| `ibm_db2.i5_allow_commit`         | "0"                   | `INI_SYSTEM`   | Available as of ibm_db2 1.4.9.                                                               |
| `ibm_db2.i5_blank_userid`         | "0"                   | `INI_SYSTEM`   | Available as of ibm_db2 1.9.7.                                                               |
| `ibm_db2.i5_char_trim`            | "0"                   | `INI_SYSTEM`   | Available as of ibm_db2 2.1.0.                                                               |
| `ibm_db2.i5_dbcs_alloc`           | "0"                   | `INI_SYSTEM`   | Available as of ibm_db2 1.5.0.                                                               |
| `ibm_db2.i5_guard_profile`        | "0"                   | `INI_SYSTEM`   | Available as of ibm_db2 1.9.7.                                                               |
| `ibm_db2.i5_ignore_userid`        | "0"                   | `INI_SYSTEM`   | Available as of ibm_db2 1.8.0.                                                               |
| `ibm_db2.i5_job_sort`             | "0"                   | `INI_SYSTEM`   | Available as of ibm_db2 1.8.4.                                                               |
| `ibm_db2.i5_log_verbose`          | "0"                   | `INI_SYSTEM`   | Available as of ibm_db2 1.9.7.                                                               |
| `ibm_db2.i5_max_pconnect`         | "0"                   | `INI_SYSTEM`   | Available as of ibm_db2 1.9.7.                                                               |
| `ibm_db2.i5_override_ccsid`       | "0"                   | `INI_SYSTEM`   | Available as of ibm_db2 1.9.7.                                                               |
| `ibm_db2.i5_servermode_subsystem` | NULL                  | `INI_SYSTEM`   | Available as of ibm_db2 1.9.7.                                                               |
| `ibm_db2.i5_sys_naming`           | "0"                   | `INI_SYSTEM`   | Available as of ibm_db2 1.9.7.                                                               |
| `ibm_db2.instance_name`           | NULL                  | `INI_SYSTEM`   | Available as of ibm_db2 1.0.2.                                                               |
| `iconv.input_encoding`            | ""                    | `INI_ALL`      | Deprecated in PHP 5.6.0.                                                                     |
| `iconv.output_encoding`           | ""                    | `INI_ALL`      | Deprecated in PHP 5.6.0.                                                                     |
| `iconv.internal_encoding`         | ""                    | `INI_ALL`      | Deprecated in PHP 5.6.0.                                                                     |
| `ignore_repeated_errors`          | "0"                   | `INI_ALL`      |                                                                                              |
| `ignore_repeated_source`          | "0"                   | `INI_ALL`      |                                                                                              |
| `ignore_user_abort`               | "0"                   | `INI_ALL`      |                                                                                              |
| `implicit_flush`                  | "0"                   | `INI_ALL`      |                                                                                              |
| `include_path`                    | ".:/path/to/php/pear" | `INI_ALL`      |                                                                                              |
| `intl.default_locale`             |                       | `INI_ALL`      |                                                                                              |
| `intl.error_level`                | 0                     | `INI_ALL`      |                                                                                              |
| `intl.use_exceptions`             | 0                     | `INI_ALL`      | Available since PECL 3.0.0a1                                                                 |
| `last_modified`                   | "0"                   | `INI_ALL`      |                                                                                              |
|                                   |                       |                |                                                                                              |
|                                   |                       |                |                                                                                              |
|                                   |                       |                |                                                                                              |
|                                   |                       |                |                                                                                              |
***
## Sections


***
## Example
``` ini
phpinfo()
PHP Version => 8.3.15

System => Linux Arch 6.12.10-arch1-1 #1 SMP PREEMPT_DYNAMIC Sat, 18 Jan 2025 02:26:57 +0000 x86_64
Build Date => Dec 17 2024 19:12:24
Build System => Linux arch-nspawn-417209 6.12.4-arch1-1 #1 SMP PREEMPT_DYNAMIC Mon, 09 Dec 2024 14:31:57 +0000 x86_64 GNU/Linux
Configure Command =>  './configure'  '--srcdir=../php-8.3.15' '--config-cache' '--prefix=/usr' '--sbindir=/usr/bin' '--sysconfdir=/etc/php' '--localstatedir=/var' '--with-layout=GNU' '--with-config-file-path=/etc/php' '--with-config-file-scan-dir=/etc/php/conf.d' '--disable-rpath' '--mandir=/usr/share/man' '--enable-cgi' '--enable-fpm' '--with-fpm-systemd' '--with-fpm-acl' '--with-fpm-user=http' '--with-fpm-group=http' '--enable-embed=shared' '--enable-bcmath=shared' '--enable-calendar=shared' '--enable-dba=shared' '--enable-exif=shared' '--enable-ftp=shared' '--enable-gd=shared' '--enable-intl=shared' '--enable-mbstring' '--enable-pcntl' '--enable-shmop=shared' '--enable-soap=shared' '--enable-sockets=shared' '--enable-sysvmsg=shared' '--enable-sysvsem=shared' '--enable-sysvshm=shared' '--with-bz2=shared' '--with-curl=shared' '--with-enchant=shared' '--with-external-gd' '--with-external-pcre' '--with-ffi=shared' '--with-gdbm' '--with-gettext=shared' '--with-gmp=shared' '--with-iconv=shared' '--with-kerberos' '--with-ldap=shared' '--with-ldap-sasl' '--with-mhash' '--with-mysql-sock=/run/mysqld/mysqld.sock' '--with-mysqli=shared' '--with-openssl' '--with-password-argon2' '--with-pdo-dblib=shared,/usr' '--with-pdo-mysql=shared' '--with-pdo-odbc=shared,unixODBC,/usr' '--with-pdo-pgsql=shared' '--with-pdo-sqlite=shared' '--with-pgsql=shared' '--with-pspell=shared' '--with-readline' '--with-snmp=shared' '--with-sodium=shared' '--with-sqlite3=shared' '--with-tidy=shared' '--with-unixODBC=shared' '--with-xsl=shared' '--with-zip=shared' '--with-zlib' 'CFLAGS=-march=x86-64 -mtune=generic -O2 -pipe -fno-plt -fexceptions -Wp,-D_FORTIFY_SOURCE=3 -Wformat -Werror=format-security -fstack-clash-protection -fcf-protection -fno-omit-frame-pointer -mno-omit-leaf-frame-pointer -g -ffile-prefix-map=/build/php/src=/usr/src/debug/php' 'LDFLAGS=-Wl,-O1 -Wl,--sort-common -Wl,--as-needed -Wl,-z,relro -Wl,-z,now -Wl,-z,pack-relative-relocs' 'CXXFLAGS=-march=x86-64 -mtune=generic -O2 -pipe -fno-plt -fexceptions -Wp,-D_FORTIFY_SOURCE=3 -Wformat -Werror=format-security -fstack-clash-protection -fcf-protection -fno-omit-frame-pointer -mno-omit-leaf-frame-pointer -Wp,-D_GLIBCXX_ASSERTIONS -g -ffile-prefix-map=/build/php/src=/usr/src/debug/php'
Server API => Command Line Interface
Virtual Directory Support => disabled
Configuration File (php.ini) Path => /etc/php
Loaded Configuration File => /etc/php/php.ini
Scan this dir for additional .ini files => /etc/php/conf.d
Additional .ini files parsed => /etc/php/conf.d/igbinary.ini,
/etc/php/conf.d/imagick.ini,
/etc/php/conf.d/mongodb.ini,
/etc/php/conf.d/redis.ini,
/etc/php/conf.d/xdebug.ini

PHP API => 20230831
PHP Extension => 20230831
Zend Extension => 420230831
Zend Extension Build => API420230831,NTS
PHP Extension Build => API20230831,NTS
Debug Build => no
Thread Safety => disabled
Zend Signal Handling => enabled
Zend Memory Manager => enabled
Zend Multibyte Support => provided by mbstring
Zend Max Execution Timers => disabled
IPv6 Support => enabled
DTrace Support => disabled

Registered PHP Streams => https, ftps, compress.zlib, compress.bzip2, php, file, glob, data, http, ftp, phar, zip
Registered Stream Socket Transports => tcp, udp, unix, udg, ssl, tls, tlsv1.0, tlsv1.1, tlsv1.2, tlsv1.3
Registered Stream Filters => zlib.*, bzip2.*, string.rot13, string.toupper, string.tolower, convert.*, consumed, dechunk, convert.iconv.*

This program makes use of the Zend Scripting Language Engine:
Zend Engine v4.3.15, Copyright (c) Zend Technologies
    with Zend OPcache v8.3.15, Copyright (c), by Zend Technologies
    with Xdebug v3.4.1, Copyright (c) 2002-2025, by Derick Rethans


 _______________________________________________________________________


Configuration

bcmath

BCMath support => enabled

Directive => Local Value => Master Value
bcmath.scale => 0 => 0

bz2

BZip2 Support => Enabled
Stream Wrapper support => compress.bzip2://
Stream Filter support => bzip2.decompress, bzip2.compress
BZip2 Version => 1.0.8, 13-Jul-2019

calendar

Calendar support => enabled

Core

PHP Version => 8.3.15

Directive => Local Value => Master Value
allow_url_fopen => On => On
allow_url_include => Off => Off
arg_separator.input => & => &
arg_separator.output => & => &
auto_append_file => no value => no value
auto_globals_jit => On => On
auto_prepend_file => no value => no value
browscap => no value => no value
default_charset => UTF-8 => UTF-8
default_mimetype => text/html => text/html
disable_classes => no value => no value
disable_functions => no value => no value
display_errors => Off => Off
display_startup_errors => Off => Off
doc_root => no value => no value
docref_ext => no value => no value
docref_root => no value => no value
enable_dl => Off => Off
enable_post_data_reading => On => On
error_append_string => no value => no value
error_log => no value => no value
error_log_mode => 0644 => 0644
error_prepend_string => no value => no value
error_reporting => 22527 => 22527
expose_php => On => On
extension_dir => /usr/lib/php/modules/ => /usr/lib/php/modules/
fiber.stack_size => no value => no value
file_uploads => On => On
hard_timeout => 2 => 2
highlight.comment => <font style="color: #FF8000">#FF8000</font> => <font style="color: #FF8000">#FF8000</font>
highlight.default => <font style="color: #0000BB">#0000BB</font> => <font style="color: #0000BB">#0000BB</font>
highlight.html => <font style="color: #000000">#000000</font> => <font style="color: #000000">#000000</font>
highlight.keyword => <font style="color: #007700">#007700</font> => <font style="color: #007700">#007700</font>
highlight.string => <font style="color: #DD0000">#DD0000</font> => <font style="color: #DD0000">#DD0000</font>
html_errors => Off => Off
ignore_repeated_errors => Off => Off
ignore_repeated_source => Off => Off
ignore_user_abort => Off => Off
implicit_flush => On => On
include_path => .: => .:
input_encoding => no value => no value
internal_encoding => no value => no value
log_errors => On => On
mail.add_x_header => Off => Off
mail.force_extra_parameters => no value => no value
mail.log => no value => no value
mail.mixed_lf_and_crlf => Off => Off
max_execution_time => 0 => 0
max_file_uploads => 20 => 20
max_input_nesting_level => 64 => 64
max_input_time => -1 => -1
max_input_vars => 1000 => 1000
max_multipart_body_parts => -1 => -1
memory_limit => 512M => 512M
open_basedir => no value => no value
output_buffering => 0 => 0
output_encoding => no value => no value
output_handler => no value => no value
post_max_size => 8M => 8M
precision => 14 => 14
realpath_cache_size => 4096K => 4096K
realpath_cache_ttl => 120 => 120
register_argc_argv => On => On
report_memleaks => On => On
report_zend_debug => Off => Off
request_order => GP => GP
sendmail_from => no value => no value
sendmail_path => /usr/bin/sendmail -t -i => /usr/bin/sendmail -t -i
serialize_precision => -1 => -1
short_open_tag => Off => Off
SMTP => localhost => localhost
smtp_port => 25 => 25
sys_temp_dir => no value => no value
syslog.facility => LOG_USER => LOG_USER
syslog.filter => no-ctrl => no-ctrl
syslog.ident => php => php
unserialize_callback_func => no value => no value
upload_max_filesize => 20M => 20M
upload_tmp_dir => no value => no value
user_dir => no value => no value
user_ini.cache_ttl => 300 => 300
user_ini.filename => .user.ini => .user.ini
variables_order => GPCS => GPCS
xmlrpc_error_number => 0 => 0
xmlrpc_errors => Off => Off
zend.assertions => -1 => -1
zend.detect_unicode => On => On
zend.enable_gc => On => On
zend.exception_ignore_args => On => On
zend.exception_string_param_max_len => 0 => 0
zend.max_allowed_stack_size => 0 => 0
zend.multibyte => Off => Off
zend.reserved_stack_size => 0 => 0
zend.script_encoding => no value => no value
zend.signal_check => Off => Off

ctype

ctype functions => enabled

curl

cURL support => enabled
cURL Information => 8.11.1
Age => 11
Features
AsynchDNS => Yes
CharConv => No
Debug => No
GSS-Negotiate => No
IDN => Yes
IPv6 => Yes
krb4 => No
Largefile => Yes
libz => Yes
NTLM => Yes
NTLMWB => No
SPNEGO => Yes
SSL => Yes
SSPI => No
TLS-SRP => Yes
HTTP2 => Yes
GSSAPI => Yes
KERBEROS5 => Yes
UNIX_SOCKETS => Yes
PSL => Yes
HTTPS_PROXY => Yes
MULTI_SSL => No
BROTLI => Yes
ALTSVC => Yes
HTTP3 => Yes
UNICODE => No
ZSTD => Yes
HSTS => Yes
GSASL => No
Protocols => dict, file, ftp, ftps, gopher, gophers, http, https, imap, imaps, mqtt, pop3, pop3s, rtsp, scp, sftp, smb, smbs, smtp, smtps, telnet, tftp, ws, wss
Host => x86_64-pc-linux-gnu
SSL Version => OpenSSL/3.4.0
ZLib Version => 1.3.1
libSSH Version => libssh2/1.11.1

Directive => Local Value => Master Value
curl.cainfo => no value => no value

date

date/time support => enabled
timelib version => 2022.12
"Olson" Timezone Database Version => 2024.2
Timezone Database => internal
Default timezone => UTC

Directive => Local Value => Master Value
date.default_latitude => 31.7667 => 31.7667
date.default_longitude => 35.2333 => 35.2333
date.sunrise_zenith => 90.833333 => 90.833333
date.sunset_zenith => 90.833333 => 90.833333
date.timezone => UTC => UTC

dba

DBA support => enabled
Supported handlers => gdbm cdb cdb_make inifile flatfile 

Directive => Local Value => Master Value
dba.default_handler => flatfile => flatfile

dom

DOM/XML => enabled
DOM/XML API Version => 20031129
libxml Version => 2.13.5
HTML Support => enabled
XPath Support => enabled
XPointer Support => enabled
Schema Support => enabled
RelaxNG Support => enabled

enchant

(process:1665010): libenchant-WARNING **: 16:01:41.343: broker.vala:159: Error loading plugin: libhspell.so.0: cannot open shared object file: No such file or directory

(process:1665010): libenchant-WARNING **: 16:01:41.343: broker.vala:159: Error loading plugin: libnuspell.so.5: cannot open shared object file: No such file or directory

(process:1665010): libenchant-WARNING **: 16:01:41.343: broker.vala:159: Error loading plugin: libvoikko.so.1: cannot open shared object file: No such file or directory

enchant support => enabled
Libenchant Version => 2.8.2

hunspell => Hunspell Provider => /usr/lib/enchant-2/enchant_hunspell.so
aspell => Aspell Provider => /usr/lib/enchant-2/enchant_aspell.so

exif

EXIF Support => enabled
Supported EXIF Version => 0220
Supported filetypes => JPEG, TIFF
Multibyte decoding support using mbstring => enabled
Extended EXIF tag formats => Canon, Casio, Fujifilm, Nikon, Olympus, Samsung, Panasonic, DJI, Sony, Pentax, Minolta, Sigma, Foveon, Kyocera, Ricoh, AGFA, Epson

Directive => Local Value => Master Value
exif.decode_jis_intel => JIS => JIS
exif.decode_jis_motorola => JIS => JIS
exif.decode_unicode_intel => UCS-2LE => UCS-2LE
exif.decode_unicode_motorola => UCS-2BE => UCS-2BE
exif.encode_jis => no value => no value
exif.encode_unicode => ISO-8859-15 => ISO-8859-15

FFI

FFI support => enabled

Directive => Local Value => Master Value
ffi.enable => preload => preload
ffi.preload => no value => no value

fileinfo

fileinfo support => enabled
libmagic => 543

filter

Input Validation and Filtering => enabled

Directive => Local Value => Master Value
filter.default => unsafe_raw => unsafe_raw
filter.default_flags => no value => no value

ftp

FTP support => enabled
FTPS support => enabled

gd

GD Support => enabled
GD headers Version => 2.3.3
GD library Version => 2.3.3
FreeType Support => enabled
FreeType Linkage => with freetype
GIF Read Support => enabled
GIF Create Support => enabled
JPEG Support => enabled
PNG Support => enabled
WBMP Support => enabled
XPM Support => enabled
XBM Support => enabled
WebP Support => enabled
BMP Support => enabled
AVIF Support => enabled
TGA Read Support => enabled

Directive => Local Value => Master Value
gd.jpeg_ignore_warning => On => On

gettext

GetText Support => enabled

gmp

gmp support => enabled
GMP version => 6.3.0

hash

hash support => enabled
Hashing Engines => md2 md4 md5 sha1 sha224 sha256 sha384 sha512/224 sha512/256 sha512 sha3-224 sha3-256 sha3-384 sha3-512 ripemd128 ripemd160 ripemd256 ripemd320 whirlpool tiger128,3 tiger160,3 tiger192,3 tiger128,4 tiger160,4 tiger192,4 snefru snefru256 gost gost-crypto adler32 crc32 crc32b crc32c fnv132 fnv1a32 fnv164 fnv1a64 joaat murmur3a murmur3c murmur3f xxh32 xxh64 xxh3 xxh128 haval128,3 haval160,3 haval192,3 haval224,3 haval256,3 haval128,4 haval160,4 haval192,4 haval224,4 haval256,4 haval128,5 haval160,5 haval192,5 haval224,5 haval256,5 

MHASH support => Enabled
MHASH API Version => Emulated Support

iconv

iconv support => enabled
iconv implementation => glibc
iconv library version => 2.40

Directive => Local Value => Master Value
iconv.input_encoding => no value => no value
iconv.internal_encoding => no value => no value
iconv.output_encoding => no value => no value

intl

Internationalization support => enabled
ICU version => 75.1
ICU Data version => 75.1
ICU TZData version => 2024a
ICU Unicode version => 15.1

Directive => Local Value => Master Value
intl.default_locale => no value => no value
intl.error_level => 0 => 0
intl.use_exceptions => Off => Off

json

json support => enabled

ldap

LDAP Support => enabled
Total Links => 0/unlimited
API Version => 3001
Vendor Name => OpenLDAP
Vendor Version => 20609
SASL Support => Enabled

Directive => Local Value => Master Value
ldap.max_links => Unlimited => Unlimited

libxml

libXML support => active
libXML Compiled Version => 2.13.5
libXML Loaded Version => 21305-GITv2.13.5
libXML streams => enabled

mbstring

Multibyte Support => enabled
Multibyte string engine => libmbfl
HTTP input encoding translation => disabled
libmbfl version => 1.3.2

mbstring extension makes use of "streamable kanji code filter and converter", which is distributed under the GNU Lesser General Public License version 2.1.

Multibyte (japanese) regex support => enabled
Multibyte regex (oniguruma) version => 6.9.9

Directive => Local Value => Master Value
mbstring.detect_order => no value => no value
mbstring.encoding_translation => Off => Off
mbstring.http_input => no value => no value
mbstring.http_output => no value => no value
mbstring.http_output_conv_mimetypes => ^(text/|application/xhtml\+xml) => ^(text/|application/xhtml\+xml)
mbstring.internal_encoding => no value => no value
mbstring.language => neutral => neutral
mbstring.regex_retry_limit => 1000000 => 1000000
mbstring.regex_stack_limit => 100000 => 100000
mbstring.strict_detection => Off => Off
mbstring.substitute_character => no value => no value

mongodb

MongoDB support => enabled
MongoDB extension version => 1.20.1
MongoDB extension stability => stable
libbson bundled version => 1.28.1
libmongoc bundled version => 1.28.1
libmongoc SSL => enabled
libmongoc SSL library => OpenSSL
libmongoc crypto => enabled
libmongoc crypto library => libcrypto
libmongoc crypto system profile => disabled
libmongoc SASL => enabled
libmongoc SRV => enabled
libmongoc compression => enabled
libmongoc compression snappy => disabled
libmongoc compression zlib => enabled
libmongoc compression zstd => enabled
libmongocrypt bundled version => 1.11.0
libmongocrypt crypto => enabled
libmongocrypt crypto library => libcrypto
crypt_shared library version => unknown

Directive => Local Value => Master Value
mongodb.debug => no value => no value

mysqli

MysqlI Support => enabled
Client API library version => mysqlnd 8.3.15
Active Persistent Links => 0
Inactive Persistent Links => 0
Active Links => 0

Directive => Local Value => Master Value
mysqli.allow_local_infile => Off => Off
mysqli.allow_persistent => On => On
mysqli.default_host => no value => no value
mysqli.default_port => 3306 => 3306
mysqli.default_pw => no value => no value
mysqli.default_socket => /run/mysqld/mysqld.sock => /run/mysqld/mysqld.sock
mysqli.default_user => no value => no value
mysqli.local_infile_directory => no value => no value
mysqli.max_links => Unlimited => Unlimited
mysqli.max_persistent => Unlimited => Unlimited
mysqli.rollback_on_cached_plink => Off => Off

mysqlnd

mysqlnd => enabled
Version => mysqlnd 8.3.15
Compression => supported
core SSL => supported
extended SSL => supported
Command buffer size => 4096
Read buffer size => 32768
Read timeout => 86400
Collecting statistics => Yes
Collecting memory statistics => No
Tracing => n/a
Loaded plugins => mysqlnd,debug_trace,auth_plugin_mysql_native_password,auth_plugin_mysql_clear_password,auth_plugin_caching_sha2_password,auth_plugin_sha256_password
API Extensions => mysqli,pdo_mysql

odbc

ODBC Support => enabled
Active Persistent Links => 0
Active Links => 0
ODBC library => unixODBC
ODBCVER => 0x0380
ODBC_CFLAGS =>  
ODBC_LFLAGS =>  
ODBC_LIBS => -lodbc

Directive => Local Value => Master Value
odbc.allow_persistent => On => On
odbc.check_persistent => On => On
odbc.default_cursortype => Static cursor => Static cursor
odbc.default_db => no value => no value
odbc.default_pw => no value => no value
odbc.default_user => no value => no value
odbc.defaultbinmode => return as is => return as is
odbc.defaultlrl => return up to 4096 bytes => return up to 4096 bytes
odbc.max_links => Unlimited => Unlimited
odbc.max_persistent => Unlimited => Unlimited

openssl

OpenSSL support => enabled
OpenSSL Library Version => OpenSSL 3.4.0 22 Oct 2024
OpenSSL Header Version => OpenSSL 3.4.0 22 Oct 2024
Openssl default config => /etc/ssl/openssl.cnf

Directive => Local Value => Master Value
openssl.cafile => no value => no value
openssl.capath => no value => no value

pcntl

pcntl support => enabled

pcre

PCRE (Perl Compatible Regular Expressions) Support => enabled
PCRE Library Version => 10.44 2024-06-07
PCRE Unicode Version => 15.0.0
PCRE JIT Support => enabled
PCRE JIT Target => x86 64bit (little endian + unaligned)

Directive => Local Value => Master Value
pcre.backtrack_limit => 1000000 => 1000000
pcre.jit => On => On
pcre.recursion_limit => 100000 => 100000

PDO

PDO support => enabled
PDO drivers => dblib, mysql, odbc, pgsql, sqlite

pdo_dblib

PDO Driver for FreeTDS/Sybase DB-lib => enabled
Flavour => freetds

pdo_mysql

PDO Driver for MySQL => enabled
Client API version => mysqlnd 8.3.15

Directive => Local Value => Master Value
pdo_mysql.default_socket => /run/mysqld/mysqld.sock => /run/mysqld/mysqld.sock

PDO_ODBC

PDO Driver for ODBC (unixODBC) => enabled
ODBC Connection Pooling => Enabled, strict matching

pdo_pgsql

PDO Driver for PostgreSQL => enabled
PostgreSQL(libpq) Version => 17.2

pdo_sqlite

PDO Driver for SQLite 3.x => enabled
SQLite Library => 3.48.0

pgsql

PostgreSQL Support => enabled
PostgreSQL (libpq) Version => 17.2
Multibyte character support => enabled
Active Persistent Links => 0
Active Links => 0

Directive => Local Value => Master Value
pgsql.allow_persistent => On => On
pgsql.auto_reset_persistent => Off => Off
pgsql.ignore_notice => Off => Off
pgsql.log_notice => Off => Off
pgsql.max_links => Unlimited => Unlimited
pgsql.max_persistent => Unlimited => Unlimited

Phar

Phar: PHP Archive support => enabled
Phar API version => 1.1.1
Phar-based phar archives => enabled
Tar-based phar archives => enabled
ZIP-based phar archives => enabled
gzip compression => enabled
bzip2 compression => enabled
Native OpenSSL support => enabled


Phar based on pear/PHP_Archive, original concept by Davey Shafik.
Phar fully realized by Gregory Beaver and Marcus Boerger.
Portions of tar implementation Copyright (c) 2003-2009 Tim Kientzle.
Directive => Local Value => Master Value
phar.cache_list => no value => no value
phar.readonly => On => On
phar.require_hash => On => On

posix

POSIX support => enabled

pspell

PSpell Support => enabled

random

Version => 8.3.15

readline

Readline Support => enabled
Readline library => 8.2

Directive => Local Value => Master Value
cli.pager => no value => no value
cli.prompt => \b \>  => \b \> 

Reflection

Reflection => enabled

session

Session Support => enabled
Registered save handlers => files user 
Registered serializer handlers => php_serialize php php_binary 

Directive => Local Value => Master Value
session.auto_start => Off => Off
session.cache_expire => 180 => 180
session.cache_limiter => nocache => nocache
session.cookie_domain => no value => no value
session.cookie_httponly => Off => Off
session.cookie_lifetime => 0 => 0
session.cookie_path => / => /
session.cookie_samesite => no value => no value
session.cookie_secure => Off => Off
session.gc_divisor => 1000 => 1000
session.gc_maxlifetime => 1440 => 1440
session.gc_probability => 1 => 1
session.lazy_write => On => On
session.name => PHPSESSID => PHPSESSID
session.referer_check => no value => no value
session.save_handler => files => files
session.save_path => no value => no value
session.serialize_handler => php => php
session.sid_bits_per_character => 5 => 5
session.sid_length => 26 => 26
session.upload_progress.cleanup => On => On
session.upload_progress.enabled => On => On
session.upload_progress.freq => 1% => 1%
session.upload_progress.min_freq => 1 => 1
session.upload_progress.name => PHP_SESSION_UPLOAD_PROGRESS => PHP_SESSION_UPLOAD_PROGRESS
session.upload_progress.prefix => upload_progress_ => upload_progress_
session.use_cookies => On => On
session.use_only_cookies => On => On
session.use_strict_mode => Off => Off
session.use_trans_sid => Off => Off

shmop

shmop support => enabled

SimpleXML

SimpleXML support => enabled
Schema support => enabled

snmp

NET-SNMP Support => enabled
NET-SNMP Version => 5.9.4.pre2

soap

Soap Client => enabled
Soap Server => enabled

Directive => Local Value => Master Value
soap.wsdl_cache => 1 => 1
soap.wsdl_cache_dir => /tmp => /tmp
soap.wsdl_cache_enabled => On => On
soap.wsdl_cache_limit => 5 => 5
soap.wsdl_cache_ttl => 86400 => 86400

sockets

Sockets Support => enabled

sodium

sodium support => enabled
libsodium headers version => 1.0.20
libsodium library version => 1.0.20

SPL

SPL support => enabled
Interfaces => OuterIterator, RecursiveIterator, SeekableIterator, SplObserver, SplSubject
Classes => AppendIterator, ArrayIterator, ArrayObject, BadFunctionCallException, BadMethodCallException, CachingIterator, CallbackFilterIterator, DirectoryIterator, DomainException, EmptyIterator, FilesystemIterator, FilterIterator, GlobIterator, InfiniteIterator, InvalidArgumentException, IteratorIterator, LengthException, LimitIterator, LogicException, MultipleIterator, NoRewindIterator, OutOfBoundsException, OutOfRangeException, OverflowException, ParentIterator, RangeException, RecursiveArrayIterator, RecursiveCachingIterator, RecursiveCallbackFilterIterator, RecursiveDirectoryIterator, RecursiveFilterIterator, RecursiveIteratorIterator, RecursiveRegexIterator, RecursiveTreeIterator, RegexIterator, RuntimeException, SplDoublyLinkedList, SplFileInfo, SplFileObject, SplFixedArray, SplHeap, SplMinHeap, SplMaxHeap, SplObjectStorage, SplPriorityQueue, SplQueue, SplStack, SplTempFileObject, UnderflowException, UnexpectedValueException

sqlite3

SQLite3 support => enabled
SQLite Library => 3.48.0

Directive => Local Value => Master Value
sqlite3.defensive => On => On
sqlite3.extension_dir => no value => no value

standard

Dynamic Library Support => enabled
Path to sendmail => /usr/bin/sendmail -t -i

Directive => Local Value => Master Value
assert.active => On => On
assert.bail => Off => Off
assert.callback => no value => no value
assert.exception => On => On
assert.warning => On => On
auto_detect_line_endings => Off => Off
default_socket_timeout => 60 => 60
from => no value => no value
session.trans_sid_hosts => no value => no value
session.trans_sid_tags => a=href,area=href,frame=src,form= => a=href,area=href,frame=src,form=
unserialize_max_depth => 4096 => 4096
url_rewriter.hosts => no value => no value
url_rewriter.tags => form= => form=
user_agent => no value => no value

sysvmsg

sysvmsg support => enabled

sysvsem

sysvsem support => enabled

sysvshm

sysvshm support => enabled

tidy

Tidy support => enabled
libTidy Version => 5.7.45
libTidy Release => 2020/11/30

Directive => Local Value => Master Value
tidy.clean_output => Off => Off
tidy.default_config => no value => no value

tokenizer

Tokenizer Support => enabled

xdebug

__   __   _      _                 
\ \ / /  | |    | |                
 \ V / __| | ___| |__  _   _  __ _ 
  > < / _` |/ _ \ '_ \| | | |/ _` |
 / . \ (_| |  __/ |_) | |_| | (_| |
/_/ \_\__,_|\___|_.__/ \__,_|\__, |
                              __/ |
                             |___/ 

Version => 3.4.1
Support Xdebug on Patreon, GitHub, or as a business: https://xdebug.org/support

             Enabled Features (through 'xdebug.mode' setting)             
Feature => Enabled/Disabled
Development Helpers => ✘ disabled
Coverage => ✘ disabled
GC Stats => ✘ disabled
Profiler => ✘ disabled
Step Debugger => ✔ enabled
Tracing => ✘ disabled

                            Optional Features                            
Compressed File Support => yes (gzip)
Clock Source => clock_gettime
TSC Clock Source => available
'xdebug://gateway' pseudo-host support => yes
'xdebug://nameserver' pseudo-host support => yes
Systemd Private Temp Directory => not enabled

Debugger => enabled
IDE Key =>  

Directive => Local Value => Master Value
xdebug.auto_trace => (setting renamed in Xdebug 3) => (setting renamed in Xdebug 3)
xdebug.cli_color => 0 => 0
xdebug.client_discovery_header => HTTP_X_FORWARDED_FOR,REMOTE_ADDR => HTTP_X_FORWARDED_FOR,REMOTE_ADDR
xdebug.client_host => 127.0.0.1 => 127.0.0.1
xdebug.client_port => 9003 => 9003
xdebug.cloud_id => no value => no value
xdebug.collect_assignments => Off => Off
xdebug.collect_includes => (setting removed in Xdebug 3) => (setting removed in Xdebug 3)
xdebug.collect_params => On => On
xdebug.collect_return => Off => Off
xdebug.collect_vars => (setting removed in Xdebug 3) => (setting removed in Xdebug 3)
xdebug.connect_timeout_ms => 200 => 200
xdebug.control_socket => time: 25ms => time: 25ms
xdebug.coverage_enable => (setting renamed in Xdebug 3) => (setting renamed in Xdebug 3)
xdebug.default_enable => (setting renamed in Xdebug 3) => (setting renamed in Xdebug 3)
xdebug.discover_client_host => Off => Off
xdebug.dump.COOKIE => no value => no value
xdebug.dump.ENV => no value => no value
xdebug.dump.FILES => no value => no value
xdebug.dump.GET => no value => no value
xdebug.dump.POST => no value => no value
xdebug.dump.REQUEST => no value => no value
xdebug.dump.SERVER => no value => no value
xdebug.dump.SESSION => no value => no value
xdebug.dump_globals => On => On
xdebug.dump_once => On => On
xdebug.dump_undefined => Off => Off
xdebug.file_link_format => no value => no value
xdebug.filename_format => no value => no value
xdebug.force_display_errors => Off => Off
xdebug.force_error_reporting => 0 => 0
xdebug.gc_stats_enable => (setting renamed in Xdebug 3) => (setting renamed in Xdebug 3)
xdebug.gc_stats_output_dir => (setting renamed in Xdebug 3) => (setting renamed in Xdebug 3)
xdebug.gc_stats_output_name => gcstats.%p => gcstats.%p
xdebug.halt_level => 0 => 0
xdebug.idekey => no value => no value
xdebug.log => no value => no value
xdebug.log_level => 7 => 7
xdebug.max_nesting_level => 512 => 512
xdebug.max_stack_frames => -1 => -1
xdebug.mode => debug => debug
xdebug.output_dir => /tmp => /tmp
xdebug.overload_var_dump => (setting removed in Xdebug 3) => (setting removed in Xdebug 3)
xdebug.profiler_append => Off => Off
xdebug.profiler_enable => (setting renamed in Xdebug 3) => (setting renamed in Xdebug 3)
xdebug.profiler_enable_trigger => (setting renamed in Xdebug 3) => (setting renamed in Xdebug 3)
xdebug.profiler_enable_trigger_value => (setting renamed in Xdebug 3) => (setting renamed in Xdebug 3)
xdebug.profiler_output_dir => (setting renamed in Xdebug 3) => (setting renamed in Xdebug 3)
xdebug.profiler_output_name => cachegrind.out.%p => cachegrind.out.%p
xdebug.remote_autostart => (setting renamed in Xdebug 3) => (setting renamed in Xdebug 3)
xdebug.remote_connect_back => (setting renamed in Xdebug 3) => (setting renamed in Xdebug 3)
xdebug.remote_enable => (setting renamed in Xdebug 3) => (setting renamed in Xdebug 3)
xdebug.remote_host => (setting renamed in Xdebug 3) => (setting renamed in Xdebug 3)
xdebug.remote_log => (setting renamed in Xdebug 3) => (setting renamed in Xdebug 3)
xdebug.remote_log_level => (setting renamed in Xdebug 3) => (setting renamed in Xdebug 3)
xdebug.remote_mode => (setting renamed in Xdebug 3) => (setting renamed in Xdebug 3)
xdebug.remote_port => (setting renamed in Xdebug 3) => (setting renamed in Xdebug 3)
xdebug.remote_timeout => (setting renamed in Xdebug 3) => (setting renamed in Xdebug 3)
xdebug.scream => Off => Off
xdebug.show_error_trace => Off => Off
xdebug.show_exception_trace => Off => Off
xdebug.show_local_vars => Off => Off
xdebug.show_mem_delta => (setting removed in Xdebug 3) => (setting removed in Xdebug 3)
xdebug.start_upon_error => default => default
xdebug.start_with_request => default => default
xdebug.trace_enable_trigger => (setting renamed in Xdebug 3) => (setting renamed in Xdebug 3)
xdebug.trace_enable_trigger_value => (setting renamed in Xdebug 3) => (setting renamed in Xdebug 3)
xdebug.trace_format => 0 => 0
xdebug.trace_options => 0 => 0
xdebug.trace_output_dir => (setting renamed in Xdebug 3) => (setting renamed in Xdebug 3)
xdebug.trace_output_name => trace.%c => trace.%c
xdebug.trigger_value => no value => no value
xdebug.use_compression => 1 => 1
xdebug.var_display_max_children => 128 => 128
xdebug.var_display_max_data => 512 => 512
xdebug.var_display_max_depth => 3 => 3

xml

XML Support => active
XML Namespace Support => active
libxml2 Version => 2.13.5

xmlreader

XMLReader => enabled

xmlwriter

XMLWriter => enabled

xsl

XSL => enabled
libxslt Version => 1.1.42
libxslt compiled against libxml Version => 2.13.5
EXSLT => enabled
libexslt Version => 0.8.23

Zend OPcache

Opcode Caching => Up and Running
Optimization => Disabled
SHM Cache => Enabled
File Cache => Disabled
JIT => Disabled
Startup => OK
Shared memory model => mmap
Cache hits => 0
Cache misses => 0
Used memory => 9167936
Free memory => 125049792
Wasted memory => 0
Interned Strings Used memory => 2614440
Interned Strings Free memory => 5774168
Cached scripts => 0
Cached keys => 0
Max keys => 16229
OOM restarts => 0
Hash keys restarts => 0
Manual restarts => 0
Start time => 2025-01-30T11:01:41+0000
Last restart time => none
Last force restart time => none

Directive => Local Value => Master Value
opcache.blacklist_filename => no value => no value
opcache.dups_fix => Off => Off
opcache.enable => On => On
opcache.enable_cli => On => On
opcache.enable_file_override => Off => Off
opcache.error_log => no value => no value
opcache.file_cache => no value => no value
opcache.file_cache_consistency_checks => On => On
opcache.file_cache_only => Off => Off
opcache.file_update_protection => 2 => 2
opcache.force_restart_timeout => 180 => 180
opcache.huge_code_pages => Off => Off
opcache.interned_strings_buffer => 8 => 8
opcache.jit => tracing => tracing
opcache.jit_bisect_limit => 0 => 0
opcache.jit_blacklist_root_trace => 16 => 16
opcache.jit_blacklist_side_trace => 8 => 8
opcache.jit_buffer_size => 0 => 0
opcache.jit_debug => 0 => 0
opcache.jit_hot_func => 127 => 127
opcache.jit_hot_loop => 64 => 64
opcache.jit_hot_return => 8 => 8
opcache.jit_hot_side_exit => 8 => 8
opcache.jit_max_exit_counters => 8192 => 8192
opcache.jit_max_loop_unrolls => 8 => 8
opcache.jit_max_polymorphic_calls => 2 => 2
opcache.jit_max_recursive_calls => 2 => 2
opcache.jit_max_recursive_returns => 2 => 2
opcache.jit_max_root_traces => 1024 => 1024
opcache.jit_max_side_traces => 128 => 128
opcache.jit_max_trace_length => 1024 => 1024
opcache.jit_prof_threshold => 0.005 => 0.005
opcache.lockfile_path => /tmp => /tmp
opcache.log_verbosity_level => 1 => 1
opcache.max_accelerated_files => 10000 => 10000
opcache.max_file_size => 0 => 0
opcache.max_wasted_percentage => 5 => 5
opcache.memory_consumption => 128 => 128
opcache.opt_debug_level => 0 => 0
opcache.optimization_level => 0 => 0x7FFEBFFF
opcache.preferred_memory_model => no value => no value
opcache.preload => no value => no value
opcache.preload_user => no value => no value
opcache.protect_memory => Off => Off
opcache.record_warnings => Off => Off
opcache.restrict_api => no value => no value
opcache.revalidate_freq => 2 => 2
opcache.revalidate_path => Off => Off
opcache.save_comments => On => On
opcache.use_cwd => On => On
opcache.validate_permission => Off => Off
opcache.validate_root => Off => Off
opcache.validate_timestamps => On => On

zip

Zip => enabled
Zip version => 1.22.3
Libzip version => 1.11.2
BZIP2 compression => Yes
XZ compression => Yes
ZSTD compression => Yes
AES-128 encryption => Yes
AES-192 encryption => Yes
AES-256 encryption => Yes

zlib

ZLib Support => enabled
Stream Wrapper => compress.zlib://
Stream Filter => zlib.inflate, zlib.deflate
Compiled Version => 1.3.1
Linked Version => 1.3.1

Directive => Local Value => Master Value
zlib.output_compression => Off => Off
zlib.output_compression_level => -1 => -1
zlib.output_handler => no value => no value

Additional Modules

Module Name

Environment

Variable => Value
SHELL => /usr/bin/bash
SESSION_MANAGER => local/user:@/tmp/.ICE-unix/3527,unix/user:/tmp/.ICE-unix/3527
COLORTERM => truecolor
NVM_INC => /home/user/.nvm/versions/node/v16.20.2/include/node
XDG_MENU_PREFIX => gnome-
GNOME_KEYRING_CONTROL => /run/user/1000/keyring
MEMORY_PRESSURE_WRITE => c29tZSAyMDAwMDAgMjAwMDAwMAA=
DESKTOP_SESSION => gnome
PWD => /home/user/Code/IBS-test-task
LOGNAME => user
XDG_SESSION_DESKTOP => gnome
XDG_SESSION_TYPE => wayland
SYSTEMD_EXEC_PID => 3688
XAUTHORITY => /run/user/1000/.mutter-Xwaylandauth.Q3FC12
MOTD_SHOWN => pam
GDM_LANG => en_US.UTF-8
HOME => /home/user
USERNAME => user
LANG => en_US.UTF-8
XDG_CURRENT_DESKTOP => GNOME
MEMORY_PRESSURE_WATCH => /sys/fs/cgroup/user.slice/user-1000.slice/user@1000.service/session.slice/org.gnome.SettingsDaemon.MediaKeys.service/memory.pressure
VTE_VERSION => 7803
WAYLAND_DISPLAY => wayland-0
GNOME_TERMINAL_SCREEN => /org/gnome/Terminal/screen/8d6eea63_f511_4780_92a0_40fa29f7534c
NVM_DIR => /home/user/.nvm
GNOME_SETUP_DISPLAY => :1
XDG_SESSION_CLASS => user
TERM => xterm-256color
USER => user
GNOME_TERMINAL_SERVICE => :1.91
DISPLAY => :0
SHLVL => 1
NVM_CD_FLAGS =>  
XDG_RUNTIME_DIR => /run/user/1000
DEBUGINFOD_URLS => https://debuginfod.archlinux.org 
XDG_DATA_DIRS => /home/user/.local/share/flatpak/exports/share:/var/lib/flatpak/exports/share:/usr/local/share/:/usr/share/
PATH => /home/user/.nvm/versions/node/v16.20.2/bin:/usr/local/bin:/usr/bin:/usr/local/sbin:/var/lib/flatpak/exports/bin:/usr/lib/jvm/default/bin:/usr/bin/site_perl:/usr/bin/vendor_perl:/usr/bin/core_perl:/home/user/.local/share/JetBrains/Toolbox/scripts
GDMSESSION => gnome
DBUS_SESSION_BUS_ADDRESS => unix:path=/run/user/1000/bus
NVM_BIN => /home/user/.nvm/versions/node/v16.20.2/bin
MAIL => /var/spool/mail/user
OLDPWD => /home/user
_ => /usr/bin/php

PHP Variables

Variable => Value
$_SERVER['SHELL'] => /usr/bin/bash
$_SERVER['SESSION_MANAGER'] => local/Arch:@/tmp/.ICE-unix/3527,unix/Arch:/tmp/.ICE-unix/3527
$_SERVER['COLORTERM'] => truecolor
$_SERVER['NVM_INC'] => /home/user/.nvm/versions/node/v16.20.2/include/node
$_SERVER['XDG_MENU_PREFIX'] => gnome-
$_SERVER['GNOME_KEYRING_CONTROL'] => /run/user/1000/keyring
$_SERVER['MEMORY_PRESSURE_WRITE'] => c29tZSAyMDAwMDAgMjAwMDAwMAA=
$_SERVER['DESKTOP_SESSION'] => gnome
$_SERVER['PWD'] => /home/user/Code/IBS-test-task
$_SERVER['LOGNAME'] => user
$_SERVER['XDG_SESSION_DESKTOP'] => gnome
$_SERVER['XDG_SESSION_TYPE'] => wayland
$_SERVER['SYSTEMD_EXEC_PID'] => 3688
$_SERVER['XAUTHORITY'] => /run/user/1000/.mutter-Xwaylandauth.Q3FC12
$_SERVER['MOTD_SHOWN'] => pam
$_SERVER['GDM_LANG'] => en_US.UTF-8
$_SERVER['HOME'] => /home/user
$_SERVER['USERNAME'] => user
$_SERVER['LANG'] => en_US.UTF-8
$_SERVER['XDG_CURRENT_DESKTOP'] => GNOME
$_SERVER['MEMORY_PRESSURE_WATCH'] => /sys/fs/cgroup/user.slice/user-1000.slice/user@1000.service/session.slice/org.gnome.SettingsDaemon.MediaKeys.service/memory.pressure
$_SERVER['VTE_VERSION'] => 7803
$_SERVER['WAYLAND_DISPLAY'] => wayland-0
$_SERVER['GNOME_TERMINAL_SCREEN'] => /org/gnome/Terminal/screen/8d6eea63_f511_4780_92a0_40fa29f7534c
$_SERVER['NVM_DIR'] => /home/user/.nvm
$_SERVER['GNOME_SETUP_DISPLAY'] => :1
$_SERVER['XDG_SESSION_CLASS'] => user
$_SERVER['TERM'] => xterm-256color
$_SERVER['USER'] => user
$_SERVER['GNOME_TERMINAL_SERVICE'] => :1.91
$_SERVER['DISPLAY'] => :0
$_SERVER['SHLVL'] => 1
$_SERVER['NVM_CD_FLAGS'] => 
$_SERVER['XDG_RUNTIME_DIR'] => /run/user/1000
$_SERVER['DEBUGINFOD_URLS'] => https://debuginfod.archlinux.org 
$_SERVER['XDG_DATA_DIRS'] => /home/user/.local/share/flatpak/exports/share:/var/lib/flatpak/exports/share:/usr/local/share/:/usr/share/
$_SERVER['PATH'] => /home/user/.nvm/versions/node/v16.20.2/bin:/usr/local/bin:/usr/bin:/usr/local/sbin:/var/lib/flatpak/exports/bin:/usr/lib/jvm/default/bin:/usr/bin/site_perl:/usr/bin/vendor_perl:/usr/bin/core_perl:/home/user/.local/share/JetBrains/Toolbox/scripts
$_SERVER['GDMSESSION'] => gnome
$_SERVER['DBUS_SESSION_BUS_ADDRESS'] => unix:path=/run/user/1000/bus
$_SERVER['NVM_BIN'] => /home/user/.nvm/versions/node/v16.20.2/bin
$_SERVER['MAIL'] => /var/spool/mail/user
$_SERVER['OLDPWD'] => /home/user
$_SERVER['_'] => /usr/bin/php
$_SERVER['PHP_SELF'] => 
$_SERVER['SCRIPT_NAME'] => 
$_SERVER['SCRIPT_FILENAME'] => 
$_SERVER['PATH_TRANSLATED'] => 
$_SERVER['DOCUMENT_ROOT'] => 
$_SERVER['REQUEST_TIME_FLOAT'] => 1738234901.3406
$_SERVER['REQUEST_TIME'] => 1738234901
$_SERVER['argv'] => Array
(
)

$_SERVER['argc'] => 0

PHP License
This program is free software; you can redistribute it and/or modify
it under the terms of the PHP License as published by the PHP Group
and included in the distribution in the file:  LICENSE

This program is distributed in the hope that it will be useful,
but WITHOUT ANY WARRANTY; without even the implied warranty of
MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.

If you did not receive a copy of the PHP license, or have any
questions about PHP licensing, please contact license@php.net.
```
***
