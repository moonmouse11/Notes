# Functions
***
## Non-sorted
- `parse_url(string $url, int $component = -1)` - парсит адресную строку, для использования лучше подсмотреть компоненты.
- `sprintf(string $format, mixed ...$values): string` - форматирует и возвращает строку.
- `exit(string $status = ?): void` - выводит сообщение и останавливает работу скрипта.
- `die(string $message = ?): void` - как и функция выше, выводит сообщение и останавливает работу скрипта.
- `array_keys(array $array, mixed $filter_value, bool $strict = false): array` - функция возвращает массив ключей переданного в аргументы массива.
- `header(string $header, bool $replace = true, int $response_code = 0): void` - функция отправляет необработанный заголовок страницы.
- `compact(array|string $var_name, array|string ...$var_names): array`  - функция создает массив, содержащий переменные и их значения. _**(Не могут быть переданы суперглобальные массивы)**_.
- `extract(array &$array, int $flags = EXTR_OVERWRITE, string $prefix = ""): int` - функция обратна функции выше, извлекает переменные из массива.
- `str_contains(string $haystack, string $needle): bool` - функция  определяет, содержит ли строка заданную подстроку.
- `strlen(string $string): int` - функция вовозращает длинну строки.
- `htmlspecialchars(string $string, int $flags = ENT_QUOTES | ENT_SUBSTITUTE | ENT_HTML401, ?string $encoding = null, bool $double_encode = true): string`  - функция преобразовывает специальные символы в HTML.
- `file_get_contents(string $filename, bool $use_include_path = false, ?resource $context = null, int $offset = 0, ?int $length = null): string|false` - функция читает заданный файл и возвращает содержимое в виде строки.
- `fgetcsv(resource $stream, ?int $length = null, string $separator = ",", string $enclosure = "\"", string $escape = "\\"): array|false` - функция читает строку из файла и производит раззбор данных CSV.
- `mime_content_type(resource|string $filename): string|false` - функция определяет MIME-тип содержимого файла.
- `preg_split(string $pattern, string $subject, int $limit = -1, int $flags = 0): array|false` - функция разбивает строку по указанному регулярному выражению, возвращает массив.
- `str_ends_with(string $haystack, string $needle): bool` - функция проверяет конец строки с заданной подстрокой.
- `strpos(string $haystack, string $needle, int $offset = 0): int|false` - функция возвращает позицию первого вхождения подстроки в строку.
- `fastcgi_finish_request(): bool` - функция сбрасывает все запрошенные данные клиенту и завершает обработку запроса.
- `http_response_code(int $response_code = 0): int|bool` - функция получает или задает коды ответов HTTP.
- `filter_var(mixed $value, int $filter = FILTER_DEFAULT, array|int $options = 0): mixed` - функция фильтрует переменную с помощью определённого фильтра.
- `http_build_query(array|object $data, string $numeric_prefix = "", ?string $arg_separator = null, int $encoding_type = PHP_QUERY_RFC1738): string` - функция генерирует URL-кодированную строку запроса из предоставленного ассоциативного (или индексированного) массива.
- `mb_strtoupper(string $string, ?string $encoding = null): string` - функция приводит строку в верхнему регистру. _**(Для кодировки больше одного байта)**_
- `mb_strtolower(string $string, ?string $encoding = null): string` - функция приводит строку к нижнему реистру. _**(Для кодировки больше одного байта)**_
- `array_key_exists(string|int|float|bool|resource|null $key, array $array): bool` - функция проверяет, содежит ли массив зпдпнный ключ или индекс.
- `call_user_func(callable $callback, mixed ...$args): mixed` - функция вызывает callback-функцию, переданную первым параметром, и передаёт остальные параметры в качестве аргументов. 
- `call_user_func_array(callable $callback, array $args): mixed` - функция вызывает callback-функцию `callback`, с параметрами из массива `args`.
- `func_get_args(): array` - функция возвращает массив, который содержит список аргументов функции.
- `func_num_args(): int` - возвращает количество аргументов, переданных функции.
- `func_get_arg(int $position): mixed` - функция возвращает указанный аргумент, из переданных функции.
- `function_exists(string $function): bool` - проверяет существование указанной в аргументе функции.
- `get_defined_functions(bool $exclude_disabled = true): array` - возвращает массив всех определенных функций.
- `register_shutdown_function(callable $callback, mixed ...$args): void` - регистрирует функцию, которая выполняется при завершении работы скрипта.
- `register_tick_function(callable $callback, mixed ...$args): bool` - регистрирует функцию, которая должна вызываться при каждом "тике".
- `unregister_tick_function(callable $callback): void` - обратна функции выше.
- `forward_static_call(callable $callback, mixed ...$args): mixed` - Вызов пользовательской функции или метода, заданные в параметре callback с последующими аргументами. Эта функция должна вызываться в контексте метода и не может быть вызвана вне класса. Она использует позднее статическое связывание.
- `forward_static_call_array(callable $callback, array $args): mixed` - Вызывает пользовательскую функцию или метод, заданные в параметре callback. Эта функция должна вызываться в контексте метода и не может быть вызвана вне класса. Она использует позднее статическое связывание. Все аргументы пересылаются в метод по значению и в виде массива, аналогично `call_user_func_array()`.
- `eval(string $code): mixed` - функция расценивает строку как PHP код и выполняет его.
- `set_error_handler()` -
- `trigger_error()` - 
- `iterator_to_array()` - 
- `error_reporting()` - 
***
## Math
- `abs(int|float $num): int|float` - функция возвращает абсолютную величину (Модуль).
- `cos(float $num): float` - функция вычисляет косинус заданного числа.
- `cosh(float $num): float` - функция вычисляет гиперболический косинус.
- 
- `acos(float $num): float` - функция вычисляет арккосинус.
- `acosh(float $num): float` - вычисляет гиперболический арккосинус.
- `asin(float $num): float` - функция вычисляет арксинус.
- `asinh(float $num): float` - функция вычисляет гиперболический арксинус.
- `atan2(float $y, float $x): float` - вычисляет арктангенс двух заданных переменных.
- `atan(float $num): float` - функция вычисляет арктангенс заданной переменной.
- `atanh(float $num): float` - функция вычисляет гиперболический арктангенс.
- `base_convert(string $num, int $from_base, int $to_base): string` - функция преобразовывает числа между произвольными системами счисления.
- `bindec(string $binary_string): int|float` - функция перобразовывает двоичное число в десятичное.
- `decbin(int $num): string` - обратна функции выше, преобразовывает число из десятичной формы в двоичную.
- `dechex(int $num): string` - преобразовывает число из десятичной системы, в шестнадцатиричную.
- `decoct(int $num): string` - преобразовывает число из десятичной системы в восьмиричную.
- `deg2rad(float $num): float` - функция преобразовывет перезанное значение из градусов в радианы.
- `exp(float $num): float` - вычисляет степень числа `e`. (Количество нулей в дробной части).
- `expm1(float $num): float` - функция возвращает результат `exp($num) - 1`, вычисленный так, чтобы он был точным, даже если значение числа близко к нулю.
- `ceil(int|float $num): float` - функция округляет дробное число в большую сторону.
- `floor(int|float $num): float` - функция округляет дробное число в меньшую сторону.
- `round(int|float $num, int $precision = 0, int $mode = PHP_ROUND_HALF_UP): float` - функция округляет число с плавающей точкой до указанного знака.
- `intdiv(int $num1, int $num2): int` - функция делит два числе без остатка.
- `fdiv(float $num1, float $num2): float` - функция делит одно число на другое по правилам стандарта `IEEE 754`.
- `fmod(float $num1, float $num2): float` - функция возвращает дробный остаток от деления по модулю.
- 
***
## Password
- `password_hash(string $password, string|int|null $algo, array $options = []): string` - функция создает хэш-пароля, используя необратимый алгоритм хеширования.
- `password_algos(): array` - функция возвращает полный список зарегистрированных идентификаторов алгоритмов шифрований.
- `password_get_info(string $hash): array` - возвращает информацию о заданном хэше.
- `password_needs_rehash(string $hash, string|int|null $algo, array $options = []): bool` - валидация переданного хэша по переданному в аргументы алгоритму.
- `password_verify(string $password, string $hash): bool` - функция проверяет соотвествие пароля хэшу.
- `crypt(string $string, string $salt): string` - функция возвращает хешированную строку, полученную с помощью стандартного алгоритма UNIX, основанного на DES или другого алгоритма.
***
## Session
- `session_abort(): bool` - функция отменяет изменения в массиве сессии и завершает ее.
- `session_cache_expire(?int $value = null): int|false` - получает и устанавливает срок действия текущего кэша.
- `session_cache_limiter(?string $value = null): string|false` - функция получает, или устанавливает текуший режим кэширования.
- `session_write_close(): bool` - функция записывает данные сесси и завершает ее.
- `session_create_id(string $prefix = ""): string|false` - создает новый идентификатор сессии.
- `session_decode(string $data): bool` - декодирует данные сесии из строки.
- `session_destroy(): bool` - уничтожает данные сессии.
- `session_encode(): string|false` - кодирует данные текущей сессии в формате строки.
- `session_gc(): int|false` - выполняет сборку мусора данных сессии.
- `session_get_cookie_params(): array` - функция возвращает cookie сессии.
- `session_id(?string $id = null): string|false` - получает и устанавливает идентификатор сессии.
- `session_module_name(?string $module = null): string|false` - возвращает и устанавливает модуль текущей сессии.
- `session_name(?string $name = null): string|false` - получает или устанавливает имя текузей сессии.
- `session_regenerate_id(bool $delete_old_session = false): bool` - генерирует и обновляет идентификатор текущей сессии.
- `session_register_shutdown(): void` - функция заверщает сессию.
- `session_reset(): bool` - реинициализирует сессию новыми значениями.
- `session_save_path(?string $path = null): string|false` - получает или устанавливает путь сохранения сессии.
- `session_set_cookie_params(int $lifetime_or_options, ?string $path = null, ?string $domain = null, ?bool $secure = null,?bool $httponly = null): bool` - кстанавливает параметры сессионной cookie.
- `session_set_save_handler(callable $open, callable $close, callable $read, callable $write, callable $destroy, callable $gc, callable $create_sid = ?, callable $validate_sid = ?, callable $update_timestamp = ?): bool` - функция устанавливает пользовательские обработчики хранения сессии.
- `session_start(array $options = []): bool` - функция стартует сесиию, или возобновляет предыдущую.
- `session_status(): int` - возвращает текущее состояние сессии.
- `session_unset(): bool` - удаляет все переменные сессии.
***
## SPL
- `class_implements(object|string $object_or_class, bool $autoload = true): array|false` - функция возвращает список интерфейсов, реализованных в заданносм классе или интерфейсе.
- `class_parents(object|string $object_or_class, bool $autoload = true): array|false` - возвращает список родительских классов заданного класса.
- `class_uses(object|string $object_or_class, bool $autoload = true): array|false` - функция возвращает список трейтов, используемых в заданном классе.
- `iterator_apply(Traversable $iterator, callable $callback, ?array $args = null): int` - вызывает указанную функцию для каждого элемента в итераторе.
- `iterator_count(Traversable|array $iterator): int` - функция возвращает количество элементов в итераторе.
- `iterator_to_array(Traversable|array $iterator, bool $preserve_keys = true): array` - функция копирует итератор в массив.
- `spl_autoload_call(string $class): void` - функцию запускают для ручного поиска класса или интерфейса через зарегистрированные методы очереди.
- `spl_autoload_extensions(?string $file_extensions = null): string` - функция реистрирует и выводит расширения файлов для функций `spl_autoload`.
- `spl_autoload_functions(): array` - функция возвращает вписок зарегистрированных функций автозагрузки классов.
- `spl_autoload_register(?callable $callback = null, bool $throw = true, bool $prepend = false): bool` - регистрирует функцию в autoload. 
- `spl_autoload_unregister(callable $callback): bool` - отменяет регистрацию функции. Противоположна функции выше.
- `spl_autoload(string $class, ?string $file_extensions = null): void` - функция по умолчанию для автозагрузки классов.
- `spl_classes(): array` - функция возвращает доступные `spl` классы.
- `spl_object_hash(object $object): string` - функция возвращает хэш-идентификатор объекта.
- `spl_object_id(object $object): int` - функция получает целочисленный идентификатор объекта.
***
## Array
- `array_map(?callable $callback, array $array, array ...$arrays)` - применяет callback ко всем элементам переданного массива.
- `array_filter(array $array, ?callable $callback = null, int $mode = 0): array` - фильтрует элементы массива с помощью callback.
- `implode(string $separator, array $array): string` - функция объединяет элементы массива в строку.
- `explode(string $separator, string $string, int $limit = PHP_INT_MAX): array` - функция разбивает строку на массив.
- `in_array(mixed $needle, array $haystack, bool $strict = false): bool` - функция проверяет наличие заданных данных в массиве.
- `array_pop(array &$array): mixed` - извлекает последний элемент массива.
- `array_pad(array $array, int $length, mixed $value): array` - дополняет массив значениями до заданной длины.
- `count(Countable|array $value, int $mode = COUNT_NORMAL): int` - функция возвращает количество элементов в массиве.
- `sizeof(Countable|array $value, int $mode = COUNT_NORMAL): int` - функция-псевдоним `count`.
- `array_chunk(array $array, int $length, bool $preserve_keys = false): array` - функция разбивает массив на части.
- `array_walk_recursive(array|object &$array, callable $callback, mixed $arg = null): bool` - рекурсивно применяет указанную пользовательскую функцию к каждому элементу массива.
- 
***
## Function for variables
- `boolval(mixed $value): bool` - функция возвращает логическое значение переменной. 
Пример использования.
``` php
echo '0: '.(boolval(0) ? 'true' : 'false')."\n";  # false
echo '42: '.(boolval(42) ? 'true' : 'false')."\n";   # true
echo '0.0: '.(boolval(0.0) ? 'true' : 'false')."\n";   # false
echo '4.2: '.(boolval(4.2) ? 'true' : 'false')."\n";   # true
echo '"": '.(boolval("") ? 'true' : 'false')."\n";  # false
echo '"string": '.(boolval("string") ? 'true' : 'false')."\n";  # true
echo '"0": '.(boolval("0") ? 'true' : 'false')."\n";  # false
echo '"1": '.(boolval("1") ? 'true' : 'false')."\n";  # true
echo '[1, 2]: '.(boolval([1, 2]) ? 'true' : 'false')."\n";  # true
echo '[]: '.(boolval([]) ? 'true' : 'false')."\n";  # false
echo 'stdClass: '.(boolval(new stdClass) ? 'true' : 'false')."\n";  # true
```
- `debug_zval_dump(mixed $value, mixed ...$values): void` - функция сбрасывает строковое представление внутренней структуры `zval` на вывод.
- `empty(mixed $var): bool` - проверяет, пустая ли переменная.
- `floatval(mixed $value): float` - функция возвращает значение переменной в виде числа с плавающей точкой.
- `gettype(mixed $value): string` - функция возвращает тип переменной.
- `settype(mixed &$var, string $type): bool` - функция задает тип указанной переменной.
***
## Class & Objects
- `enum_exists(string $enum, bool $autoload = true): bool` - функция проверяет существование переданного в аргументы Enum.
- `__autoload(string $class): void` - функция пытается загрузить класс, переданные в аргумент. _**Устаревшая с  PHP 7.2.0**_.
- `class_alias(string $class, string $alias, bool $autoload = true): bool` - функция создает псевдоним класса.
- `class_exists(string $class, bool $autoload = true): bool` - функция проверяет объявление класса.
- `get_called_class(): string` - функция возвращает имя класса, из которого вызван статический метод.
- `get_class_methods(object|string $object_or_class): array` - функция возвращает массив с именами методов класса.
- `get_class_vars(string $class): array` - функция получает свойства классов, объявленных по умолчанию.
- `get_class(object $object = ?): string` - функция возвращает имя класса, которому принадлежит переданный метод.
- `get_declared_classes(): array` - функция возвращает массив с именами объявленных классов.
- `get_declared_interfaces(): array` - возвращает массив с именами объявленных интерфейсов.
- `get_declared_traits(): array` - возвращает массив с именами объявленных трейтов.
- `get_mangled_object_vars(object $object): array` - возвращает массив искаженных свойств объекта. (Функция возвращает массив, который содержит свойства объекта).
- `get_object_vars(object $object): array` - Возвращает видимые нестатические свойства указанного объекта `object` в соответствии с областью видимости.
- `get_parent_class(object|string $object_or_class = ?): string|false` - функция получает имя родительского класса для объекта или класса.
- `interface_exists(string $interface, bool $autoload = true): bool` - функция проверяет, определён ли указанный интерфейс.
- `is_a(mixed $object_or_class, string $class, bool $allow_string = false): bool` - функция проверяет, принадлежит ли объект к типу или подтипу.
- `is_subclass_of(mixed $object_or_class, string $class, bool $allow_string = true): bool` - функция проверяет, принадлежит ли объект к потомкам класса, или реализует ли объект или родители объекта интерфейс.
- `method_exists(object|string $object_or_class, string $method): bool` - функция проверяет наличие указанного метода у объекта.
- `property_exists(object|string $object_or_class, string $property): bool` - функция проверяет, есть ли у объекта или класса свойство.
- `trait_exists(string $trait, bool $autoload = true): bool` - проверяет, существует ли трейт.
***
## Filter functions
- `filter_has_var(int $input_type, string $var_name): bool` - функция проверяет сузествование переменной указанного типа.
- `filter_id(string $name): int|false` - функция возвращает идентификато поля.
- `filter_input_array(int $type, array|int $options = FILTER_DEFAULT, bool $add_empty = true): array|false|null` - получает несколько переменных извне PHP, и, если установлено, фильтрует их.
- `filter_input(int $type, string $var_name, int $filter = FILTER_DEFAULT, array|int $options = 0): mixed` - функция получает конкретную внешнюю переменную по имени и, если нужно, фильтрует значение переменной.
- `filter_list(): array` - функция возвращает список всех поддерживаемых фильтров.
- `filter_var_array(array $array, array|int $options = FILTER_DEFAULT, bool $add_empty = true): array|false|null` - функция принимает несколько переменных и при необходимости фильтрует их.
- `filter_var(mixed $value, int $filter = FILTER_DEFAULT, array|int $options = 0): mixed` - функция фильтрует указанную переменную.
***
## Filesystem functions
- `basename(string $path, string $suffix = ""): string` - 

***
## Image functions
- `gd_info(): array` - функция выводит информацию об установленной библиотеке GD.
- 