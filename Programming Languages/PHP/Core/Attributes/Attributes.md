# Attributes
***
## Basics
**Атрибуты** — это структурированные машиночитаемые метаданные, объявленные в коде. Целью атрибутов могу быть: классы (включая анонимные), методы, функции, параметры, свойства и константы класса. Затем описанные атрибутами метаданные можно проанализировать во время исполнения средствами `Reflection API`. Поэтому атрибуты можно рассматривать как встроенный в код язык конфигурации.
**Атрибуты** разделяют общее и специфическое поведение сущностей в приложении. В каком-то смысле это похоже на интерфейс с его реализациями. Но интерфейсы и реализации — это про код, а атрибуты — про добавление дополнительной информации и конфигурацию. Интерфейсы могут реализовываться только классами, тогда как атрибуты можно нацеливать на методы, функции, параметры, свойства и константы классов. Поэтому атрибуты — существенно более гибкий механизм, чем интерфейсы.
***
## Syntax
Синтаксис атрибутов состоит из нескольких частей. Во-первых, декларация атрибута начинается с символов `#[` и заканчивается символом `]`. Во-вторых, внутри конструкции перечисляют один или больше атрибутов, которые разделяют запятой. Названия атрибутов по аналогии с названиями классов указывают неполными, полными и абсолютными. Аргументы атрибутов необязательны, но если аргументы указали, их заключают в круглые скобки `()`. Атрибуты принимают аргументы либо в виде конкретных литеральных значений, либо константных выражений. Аргументы разрешается записывать как позиционным, так и именованным синтаксисом.
Когда API-интерфейс модуля Reflection запрашивает экземпляр класса атрибута, название атрибута трактуется как название запрашиваемого класса, а аргументы атрибута передаются в конструктор этого класса. **Поэтому для каждого атрибута необходимо создавать класс.**
### Example
``` php
// a.php  
namespace MyExample;  
  
use Attribute;  
  
#[Attribute]  
class MyAttribute  
{  
const VALUE = 'value';  
  
private $value;  
  
public function __construct($value = null)  
{  
$this->value = $value;  
}  
}  
  
// b.php  
  
namespace Another;  
  
use MyExample\MyAttribute;  
  
#[MyAttribute]  
#[\MyExample\MyAttribute]  
#[MyAttribute(1234)]  
#[MyAttribute(value: 1234)]  
#[MyAttribute(MyAttribute::VALUE)]  
#[MyAttribute(array("key" => "value"))]  
#[MyAttribute(100 + 200)]  
class Thing {}  
  
#[MyAttribute(1234), MyAttribute(5678)]  
class AnotherThing {}
```
***
## Reflection API
Для доступа к атрибутам классов, методов, функций, параметров, свойств и констант класса в API-интерфейсе модуля Reflection предусмотрели метод `getAttributes()`. Метод возвращает массив объектов `ReflectionAttribute`, каждый из которых умеет возвращать название и аргументы атрибута, и создавать объект класса, которым представили атрибут.
Разделение объекта, который представляет отражение атрибута, и объекта самого класса, которым представили атрибут, повышает контроль над обработкой ошибок, которые возникают, когда для атрибута не определили класс, допустили опечатку или пропустили аргумент. Объект класса атрибута создаётся и проверяется на корректность аргументов только после вызова метода `ReflectionAttribute::newInstance()`, не раньше.
### Example
```php
#[Attribute]  
class MyAttribute  
{  
	public $value;  
  
	public function __construct($value)  
	{  
		$this->value = $value;  
	}  
}  
  
#[MyAttribute(value: 1234)]  
class Thing {}  
  
function dumpAttributeData($reflection)  
{  
	$attributes = $reflection->getAttributes();  
  
	foreach ($attributes as $attribute) {  
		var_dump($attribute->getName());  
		var_dump($attribute->getArguments());  
		var_dump($attribute->newInstance());  
	}  
}  
  
dumpAttributeData(new ReflectionClass(Thing::class));  
/*  
string(11) "MyAttribute"  
array(1) {  
["value"]=>  
int(1234)  
}  
object(MyAttribute)#3 (1) {  
["value"]=>  
int(1234)  
}
```
***
## Work with Attributes
Хотя нет строго требования, лучше выполнять рекомендацию — создавать отдельный класс для каждого атрибута. В самом простом случае создают пустой класс, для которого объявляют атрибут `#[Attribute]`, который доступен для импорта из глобального пространства имён через оператор `use`.
```php
/* Пример простого класса с атрибутом */

namespace Example;  
  
use Attribute;  
  
#[Attribute]  
class MyAttribute {}
```
Объекты, для которых разрешается назначение атрибута, ограничивают путём передачи битовой маски в первом аргументе объявления атрибута `#[Attribute]`.
```php
/* Спецификация ограничения целей, доступных для присваивания атрибутов */

namespace Example;  
  
use Attribute;  
  
#[Attribute(Attribute::TARGET_METHOD | Attribute::TARGET_FUNCTION)]  
class MyAttribute {}
```
Теперь назначение атрибута `MyAttribute` другому типу, которой отличается от метода или функции, при вызове метода `ReflectionAttribute::newInstance()` выбросит исключение.
По умолчанию атрибут разрешается назначить классу, свойству или другому объекту рефлексии только один раз. Назначить одинаковые атрибуты одному объекту рефлексии получится, только если объявить атрибут `#[Attribute]` с флагом **`[Attribute::IS_REPEATABLE]`** в битовой маске.
```php
/* Пример с константой IS_REPEATABL, которая разрешит назначать атрибут многократно */

namespace Example;  
  
use Attribute;  
  
#[Attribute(Attribute::TARGET_METHOD | Attribute::TARGET_FUNCTION | Attribute::IS_REPEATABLE)]  
class MyAttribute  
{  
}
```
***
## Attributes Targets
```php
Attribute::TARGET_CLASS
Attribute::TARGET_FUNCTION
Attribute::TARGET_METHOD
Attribute::TARGET_PROPERTY
Attribute::TARGET_CLASS_CONSTANT
Attribute::TARGET_PARAMETER
Attribute::TARGET_ALL
```
***
## Example
```php
#[ExampleAttribute('Hello world', 42)]
class Foo {}

#[Attribute]
class ExampleAttribute {
    private string $message;
    private int $answer;
    public function __construct(string $message, int $answer) {
        $this->message = $message;
        $this->answer = $answer;
    }
}

$reflector = new \ReflectionClass(Foo::class);
$attrs = $reflector->getAttributes();

foreach ($attrs as $attriubute) {

    $attriubute->getName(); // "My\Attributes\ExampleAttribute"
    $attriubute->getArguments(); // ["Hello world", 42]
    $attriubute->newInstance();
        // object(ExampleAttribute)#1 (2) {
        //  ["message":"Foo":private]=> string(11) "Hello World"        
        //  ["answer":"Foo":private]=> int(42) 
        // }
}
```
***
