# Exceptions
***
# Basics
Модель исключений PHP напоминает модель исключений других языков программирования. PHP умеет выбрасывать — `throw` — и ловить — `catch` — исключения. Код заключают в блок `try`, чтобы упростить обработку вероятных исключений. Каждому блоку `try` указывают как минимум один блок `catch` или `finally`.
Исключение будет «всплывать» по стеку вызовов функций, пока не найдёт блок `catch`, если функция выбросила исключение, а в текущей области видимости функции, которая её вызвала, нет блока `catch`. PHP выполнит каждый блок `finally`, который встретит по пути. Программа завершается фатальной ошибкой, когда стек вызовов не встречает блок `catch` и разворачивается до глобальной области видимости, если разработчик не установил глобальный обработчик исключений.
В коде допустимо выбрасывать только объект исключения, тип которого при проверке оператором `instanceof` соответствует интерфейсу `Throwable`. Попытка выбросить объект, который не выполняет это условие, приведёт к фатальной ошибке PHP.
***
## `catch`
Блок `catch` определяет, как реагировать на исключение, которое выбросил код. Блок catch определяет один или больше типов исключений или ошибок, которые он обрабатывает, и необязательную переменную, которой блок присвоит исключение. До PHP 8.0.0 переменную требовалось указывать. Объект исключения обработает первый блок `catch`, с которым столкнутся исключение или ошибка того типа или подтипа, который ожидает блок.
Блоки `catch` записывают один за другим, чтобы перехватывать исключения разных классов. Нормальное выполнение, когда блок `try` не выбросил исключение, продолжится после последнего блока `catch`, который определили в последовательности. Внутри блока catch допустимо выбрасывать, а точнее — повторно выбрасывать исключения через ключевое слово throw. PHP продолжит выполнение кода после блока `catch`, который сработал, если внутри блока не выбросили исключение.
При появлении исключения PHP не выполнит код, который идёт за инструкцией, а попытается найти первый подходящий блок `catch`. PHP выдаст фатальную ошибку с сообщением `Uncaught Exception`, если исключение не поймали и через функцию `set_exception_handler()` не определили обработчик исключений.
***
## `finally`
Блок `finally` также допустимо указывать после или вместо блоков `catch`. PHP выполнит код в блоке `finally` после блоков `try` и `catch`, независимо от того, выбросил ли код исключение, и до возобновления нормального выполнения.
Заслуживает внимания взаимодействие между блоком `finally` и инструкцией `return`. PHP выполнит блок `finally`, даже если встретит внутри блоков `try` или `catch` инструкцию return. Больше того, когда PHP встречает инструкцию `return`, он вычисляет её, но вернёт результат после выполнения блока `finally`. Кроме того, PHP вернёт значение из блока `finally`, если блок `finally` тоже содержит инструкцию `return`.
***
## Global Exception Handler
Глобальный обработчик исключений, если обработчик установили, перехватит исключение, если исключению разрешили всплывать до глобальной области видимости. Функция `set_exception_handler()` устанавливает функцию, которую PHP вызовет вместо блока `catch`, если в коде не вызвали другие блоки. Эффект по существу такой же, как если бы всю программу обернули в блок `try-catch` с этой функцией в качестве `catch`.
***
## Extending Exceptions
Пользовательский класс исключения определяют путём расширения встроенного класса `Exception`.
```php
/* Встроенный класс Exception */

class Exception implements Throwable  
{  
protected $message = 'Unknown exception'; // exception message  
private $string; // __toString cache  
protected $code = 0; // user defined exception code  
protected $file; // source filename of exception  
protected $line; // source line of exception  
private $trace; // backtrace  
private $previous; // previous exception if nested exception  
  
public function __construct($message = '', $code = 0, ?Throwable $previous = null);  
  
final private function __clone(); // Inhibits cloning of exceptions.  
  
final public function getMessage(); // message of exception  
final public function getCode(); // code of exception  
final public function getFile(); // source filename  
final public function getLine(); // source line  
final public function getTrace(); // an array of the backtrace()  
final public function getPrevious(); // previous exception  
final public function getTraceAsString(); // formatted string of trace  
  
// Overrideable  
public function __toString(); // formatted string for display  
}
```
В конструкторе класса-наследника нужно также вызвать конструктор родительского класса — `parent::__construct()`, когда класс расширяет класс `Exception` и переопределяет конструктор, чтобы гарантировать, что родительский класс правильно присвоил значения доступным данным. Метод `__toString()` допустимо переопределять, чтобы настроить пользовательский вывод, когда с объектом исключения работают как со строкой.
> Исключения нельзя клонировать. Попытка клонировать исключение приведёт к фатальной ошибке `E_ERROR`.
```php
/* Наследование класса Exception */

/**
 * Define a custom exception class
 */
class MyException extends Exception
{
    // Redefine the exception so message isn't optional
    public function __construct($message, $code = 0, ?Throwable $previous = null) {
        // some code

        // make sure everything is assigned properly
        parent::__construct($message, $code, $previous);
    }

    // custom string representation of object
    public function __toString() {
        return __CLASS__ . ": [{$this->code}]: {$this->message}\n";
    }

    public function customFunction() {
        echo "A custom function for this type of exception\n";
    }
}


/**
 * Create a class to test the exception
 */
class TestException
{
    public $var;

    const THROW_NONE    = 0;
    const THROW_CUSTOM  = 1;
    const THROW_DEFAULT = 2;

    function __construct($avalue = self::THROW_NONE) {

        switch ($avalue) {
            case self::THROW_CUSTOM:
                // throw custom exception
                throw new MyException('1 is an invalid parameter', 5);
                break;

            case self::THROW_DEFAULT:
                // throw default one.
                throw new Exception('2 is not allowed as a parameter', 6);
                break;

            default:
                // No exception, object will be created.
                $this->var = $avalue;
                break;
        }
    }
}


// Example 1
try {
    $o = new TestException(TestException::THROW_CUSTOM);
} catch (MyException $e) {      // Will be caught
    echo "Caught my exception\n", $e;
    $e->customFunction();
} catch (Exception $e) {        // Skipped
    echo "Caught Default Exception\n", $e;
}

// Continue execution
var_dump($o); // Null
echo "\n\n";


// Example 2
try {
    $o = new TestException(TestException::THROW_DEFAULT);
} catch (MyException $e) {      // Doesn't match this type
    echo "Caught my exception\n", $e;
    $e->customFunction();
} catch (Exception $e) {        // Will be caught
    echo "Caught Default Exception\n", $e;
}

// Continue execution
var_dump($o); // Null
echo "\n\n";


// Example 3
try {
    $o = new TestException(TestException::THROW_CUSTOM);
} catch (Exception $e) {        // Will be caught
    echo "Default Exception caught\n", $e;
}

// Continue execution
var_dump($o); // Null
echo "\n\n";


// Example 4
try {
    $o = new TestException();
} catch (Exception $e) {        // Skipped, no exception
    echo "Default Exception caught\n", $e;
}

// Continue execution
var_dump($o); // TestException
echo "\n\n";
```
***
