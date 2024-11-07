# Blade
***
## Template directives
- `@csrf` - вставляет в форму скрытое поле с электронным маркером безопасности.
- `@yield(string file_path)` - директива для подключения секции дочернего шаблона.
- `@extends(string file_path)` - 
- `@section(string file_path)` - 
- `@if(condition)` - условная конструкция для использования в шаблоне.
- `@foreach(array as variable)` - аналог цикла в шаблонизаторе Blade.
- `@method(method_name)` - функция подменяет запросы HTML формы _(PUT, PATCH, DELETE)_. Добавляет скрытое поле `_method`.
- `@error` - добавляет в валидацию обработку ошибок.
- `@guest` - выводит при условии неавторизированного пользователя.
- `@auth` - обратна операции выше.