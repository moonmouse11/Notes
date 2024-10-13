# PSR-2
***
## Руководство написания кода
Это руководство наследует и расширяет `PSR-1`, основной стандарт написания кода.
Целью данного руководства является уменьшение недовольства во время чтения (сканирования) кода другого автора. Этого удается достичь путем следования перечисленному общему набору правил и рекомендаций по оформлению PHP кода.
Правила стилизации, приведенные здесь, выводятся из сходства между различными членами проектов. Когда разные авторы сотрудничают между несколькими проектами, это помогает иметь один набор руководящих принципов для использования сразу всеми этими проектами. Таким образом, преимущество этого руководства не в самих правилах, а в обмене этими правилами.
Ключевые слова `«ДОЛЖЕН», «НЕ ДОЛЖЕН», «НЕОБХОДИМ», «ЖЕЛАТЕЛЕН», «НЕ ЖЕЛАТЕЛЕН», «ДОЛЖЕН», «НЕ ДОЛЖЕН», «РЕКОМАНДОВАН», «ВЕРОЯТЕН», «ОПЦИОНАЛЕН»` в этом документе должны быть интерпретированы в соответствии с [RFC 2119](http://www.ietf.org/rfc/rfc2119.txt).
***
## Обзор
1. Код `ДОЛЖЕН` следовать `PSR-1`.
2. При написании кода `ДОЛЖНЫ` использоваться для отступа 4 пробела, а не табы.
3. `НЕ ОБЯЗАТЕЛЬНЫ` жесткие ограничения на длину строки; мягкое ограничение `ОБЯЗАТЕЛЬНО` должно быть равно 120 символам; строки `ДОЛЖНЫ` содержать 80 или меньше символов.
4. После объявления `пространства_имен` `ДОЛЖНА` быть одна пустая строка, также после объявления блока `use` `ДОЛЖНА` быть одна пустая строка.
5. Открывающая фигурная скобка при описании классов `ОБЯЗАТЕЛЬНО` должна находиться на следующей строке, также закрывающая фигурная скобка `ДОЛЖНА` находиться после тела класса.
6. Открывающая фигурная скобка при описании методов `ОБЯЗАТЕЛЬНО` должна находиться на следующей строке, также закрывающая фигурная скобка `ДОЛЖНА` находиться после тела метода.
7. Видимость `ДОЛЖНА` быть объявлена на всех свойствах и методах; `abstract` и `final` `ОБЯЗАТЕЛЬНО` должно быть объявлено перед видимостью; `static` `ОБЯЗАТЕЛЬНО` должно быть объявлено после видимости.
8. Ключевые слова управляющих структур `ОБЯЗАТЕЛЬНО` должны иметь один пробел после себя; методы и вызовы функций `НЕ ДОЛЖНЫ`.
9. Открывающиеся фигурные скобки для управляющих структур `ОБЯЗАТЕЛЬНО` должны находиться на той же самой линии, а закрывающиеся фигурные скобки `ОБЯЗАТЕЛЬНО` должно находиться на следующей за телом линии.
10. Открывающая круглая скобка управляющих структур `ОБЯЗАТЕЛЬНО НЕ ДОЛЖНА` иметь пробела после неё, а закрывающая круглая скобка управляющих структур `ОБЯЗАТЕЛЬНО НЕ ДОЛЖНА` иметь пробела до нее.
***
## Пример
Следующий код представляет собой краткий пример верного следования правилам:
```php
<?php
namespace Vendor\Package;

use FooInterface;
use BarClass as Bar;
use OtherVendor\OtherPackage\BazClass;

class Foo extends Bar implements FooInterface
{
    public function sampleFunction($a, $b = null)
    {
        if ($a === $b) {
            bar();
        } elseif ($a > $b) {
            $foo->bar($arg1);
        } else {
            BazClass::bar($arg2, $arg3);
        }
    }

    final public static function bar()
    {
        // тело метода
    }
}
```
***
## Общее
1. Основной стандарт написания кода - Код `ДОЛЖЕН` следовать всем правилам указанным в `PSR-1`.
2. Файлы - Все PHP файлы `ДОЛЖНЫ` использовать Unix LF (перевод строки) для указания завершения строки.
Все PHP файлы `ДОЛЖНЫ` заканчиваться одной пустой строкой.
Закрывающий тег `?>` `ДОЛЖЕН` быть удален из файлов, содержащих только PHP.
3. Строки - `НЕ ДОЛЖНО` быть жесткого ограничения на длину строки.
Мягкое ограничение на длину строки `ДОЛЖНО` быть равно 120 символам; автоматические проверки стиля `ДОЛЖНЫ` предупреждать, но `НЕ ДОЛЖНЫ` выдавать ошибку при пресечении мягкого ограничения.
Строкам `НЕ СЛЕДУЕТ` быть длинее 80 символов; строки длиннее `СЛЕДУЕТ` разделить на несколько последующих строк, не более 80 символов каждая.
В конце непустых строк `НЕ ДОЛЖНО` быть пустого пространства.
Пустые строки `МОГУТ` быть добавлены для улучшения читабельности кода и для определения взаимосвязанных блоков кода.
В каждой строке `ДОЛЖНО` быть не более одного оператора.
4. Отступы- Код `ДОЛЖЕН` использовать 4 пробела, и `НЕ ДОЛЖЕН` использовать табы для отступов.
	N.b.: Использование только пробелов, и не смешивание пробелов с табами, помогает избежать проблем с различиями, патчами, историй и аннотациями. Использование пробелов также позволяет легко вставить мелкозернистый суботступ для межстрочного выравнивания.
5. Ключевые слова и `True/False/Null` - [Ключевые слова](http://php.net/manual/ru/reserved.keywords.php) PHP `ДОЛЖНЫ` быть записаны в нижнем регистре.
Константы PHP `true`, `false`, и `null` `ДОЛЖНЫ` быть в нижнем регистре.
***
## Объявление пространств имен и `use`
Если присутствует, после объявления пространства имен `ДОЛЖНА` быть одна пустая строка.
Если присутствует, все объявления `use` `ДОЛЖНЫ` идти после объявления пространства имен.
`ДОЛЖНО` быть только одно ключевое слово `use` на объявление.
`ДОЛЖНА` быть одна пустая строка после блока `use`.
Например:
```php
<?php
namespace Vendor\Package;

use FooClass;
use BarClass as Bar;
use OtherVendor\OtherPackage\BazClass;

// ... Дополнительный PHP код ...
```
***
## Классы, Свойства и Методы.
Термин «класс» относится ко всем классам, интерфейсам и трэйтам.
1. Наследование и Имплементация - Ключевые слова `extends` и `implements` `ДОЛЖНЫ` быть объявлены на той же строке, что и класс.
Открывающая фигурная скобка для класса `ДОЛЖНА` быть на отдельной строке; закрывающая фигурная скобка класса `ДОЛЖНА` быть на следующей строке, после тела класса.
```php
<?php
namespace Vendor\Package;

use FooClass;
use BarClass as Bar;
use OtherVendor\OtherPackage\BazClass;

class ClassName extends ParentClass implements \ArrayAccess, \Countable
{
    // константы, свойства, методы
}
```
Список `implements` `МОЖЕТ` быть разделен на несколько строк, где каждая последующая строка с одним отступом. При этом первый элемент списка `ДОЛЖЕН` быть на следующей линии, и в этом списке `ДОЛЖЕН` быть только один интерфейс на линии.
```php
<?php
namespace Vendor\Package;

use FooClass;
use BarClass as Bar;
use OtherVendor\OtherPackage\BazClass;

class ClassName extends ParentClass implements
    \ArrayAccess,
    \Countable,
    \Serializable
{
    // константы, свойства, методы
}
```
2. Свойства - Видимость `ДОЛЖНА` быть объявлена на всех свойствах.
Ключевое слово `var` `НЕ ДОЛЖНО` использоваться для объявления свойств.
`НЕ ДОЛЖНО` быть больше одного свойства объявленного на выражение.
Названиям свойств `НЕ СЛЕДУЕТ` быть с префиксом с одиночным символом подчеркиваия для обозначения защищенной или приватной видимости.
Объявление свойств должно выглядеть, как в примере:
```php
<?php
namespace Vendor\Package;

class ClassName
{
    public $foo = null;
}
```
3. Методы - Видимость `ДОЛЖНА` быть объявлена на всех методах.
Названиям методов `НЕ СЛЕДУЕТ` быть с префиксом символом подчеркивания для обозначения защищенной или приватной видимости.
Названия методов `НЕ ДОЛЖНЫ` объявляться с пробелами после названия. Открывающая фигурная скобка `ДОЛЖНА` быть на своей собственной линии, и закрывающая фигурная скобка `ДОЛЖНА` быть на следующей линии после тела метода. После открывающей круглой скобки `НЕ ДОЛЖНО` быть пробелов, также `НЕ ДОЛЖНО` быть пробелов перед закрывающей круглой скобкой.
Объявление методов должно выглядеть, как в примере. Обратите внимание на расположение скобок, запятых и пробелов:
```php
<?php
namespace Vendor\Package;

class ClassName
{
    public function fooBarBaz($arg1, &$arg2, $arg3 = [])
    {
        // тело метода
    }
}
```
4. Аргументы методов - В списке аргументов, `НЕ ДОЛЖНО` быть пробела перед каждой запятой, но `ДОЛЖЕН` быть один пробел после каждой запятой.
Аргументы методов со значениями по умолчанию `ДОЛЖНЫ` находиться в конце списка аргументов.
```php
<?php
namespace Vendor\Package;

class ClassName
{
    public function foo($arg1, &$arg2, $arg3 = [])
    {
        // тело метода
    }
}
```
Список аргументов `МОЖЕТ` быть разделен на несколько строк, где каждая следующая линия выделяется одним отступом. При этом первый элемент в списке `ДОЛЖЕН` находиться на следующей строке, а также `ДОЛЖЕН` быть только один аргумент в строке.
Когда список аргументов разделен на несколько строк, закрывающая круглая скобка и открывающая фигурная `ДОЛЖНЫ` находиться вместе на отдельной строке с одним пробелом между ними.
```php
<?php
namespace Vendor\Package;

class ClassName
{
    public function aVeryLongMethodName(
        ClassTypeHint $arg1,
        &$arg2,
        array $arg3 = []
    ) {
        // тело метода
    }
}
```
5. `abstract`, `final`, и `static` - Если присутствует объявление `abstract` или `final`, они `ДОЛЖНЫ` предшествовать объявлению видимости.
Если присутствует объявление `static`, оно `ДОЛЖНО` идти после объявления видимости.
```php
<?php
namespace Vendor\Package;

abstract class ClassName
{
    protected static $foo;

    abstract protected function zim();

    final public static function bar()
    {
        // тело метода
    }
}
```
6. Вызов Метода или Функции - Когда происходит вызов метода или функции, между названием функции или метода и открывающей круглой скобкой `НЕ ДОЛЖНО` быть пробела, также `НЕ ДОЛЖНО` быть пробела после открывающей круглой скобки, а также `НЕ ДОЛЖНО` быть пробела до закрывающей круглой скобки. В списке аргументов `НЕ ДОЛЖНО` быть пробела перед каждой запятой и `ДОЛЖЕН` быть один пробел после каждой запятой.
```php
<?php
bar();
$foo->bar($arg1);
Foo::bar($arg2, $arg3);
```
Список аргументов `МОЖЕТ` быть разделен на несколько строк, каждая последующая строка должна отделяться одним отступом. При этом первый элемент списка `ДОЛЖЕН` быть на следующей строке, а также `ДОЛЖЕН` быть только один аргумент в строке.
```php
<?php
$foo->bar(
    $longArgument,
    $longerArgument,
    $muchLongerArgument
);
```
***
## Управляющие Структуры
Главные правила стиля для управляющих структур следующие:
- После ключевого слова управляющих структур `ДОЛЖЕН` быть один пробел.
- После открывающей круглой скобки `НЕ ДОЛЖНО` быть пробелов.
- Перед закрывающей круглой скобкой `НЕ ДОЛЖНО` быть пробелов.
- Между закрывающей круглой скобкой и открывающей фигурной скобкой `ДОЛЖЕН` быть один пробел.
- Тело структуры `ДОЛЖНО` быть отделено один раз.
- Закрывающая фигурная скобка `ДОЛЖНА` быть на следующей линии после тела.
Тело каждой структуры `ДОЛЖНО` быть заключено в фигурные скобки. Это стандартизирует вид структур, а также уменьшает вероятность получения ошибок при добавление новых строк к телу.
1. `if`, `elseif`, `else` - Управляющая структура `if` `ДОЛЖНА` выглядеть, так, как показано. Обратите внимание на размещение круглых скобок, пробелов и фигурных скобок; также обратите внимание, что `else` и `elseif` находятся на той же линии, что закрывающая фигурная скобка предыдущего тела.
```php
<?php
if ($expr1) {
    // тело if
} elseif ($expr2) {
    // тело elseif
} else {
    // тело else;
}
```
Ключевое слово `elseif` `СЛЕДУЕТ` использовать вместо `else if`, так чтобы все ключевые слова выглядели как единое слово.
2. `switch`, `case` - Управляющая структура `switch` должна выглядеть так, как показано. Обратите внимание на расположение круглых скобок, пробелов и фигурных скобок. Выражение `case` `ДОЛЖНО` быть отделено один раз от `switch`, также ключевое слово `break`(или другое завершающие ключевое слово) `ДОЛЖНО` быть на том же уровне, что и тело `case`. `ДОЛЖЕН` присутствовать комментарий такой, как `// no break`, когда встречается непустое тело `case` без завершающего ключевого слова.
```php
<?php
switch ($expr) {
    case 0:
        echo 'First case, with a break';
        break;
    case 1:
        echo 'Second case, which falls through';
        // no break
    case 2:
    case 3:
    case 4:
        echo 'Third case, return instead of break';
        return;
    default:
        echo 'Default case';
        break;
}
```
3. `while`, `do while` - Выражение `while` `ДОЛЖНО` выглядеть, как показано ниже. Обратите внимание на расположение скобок и пробелов.
```php
<?php
while ($expr) {
    // тело структуры
}
```
Кроме того, выражение `do while` `ДОЛЖНО` выглядеть, как показано ниже. Обратите внимание на расположение скобок и пробелов.
```php
<?php
do {
    // тело структуры;
} while ($expr);
```
4. `for` - Выражение `for` `ДОЛЖНО` выглядеть, как показано ниже. Обратите внимание на расположение скобок и пробелов.
```php
<?php
for ($i = 0; $i < 10; $i++) {
    // тело for.
}
```
5. `foreach` - Выражение `foreach` `ДОЛЖНО` выглядеть, как показано ниже. Обратите внимание на расположение скобок и пробелов.
```php
<?php
foreach ($iterable as $key => $value) {
    // тело foreach
}
```
6. `try`, `catch` - Блок `try catch` должен выглядеть, как показано ниже. Обратите внимание на расположение скобок и пробелов.
```php
<?php
try {
    // тело try
} catch (FirstExceptionType $e) {
    // тело catch
} catch (OtherExceptionType $e) {
    // тело catch
}
```
***
## Замыкания
Замыкания `ДОЛЖНЫ` быть объявлены с одним пробелом после ключевого слова `function`, а также с пробелом до и после ключевого слова `use`.
Открывающая фигурная скобка `ДОЛЖНА` быть на той же строке, а закрывающая фигурная скобка `ДОЛЖНА` быть на следующей за телом строке.
После открывающей круглой скобки списка аргументов или списка переменных `НЕ ДОЛЖНО` быть пробела, также `НЕ ДОЛЖНО` быть пробела до закрывающей круглой скобки списка аргументов или списка переменных.
В списке аргументов и списке переменных `НЕ ДОЛЖНО` быть пробелов перед каждой запятой, но `ДОЛЖЕН` быть пробел после каждой запятой.
Аргументы замыкания со значениями по умолчанию должны находиться в конце списка аргументов.
Объявление замыкания должно выглядеть, как показано ниже. Обратите внимание на расположение скобок, запятых и пробелов.
```php
<?php
$closureWithArgs = function ($arg1, $arg2) {
    // тело
};

$closureWithArgsAndVars = function ($arg1, $arg2) use ($var1, $var2) {
    // тело
};
```
Списки аргументов и переменных `МОГУТ` быть разделены на несколько строк, где каждая последующая строка отделена один раз. При этом первый элемент в списке `ДОЛЖЕН` быть на следующей строке и на каждой строке `ДОЛЖЕН` быть только один аргумент или переменная.
Когда конец списка(в независимости от того аргументы это, или переменные) разделен на несколько строк, закрывающая круглая скоба и открывающая фигурная `ДОЛЖНЫ` быть расположены вместе на отдельной линии с одним пробелом между ними.
Ниже приведены примеры замыканий со списком аргументов и без него, а также с разделением списка переменных на несколько строк.
```php
<?php
$longArgs_noVars = function (
    $longArgument,
    $longerArgument,
    $muchLongerArgument
) {
   // тело
};

$noArgs_longVars = function () use (
    $longVar1,
    $longerVar2,
    $muchLongerVar3
) {
   // тело
};

$longArgs_longVars = function (
    $longArgument,
    $longerArgument,
    $muchLongerArgument
) use (
    $longVar1,
    $longerVar2,
    $muchLongerVar3
) {
   // тело
};

$longArgs_shortVars = function (
    $longArgument,
    $longerArgument,
    $muchLongerArgument
) use ($var1) {
   // тело
};

$shortArgs_longVars = function ($arg) use (
    $longVar1,
    $longerVar2,
    $muchLongerVar3
) {
   // тело
};
```
Обратите внимание, что правила форматирования также применяются, когда замыкания используются непосредственно в вызове функции или метода в качестве аргумента.
```php
<?php
$foo->bar(
    $arg1,
    function ($arg2) use ($var1) {
        // тело
    },
    $arg3
);
```
***
## Вывод
Есть очень много элементов стиля и практики, которые намеренно опущены в этом руководстве. Они включают, но не ограничиваются:
- Объявлением глобальных переменных и констант.
- Объявлением функций.
- Операторами и присваиванием.
- Выравнивание
- Комментарии и документации блоков
- Преффиксы и суффиксы названий классов
- Лучшие практики
В будущем руководство `МОЖЕТ` быть пересмотрено или расширено для решения тех или иных элементов стиля и практики.
***
## Appendix A. Survey
In writing this style guide, the group took a survey of member projects to determine common practices. The survey is retained herein for posterity.
### A.1. Survey Data
```
url,http://www.horde.org/apps/horde/docs/CODING_STANDARDS,http://pear.php.net/manual/en/standards.php,http://solarphp.com/manual/appendix-standards.style,http://framework.zend.com/manual/en/coding-standard.html,http://symfony.com/doc/2.0/contributing/code/standards.html,http://www.ppi.io/docs/coding-standards.html,https://github.com/ezsystems/ezp-next/wiki/codingstandards,http://book.cakephp.org/2.0/en/contributing/cakephp-coding-conventions.html,https://github.com/UnionOfRAD/lithium/wiki/Spec%3A-Coding,http://drupal.org/coding-standards,http://code.google.com/p/sabredav/,http://area51.phpbb.com/docs/31x/coding-guidelines.html,https://docs.google.com/a/zikula.org/document/edit?authkey=CPCU0Us&hgd=1&id=1fcqb93Sn-hR9c0mkN6m_tyWnmEvoswKBtSc0tKkZmJA,http://www.chisimba.com,n/a,https://github.com/Respect/project-info/blob/master/coding-standards-sample.php,n/a,Object Calisthenics for PHP,http://doc.nette.org/en/coding-standard,http://flow3.typo3.org,https://github.com/propelorm/Propel2/wiki/Coding-Standards,http://developer.joomla.org/coding-standards.html
voting,yes,yes,yes,yes,yes,yes,yes,yes,yes,yes,yes,yes,yes,yes,yes,no,no,no,?,yes,no,yes
indent_type,4,4,4,4,4,tab,4,tab,tab,2,4,tab,4,4,4,4,4,4,tab,tab,4,tab
line_length_limit_soft,75,75,75,75,no,85,120,120,80,80,80,no,100,80,80,?,?,120,80,120,no,150
line_length_limit_hard,85,85,85,85,no,no,no,no,100,?,no,no,no,100,100,?,120,120,no,no,no,no
class_names,studly,studly,studly,studly,studly,studly,studly,studly,studly,studly,studly,lower_under,studly,lower,studly,studly,studly,studly,?,studly,studly,studly
class_brace_line,next,next,next,next,next,same,next,same,same,same,same,next,next,next,next,next,next,next,next,same,next,next
constant_names,upper,upper,upper,upper,upper,upper,upper,upper,upper,upper,upper,upper,upper,upper,upper,upper,upper,upper,upper,upper,upper,upper
true_false_null,lower,lower,lower,lower,lower,lower,lower,lower,lower,upper,lower,lower,lower,upper,lower,lower,lower,lower,lower,upper,lower,lower
method_names,camel,camel,camel,camel,camel,camel,camel,camel,camel,camel,camel,lower_under,camel,camel,camel,camel,camel,camel,camel,camel,camel,camel
method_brace_line,next,next,next,next,next,same,next,same,same,same,same,next,next,same,next,next,next,next,next,same,next,next
control_brace_line,same,same,same,same,same,same,next,same,same,same,same,next,same,same,next,same,same,same,same,same,same,next
control_space_after,yes,yes,yes,yes,yes,no,yes,yes,yes,yes,no,yes,yes,yes,yes,yes,yes,yes,yes,yes,yes,yes
always_use_control_braces,yes,yes,yes,yes,yes,yes,no,yes,yes,yes,no,yes,yes,yes,yes,no,yes,yes,yes,yes,yes,yes
else_elseif_line,same,same,same,same,same,same,next,same,same,next,same,next,same,next,next,same,same,same,same,same,same,next
case_break_indent_from_switch,0/1,0/1,0/1,1/2,1/2,1/2,1/2,1/1,1/1,1/2,1/2,1/1,1/2,1/2,1/2,1/2,1/2,1/2,0/1,1/1,1/2,1/2
function_space_after,no,no,no,no,no,no,no,no,no,no,no,no,no,no,no,no,no,no,no,no,no,no
closing_php_tag_required,no,no,no,no,no,no,no,no,yes,no,no,no,no,yes,no,no,no,no,no,yes,no,no
line_endings,LF,LF,LF,LF,LF,LF,LF,LF,?,LF,?,LF,LF,LF,LF,?,,LF,?,LF,LF,LF
static_or_visibility_first,static,?,static,either,either,either,visibility,visibility,visibility,either,static,either,?,visibility,?,?,either,either,visibility,visibility,static,?
control_space_parens,no,no,no,no,no,no,yes,no,no,no,no,no,no,yes,?,no,no,no,no,no,no,no
blank_line_after_php,no,no,no,no,yes,no,no,no,no,yes,yes,no,no,yes,?,yes,yes,no,yes,no,yes,no
class_method_control_brace,next/next/same,next/next/same,next/next/same,next/next/same,next/next/same,same/same/same,next/next/next,same/same/same,same/same/same,same/same/same,same/same/same,next/next/next,next/next/same,next/same/same,next/next/next,next/next/same,next/next/same,next/next/same,next/next/same,same/same/same,next/next/same,next/next/next
```
### A.2. Survey Legend
- `indent_type`: The type of indenting. `tab` = "Use a tab", `2` or `4` = "number of spaces"
- `line_length_limit_soft`: The "soft" line length limit, in characters. `?` = not discernible or no response, `no` means no limit.
- `line_length_limit_hard`: The "hard" line length limit, in characters. `?` = not discernible or no response, `no` means no limit.
- `class_names`: How classes are named. `lower` = lowercase only, `lower_under` = lowercase with underscore separators, `studly` = StudlyCase.
- `class_brace_line`: Does the opening brace for a class go on the `same` line as the class keyword, or on the `next` line after it?
- `constant_names`: How are class constants named? `upper` = Uppercase with underscore separators.
- `true_false_null`: Are the `true`, `false`, and `null` keywords spelled as all `lower` case, or all `upper` case?
- `method_names`: How are methods named? `camel` = `camelCase`, `lower_under` = lowercase with underscore separators.
- `method_brace_line`: Does the opening brace for a method go on the `same` line as the method name, or on the `next` line?
- `control_brace_line`: Does the opening brace for a control structure go on the `same` line, or on the `next` line?
- `control_space_after`: Is there a space after the control structure keyword?
- `always_use_control_braces`: Do control structures always use braces?
- `else_elseif_line`: When using `else` or `elseif`, does it go on the `same` line as the previous closing brace, or does it go on the `next` line?
- `case_break_indent_from_switch`: How many times are `case` and `break` indented from an opening `switch` statement?
- `function_space_after`: Do function calls have a space after the function name and before the opening parenthesis?
- `closing_php_tag_required`: In files containing only PHP, is the closing `?>` tag required?
- `line_endings`: What type of line ending is used?
- `static_or_visibility_first`: When declaring a method, does `static` come first, or does the visibility come first?
- `control_space_parens`: In a control structure expression, is there a space after the opening parenthesis and a space before the closing parenthesis? `yes` = `if ( $expr )`, `no` = `if ($expr)`.
- `blank_line_after_php`: Is there a blank line after the opening PHP tag?
- `class_method_control_brace`: A summary of what line the opening braces go on for classes, methods, and control structures.
### A.3. Survey Results
```
indent_type:
    tab: 7
    2: 1
    4: 14
line_length_limit_soft:
    ?: 2
    no: 3
    75: 4
    80: 6
    85: 1
    100: 1
    120: 4
    150: 1
line_length_limit_hard:
    ?: 2
    no: 11
    85: 4
    100: 3
    120: 2
class_names:
    ?: 1
    lower: 1
    lower_under: 1
    studly: 19
class_brace_line:
    next: 16
    same: 6
constant_names:
    upper: 22
true_false_null:
    lower: 19
    upper: 3
method_names:
    camel: 21
    lower_under: 1
method_brace_line:
    next: 15
    same: 7
control_brace_line:
    next: 4
    same: 18
control_space_after:
    no: 2
    yes: 20
always_use_control_braces:
    no: 3
    yes: 19
else_elseif_line:
    next: 6
    same: 16
case_break_indent_from_switch:
    0/1: 4
    1/1: 4
    1/2: 14
function_space_after:
    no: 22
closing_php_tag_required:
    no: 19
    yes: 3
line_endings:
    ?: 5
    LF: 17
static_or_visibility_first:
    ?: 5
    either: 7
    static: 4
    visibility: 6
control_space_parens:
    ?: 1
    no: 19
    yes: 2
blank_line_after_php:
    ?: 1
    no: 13
    yes: 8
class_method_control_brace:
    next/next/next: 4
    next/next/same: 11
    next/same/same: 1
    same/same/same: 6
```
***
