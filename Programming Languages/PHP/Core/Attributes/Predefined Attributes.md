# Predefined Attributes
***
## Attribute
Атрибуты помогают добавлять к объявлениям в коде структурированную машиночитаемую метаинформацию. Атрибуты нацеливают на классы, включая анонимные, методы, функции, параметры, свойства и константы класса. Затем во время выполнения кода метаданные, которые определили атрибутами, инспектируют через API-интерфейс модуля `Reflection`. Поэтому атрибуты рассматривают как язык конфигурации, который встраивается в код.
```php
/* Обзор класса */

final class Attribute {
	/* Константы */
	const int TARGET_CLASS;
	const int TARGET_FUNCTION;
	const int TARGET_METHOD;
	const int TARGET_PROPERTY;
	const int TARGET_CLASS_CONSTANT;
	const int TARGET_PARAMETER;
	const int TARGET_ALL;
	const int IS_REPEATABLE;
	/* Свойства */
	public int $flags;
	/* Методы */
	public __construct(int $flags = Attribute::TARGET_ALL)
}
```
***
## Predefined Constants
- `Attribute::TARGET_CLASS` - ограничивает установку атрибута только на класс.
- `Attribute::TARGET_FUNCTION` - ограничивает установку атрибута только на функцию.
- `Attribute::TARGET_METHOD` - ограничивает установку атрибута только на метод класса.
- `Attribute::TARGET_PROPERTY` - ограничивает установку атрибута только на свойство класса.
- `Attribute::TARGET_CLASS_CONSTANT` - ограничивает установку атрибута только на константу класса.
- `Attribute::TARGET_PARAMETER` - ограничивает установку атрибута только на параметр функции.
- `Attribute::TARGET_ALL` - разрешает устанавливать аттрибут ко всему.
- `Attribute::IS_REPEATABLE` - позволяет переиспользовать аттрибут в классе. (По умолчанию можно использовать только один раз).
***
## `AllowDynamicProperties`
Атрибут маркирует классы, в которых разрешается объявлять **динамические свойства**
```php
/* Обзор класса */

final class AllowDynamicProperties {
	/* Методы */
	public __construct()
}
```
Динамические свойства устарели с `PHP 8.2.0`, поэтому объявление динамического свойства без маркировки класса этим атрибутом выдаст уведомление об устаревании.
### Example
```php
class DefaultBehaviour {}

#[\AllowDynamicProperties]
class ClassAllowsDynamicProperties {}

$o1 = new DefaultBehaviour();
$o2 = new ClassAllowsDynamicProperties();

$o1->nonExistingProp = true;
$o2->nonExistingProp = true;
```
***
## `Deprecated`
Атрибут помечает функциональность устаревшей. Устаревшая функциональность вызывает ошибки уровня `E_USER_DEPRECATED`.
```php
/* Обзор класса */

final class Deprecated {
	/* Свойства */
	public readonly ?string $message;
	public readonly ?string $since;
	/* Методы */
	public __construct(?string $message = null, ?string $since = null)
}
```
- `message` - Необязательное сообщение, которое объясняет причину устаревания и возможную замену функциональности. Текст сообщения включается в предупреждение об устаревании.
- `since` - Необязательная строка, которая указывает, с какого момента устарела функциональность. PHP не проверяет содержание строки и поэтому иногда строка включает сведения о версии, дате или другие значения, которые считает уместными. Строка включается в предупреждение об устаревании. Функциональность самого PHP указывает в значении свойства `since` момент устаревания в виде мажорной и минорной версий, например `'8.4'`.
### Example
```php
#[\Deprecated(message: "use safe_replacement() instead", since: "1.5")]  
function unsafe_function()  
{  
	echo "This is unsafe", PHP_EOL;  
}
```
***
## `Override`
Атрибут указывает, что метод переопределяет метод родительского класса или реализует метод, который определили в интерфейсе.
PHP выдаст ошибку времени компиляции, если в родительском классе или интерфейсе, который реализовали, не содержится метода с таким же названием.
```php
/* Обзор класса */

final class Override {
	/* Методы */
	public __construct()
}
```
### Example
```php
class Base  
{  
	protected function foo(): void {}  
}  
  
final class Extended extends Base  
{  
	#[\Override]  
	protected function boo(): void {}  
}
```
***
## `ReturnTypeWillChange`
Большинство неокончательных внутренних методов теперь требуют, чтобы методы, которые их переопределяют, объявляли совместимый тип возвращаемого значения, иначе при проверке наследования выдаётся уведомление об устаревании. Если тип возвращаемого значения невозможно объявить для переопределяемого метода из-за проблем совместимости с кросс-версиями PHP, добавляют атрибут `#[\ReturnTypeWillChange]`, чтобы заглушить уведомление об устаревании.
```php
/* Обзор класса */

final class ReturnTypeWillChange {
	/* Методы */
	public __construct()
}
```
***
## `SensitiveParameter`
Атрибутом размечают параметр с чувствительным значением, которое PHP отредактирует, когда значение пападёт в трассировку стека.
```php
/* Обзор класса */

final class SensitiveParameter {
	/* Методы */
	public __construct()
}
```
### Example
```php
function defaultBehavior(
    string $secret,
    string $normal
) {
    throw new Exception('Error!');
}

function sensitiveParametersWithAttribute(
    #[\SensitiveParameter]
    string $secret,
    string $normal
) {
    throw new Exception('Error!');
}

try {
    defaultBehavior('password', 'normal');
} catch (Exception $e) {
    echo $e, PHP_EOL, PHP_EOL;
}

try {
    sensitiveParametersWithAttribute('password', 'normal');
} catch (Exception $e) {
    echo $e, PHP_EOL, PHP_EOL;
}

/* Результат */
Exception: Error! in example.php:7
Stack trace:
#0 example.php(19): defaultBehavior('password', 'normal')
#1 {main}

Exception: Error! in example.php:15
Stack trace:
#0 example.php(25): sensitiveParametersWithAttribute(Object(SensitiveParameterValue), 'normal')
#1 {main}
```
***
