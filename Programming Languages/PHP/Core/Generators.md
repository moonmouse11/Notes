# Generators
***
## Basic
**Генераторы** — легкий способ реализации простых **итераторов** без дополнительных ресурсов или сложностей, которые связаны с написанием класса, который реализует интерфейс `Iterator`.
Генератор поддерживает удобную передачу данных в циклы `foreach` без предварительной загрузки массива в память, что иногда вызывает превышение программой предела памяти или значительно увеличивает время обработки, которое уходит на генерацию результата. Вместо этого пишут функцию-генератор, которая аналогична стандартной функции, за исключением того, что вместо `return` единого значения с результатом работы цикла, генератор через ключевое слово `yield` умеет выдавать результат столько раз, сколько значений требуется перебрать. Как и итераторы, генераторы не поддерживают произвольный доступ к элементам массива.
***
## Description
Простой пример этого — переопределение функции `range()` как генератора. Стандартная функция `range()` генерирует массив, который состоит из значений, и возвращает его, что приводит к генерации больших массивов: например, вызов `range(0, 1000000)` займёт более 100 МБ оперативной памяти.
В качестве альтернативы можно создать генератор `xrange()`, который использует память только чтобы создать объект `Iterator` и сохранить текущее состояние, что потребует не больше 1 килобайта памяти.
###### Example
```php
function xrange($start, $limit, $step = 1)
{
    if ($start <= $limit) {
        if ($step <= 0) {
            throw new LogicException('Step must be positive');
        }

        for ($i = $start; $i <= $limit; $i += $step) {
            yield $i;
        }
    } else {
        if ($step >= 0) {
            throw new LogicException('Step must be negative');
        }

        for ($i = $start; $i >= $limit; $i += $step) {
            yield $i;
        }
    }
}

/* Обратите внимание, что и range() и xrange() дадут один и тот же вывод */

echo 'Single digit odd numbers from range():';
foreach (range(1, 9, 2) as $number) {
    echo "$number ";
}
echo "\n";

echo 'Single digit odd numbers from xrange():';
foreach (xrange(1, 9, 2) as $number) {
    echo "$number ";
}

/* Output */
/* Single digit odd numbers from range():  1 3 5 7 9 */
/* Single digit odd numbers from xrange(): 1 3 5 7 9 */
```
***
## Generator object 
Когда функция-генератор вызывается, она возвращает объект встроенного класса `Generator`. Этот объект реализует интерфейс `Iterator`, станет однонаправленным объектом итератора и предоставит методы, с помощью которых можно управлять его состоянием, включая передачу в него и возвращения из него значений.
***
## Syntax
**Функция-генератор** выглядит как обычная функция, за исключением того, что вместо возврата значения генератор выдаёт столько значений, столько ему необходимо. Каждая функция с оператором `yield` — функция-генератор.
Когда вызывается генератор, он возвращает объект, который можно итерировать. При итерации по этому объекту (например, в цикле `foreach`) PHP вызывает методы итерации объекта каждый раз, когда ему требуется значение, а затем сохраняет состояние генератора и при следующем вызове возвращает следующее значение.
Когда значения в генераторе закончатся, генератор может просто выполнить возврат, и вызывающий код продолжится так же, как если бы в массиве закончились значения.
> **Замечание:**
> Генераторы умеют возвращать значения, которые можно получить методом `Generator::getReturn()`.
***
## Yield
**Сердце функции-генератора** — ключевое слово `yield`. В простейшей форме инструкция `yield` похожа на инструкцию `return`, за исключением того, что вместо остановки выполнения функции и возврата, `yield` отдаёт значение коду, который выполняет цикл над генератором, и приостанавливает выполнение функции генератора.
###### Example
```php
/* Простой пример выдачи значений */

function gen_one_to_three()
{
    for ($i = 1; $i <= 3; $i++) {
        // Обратите внимание, что переменная $i сохраняет значение между вызовами
        yield $i;
    }
}

$generator = gen_one_to_three();

foreach ($generator as $value) {
    echo "$value\n";
}

```
> **Замечание**:
> Внутренне последовательные целочисленные ключи свяжутся с полученными значениями, как и в случае с неассоциативным массивом.

PHP также поддерживает ассоциативные массивы, и генераторы — не исключение. Помимо получения простых значений, как показывает пример, разрешается также одновременно получить ключ.
Синтаксис получения пары ключ и значение очень похож на синтаксис определения ассоциативных массивов.
```php
/* Получение пар ключ/значение */

/*
 * The input is semi-colon separated fields, with the first
 * field being an ID to use as a key.
 */

$input = <<<'EOF'
1;PHP;Likes dollar signs
2;Python;Likes whitespace
3;Ruby;Likes blocks
EOF;

function input_parser($input) {
    foreach (explode("\n", $input) as $line) {
        $fields = explode(';', $line);
        $id = array_shift($fields);

        yield $id => $fields;
    }
}

foreach (input_parser($input) as $id => $fields) {
    echo "$id:\n";
    echo "    $fields[0]\n";
    echo "    $fields[1]\n";
}
```
Чтобы получить значение `null`, нужно вызвать `yield` без аргументов. Ключ сгенерируется автоматически.
```php
/* Получение null */

function gen_three_nulls() {
    foreach (range(1, 3) as $i) {
        yield;
    }
}

var_dump(iterator_to_array(gen_three_nulls()));
```
**Функции-генераторы** умеют возвращать значения как по ссылке, так и по значению. Это делается так же, как и возврат ссылок из функций: добавлением амперсанда `&` к имени функции.
```php
/* Получение значений по ссылке */

function &gen_reference() {
    $value = 3;

    while ($value > 0) {
        yield $value;
    }
}

/*
 * Note that we can change $number within the loop, and
 * because the generator is yielding references, $value
 * within gen_reference() changes.
 */
foreach (gen_reference() as &$number) {
    echo (--$number).'... ';
}
```
***
## Делегирование генератора через yield from
Делегирование генератора позволяет получать значения из другого генератора, объекта `Traversable` или массива через ключевые слова `yield` `from`. Внешний генератор будет возвращать значения из внутреннего генератора, объекта или массива до тех пор, пока они не перестанут действовать, после чего выполнение продолжится во внешнем генераторе.
Если генератор используется с ключевыми словами `yield` `from`, выражение `yield` `from` также будет возвращать значения из внутреннего генератора.
```php
/* Основы работы с выражением yield from */

function count_to_ten() {
    yield 1;
    yield 2;
    yield from [3, 4];
    yield from new ArrayIterator([5, 6]);
    yield from seven_eight();
    yield 9;
    yield 10;
}

function seven_eight() {
    yield 7;
    yield from eight();
}

function eight() {
    yield 8;
}

foreach (count_to_ten() as $num) {
    echo "$num ";
}

/* Выражение yield from и возвращаемые значения */

function count_to_ten() {
    yield 1;
    yield 2;
    yield from [3, 4];
    yield from new ArrayIterator([5, 6]);
    yield from seven_eight();
    return yield from nine_ten();
}

function seven_eight() {
    yield 7;
    yield from eight();
}

function eight() {
    yield 8;
}

function nine_ten() {
    yield 9;
    return 10;
}

$gen = count_to_ten();
foreach ($gen as $num) {
    echo "$num ";
}
echo $gen->getReturn();
```
***
## Сохранение в массив (например, через функцию `iterator_to_array()`)
Ключевые слова `yield` `from` не сбрасывают ключи. Ключи, которые вернул объект `Traversable` или массив, сохранятся. Поэтому некоторые значения могут пересекаться по ключам с другими выражениями `yield` или `yield` `from`, что при записи в массив перезапишет прежние значения этим ключом.
Распространенный случай, когда это имеет значение, — функция `iterator_to_array()`, которая возвращает массив с ключом по умолчанию, что иногда приводит к неожиданным результатам. У функции `iterator_to_array()` есть второй параметр `preserve_keys`, которому можно присвоить значение `false` для генерации собственных ключей и игнорирования ключей, которые передаются из объекта `Generator`.
```php
/* Выражение yield from с функцией iterator_to_array() */

function inner() {
    yield 1; // key 0
    yield 2; // key 1
    yield 3; // key 2
}
function gen() {
    yield 0; // key 0
    yield from inner(); // keys 0-2
    yield 4; // key 1
}

/* pass false as second parameter to get an array [0, 1, 2, 3, 4] */
var_dump(iterator_to_array(gen()));
```
***
## Comparing generators with Iterator objects
Главное преимущество генераторов — простота. Требуется написать гораздо меньше шаблонного кода по сравнению с реализацией объекта класса `Iterator`, и этот код гораздо более простой и понятный. Например, эта функция и класс делают одно и то же.
```php
function getLinesFromFile($fileName) {
    if (!$fileHandle = fopen($fileName, 'r')) {
        return;
    }

    while (false !== $line = fgets($fileHandle)) {
        yield $line;
    }

    fclose($fileHandle);
}

// versus...

class LineIterator implements Iterator {
    protected $fileHandle;

    protected $line;
    protected $i;

    public function __construct($fileName) {
        if (!$this->fileHandle = fopen($fileName, 'r')) {
            throw new RuntimeException('Couldn\'t open file "' . $fileName . '"');
        }
    }

    public function rewind() {
        fseek($this->fileHandle, 0);
        $this->line = fgets($this->fileHandle);
        $this->i = 0;
    }

    public function valid() {
        return false !== $this->line;
    }

    public function current() {
        return $this->line;
    }

    public function key() {
        return $this->i;
    }

    public function next() {
        if (false !== $this->line) {
            $this->line = fgets($this->fileHandle);
            $this->i++;
        }
    }

    public function __destruct() {
        fclose($this->fileHandle);
    }
}
```
> Однако за эту гибкость приходится платить: генераторы — однонаправленные итераторы и их нельзя перемотать после начала итерации. Это также означает, что один и тот же генератор нельзя повторять несколько раз: генератор необходимо пересоздавать каждый раз, снова вызвав функцию генератора.
***
