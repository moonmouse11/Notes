# Contribution Guide
***
## Coding Style
Laravel следует стандарту кодирования `PSR-2` и стандарту автозагрузки `PSR-4`.
***
## PHPDoc
Пример валидного блока документации Laravel. За атрибутом `@param` идут два пробела, тип аргумента, еще два пробела и, наконец, имя переменной.
```php
/**
 * Register a binding with the container.
 *
 * @param  string|array  $abstract
 * @param  \Closure|string|null  $concrete
 * @param  bool  $shared
 * @return void
 *
 * @throws \Exception
 */
public function bind($abstract, $concrete = null, $shared = false)
{
    // ...
}
```
Когда атрибуты `@param` или `@return` являются избыточными из-за использования нативных типов, их можно удалить.
```php
/**
 * Execute the job.
 */
public function handle(AudioProcessor $processor): void
{
    //
}
```
Когда нативный тип является обобщенным, укажите его через использование атрибутов `@param` или `@return`
```php
/**
 * Get the attachments for the message.
 *
 * @return array<int, \Illuminate\Mail\Mailables\Attachment>
 */
public function attachments(): array
{
    return [
        Attachment::fromStorage('/path/to/file'),
    ];
}
```

***
