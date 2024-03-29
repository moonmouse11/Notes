# Functions
***
## Array
- `array_map(?callable $callback, array $array, array ...$arrays)` - применяет callback ко всем элементам переданного массива.
- `array_filter(array $array, ?callable $callback = null, int $mode = 0): array` - фильтрует элементы массива с помощью callback.
- `parse_url(string $url, int $component = -1)` - парсит адресную строку, для использования лучше подсмотреть компоненты.
- `sprintf(string $format, mixed ...$values): string` - форматирует и возвращает строку.
- `exit(string $status = ?): void` - выводит сообщение и останавливает работу скрипта.
- `die(string $message = ?): void` - как и функция выше, выводит сообщение и останавливает работу скрипта.
- `implode(string $separator, array $array): string` - функция объединяет элементы массива в строку.
- `explode(string $separator, string $string, int $limit = PHP_INT_MAX): array` - функция разбивает строку на массив.
- `array_keys(array $array, mixed $filter_value, bool $strict = false): array` - функция возвращает массив ключей переданного в аргументы массива.
- `header(string $header, bool $replace = true, int $response_code = 0): void` - функция отправляет необработанный заголовок страницы.
- `method_exists(object|string $object_or_class, string $method): bool` - функция проверяет наличие указанного метода у объекта.
- `compact(array|string $var_name, array|string ...$var_names): array`  - функция создает массив, содержащий переменные и их значения. _**(Не могут быть переданы суперглобальные массивы)**_.
- `extract(array &$array, int $flags = EXTR_OVERWRITE, string $prefix = ""): int` - функция обратна функции выше, извлекает переменные из массива.
- `str_contains(string $haystack, string $needle): bool` - функция  определяет, содержит ли строка заданную подстроку.
- `strlen(string $string): int` - функция вовозращает длинну строки.
- `htmlspecialchars(string $string, int $flags = ENT_QUOTES | ENT_SUBSTITUTE | ENT_HTML401, ?string $encoding = null, bool $double_encode = true): string`  - функция преобразовывает специальные символы в HTML.
- `file_get_contents(string $filename, bool $use_include_path = false, ?resource $context = null, int $offset = 0, ?int $length = null): string|false` - функция читает заданный файл и возвращает содержимое в виде строки.
- `preg_split(string $pattern, string $subject, int $limit = -1, int $flags = 0): array|false` - функция разбивает строку по указанному регулярному выражению, возвращает массив.
- `str_ends_with(string $haystack, string $needle): bool` - функция проверяет конец строки с заданной подстрокой.
- `get_class_methods(object|string $object_or_class): array` - функция возвращает массив методов переданного класса или объекта.
- `strpos(string $haystack, string $needle, int $offset = 0): int|false` - функция возвращает позицию первого вхождения подстроки в строку.
- `in_array(mixed $needle, array $haystack, bool $strict = false): bool` - функция проверяет наличие заданных данных в массиве.
- `array_pop(array &$array): mixed` - извлекает последний элемент массива.
- `array_pad(array $array, int $length, mixed $value): array` - дополняет массив значениями до заданной длины.
- `count(Countable|array $value, int $mode = COUNT_NORMAL): int` - функция возвращает количество элементов в массиве.
- `array_chunk(array $array, int $length, bool $preserve_keys = false): array` - функция разбивает массив на части.
- `fastcgi_finish_request(): bool` - функция сбрасывает все запрошенные данные клиенту и завершает обработку запроса.
- `http_response_code(int $response_code = 0): int|bool` - функция получает или задает коды ответов HTTP.
- `filter_var(mixed $value, int $filter = FILTER_DEFAULT, array|int $options = 0): mixed` - функция фильтрует переменную с помощью определённого фильтра.
- `spl_autoload_register(?callable $callback = null, bool $throw = true, bool $prepend = false): bool` - регистрирует функцию в autoload. 
- `http_build_query(array|object $data, string $numeric_prefix = "", ?string $arg_separator = null, int $encoding_type = PHP_QUERY_RFC1738): string` - функция генерирует URL-кодированную строку запроса из предоставленного ассоциативного (или индексированного) массива.
- `mb_strtoupper(string $string, ?string $encoding = null): string` - функция приводит строку в верхнему регистру. _**(Для кодировки больше одного байта)**_
- `mb_strtolower(string $string, ?string $encoding = null): string` - функция приводит строку к нижнему реистру. _**(Для кодировки больше одного байта)**_
- `array_key_exists(string|int|float|bool|resource|null $key, array $array): bool` - функция проверяет, содежит ли массив зпдпнный ключ или индекс.
- `call_user_func(callable $callback, mixed ...$args): mixed` - функция вызывает callback-функцию, переданную первым параметром, и передаёт остальные параметры в качестве аргументов. 
- `call_user_func_array(callable $callback, array $args): mixed` - функция вызывает callback-функцию `callback`, с параметрами из массива `args`.
- `spl_autoload_call(string $class): void` - функцию запускают для ручного поиска класса или интерфейса через зарегистрированные методы очереди.
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
## Password
- `password_hash(string $password, string|int|null $algo, array $options = []): string` - функция создает хэш-пароля, используя необратимый алгоритм хеширования.
- `password_algos(): array` - функция возвращает полный список зарегистрированных идентификаторов алгоритмов шифрований.
- `password_get_info(string $hash): array` - возвращает информацию о заданном хэше.
- `password_needs_rehash(string $hash, string|int|null $algo, array $options = []): bool` - валидация переданного хэша по переданному в аргументы алгоритму.
- `password_verify(string $password, string $hash): bool` - функция проверяет соотвествие пароля хэшу.
- `crypt(string $string, string $salt): string` - функция возвращает хешированную строку, полученную с помощью стандартного алгоритма UNIX, основанного на DES или другого алгоритма.
## Session
- 