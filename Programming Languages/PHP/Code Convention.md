# [Code Convention](https://github.com/roistat/php-code-conventions?tab=readme-ov-file)
***
## **Принципы**
**Принципы** — это способы соблюдения описанных выше ценностей. Они чуть более детальны, содержат основные методологии разработки и подходы, которыми мы руководствуемся.
Код должен быть:
- Понятным, явным. Явное лучше, чем неявное. Например, не должны использоваться магические методы. Также нельзя использовать `exit` и любые другие операторы, которые могут завершить или изменить работу процесса.
- Удобным для использования сейчас
- Удобным для использования в будущем
- Должен стремиться к соблюдению принципов [KISS](https://ru.wikipedia.org/wiki/KISS_(%D0%BF%D1%80%D0%B8%D0%BD%D1%86%D0%B8%D0%BF)), [SOLID](https://ru.wikipedia.org/wiki/SOLID_(%D0%BE%D0%B1%D1%8A%D0%B5%D0%BA%D1%82%D0%BD%D0%BE-%D0%BE%D1%80%D0%B8%D0%B5%D0%BD%D1%82%D0%B8%D1%80%D0%BE%D0%B2%D0%B0%D0%BD%D0%BD%D0%BE%D0%B5_%D0%BF%D1%80%D0%BE%D0%B3%D1%80%D0%B0%D0%BC%D0%BC%D0%B8%D1%80%D0%BE%D0%B2%D0%B0%D0%BD%D0%B8%D0%B5)), [DRY](https://ru.wikipedia.org/wiki/Don%E2%80%99t_repeat_yourself), [GRASP](https://ru.wikipedia.org/wiki/GRASP)
- Код должен обладать слабым зацеплением и высокой связностью (подробно это описано в GRASP). Любая часть системы должна иметь изолированную логику и при надобности внешний интерфейс, который позволяет с этой логикой работать. Любая внутренняя часть должна иметь возможность быть измененной без какого-либо ущерба внешним системам
- Код должен быть таким, чтобы его можно было автоматически отрефакторить в IDE (например, Find usages и Rename в PHPStorm). То есть должен быть слинкован типизацией и PHPDoc'ами
- В БД не должны храниться части кода (даже названия классов, переменных и констант), так как это делает невозможным автоматический рефакторинг
- Последовательным. Код должен читаться сверху вниз. Читающий не должен держать что-то в уме, возвращаться назад и интерпретировать код иначе. Например, надо избегать обратных циклов `do {} while ();`
- Должен иметь минимальную [цикломатическую сложность](https://ru.wikipedia.org/wiki/%D0%A6%D0%B8%D0%BA%D0%BB%D0%BE%D0%BC%D0%B0%D1%82%D0%B8%D1%87%D0%B5%D1%81%D0%BA%D0%B0%D1%8F_%D1%81%D0%BB%D0%BE%D0%B6%D0%BD%D0%BE%D1%81%D1%82%D1%8C)
***
## Общие правила

### 📖 Запрещен неиспользуемый код
Если код можно убрать, и работа системы от этого не изменится, его быть не должно.
```php
/* Плохо */

if (false) {
    legacyMethodCall();
}

$legacyCondition = true;
if ($legacyCondition) {
    finalizeData($data);
}

/* Хорошо */

finalizeData($data);
```
### 📖 Не должны напрямую использоваться функции стандартной библиотеки PHP, если этого можно избежать
Это упростит миграцию кода на новую версию языка. Часто в новой версии языка удаляются какие-либо функции или изменяется их работа. Чем меньше идет завязки на язык и его версию, тем лучше.
Специфичные функции всегда лучше использовать через функции-обёртки внутри проекта. Тогда в случае миграции придется исправлять одно место, а не тысячу.
Как понять, можно ли использовать встроенную в PHP функцию или нет?
- Если эта функция уже используется повсеместно в проекте, значит, её можете использовать и вы. Например, это может быть `explode`/`implode`. Если эти функции будут изменены в новой версии PHP, то в любом случае придется переделать много кода и делать это будет автоматика.
- Если эта функция не используется или используется только через обёртку в специализированном сервисе, то и вы использовать её можете только через обёртку (добавляется при необходимости).
```php
/* Плохо */

if (ctype_digit($number)) {

}
$distance = levenshtein($text1, $text2);
$urlParts = parse_url($url);

/* Хорошо */

if ($number >= 0) {

}
$distance = $textService->levenshtein($text1, $text2);
$urlParts = $urlService->parseUrl($url);
```
### 📖 Вместо отсутствующего скалярного значения используется null
0 и пустую строку нельзя использовать в качестве показателя отсутствия значения.
```php
function sendEmail(string $title, ?string $message = null, ?string $date = null): void {

}

// сообщение не было передано
$object->sendEmail('Title', null, '2017-01-01');

// было передано пустое сообщение
$object->sendEmail('Title', '', '2017-01-01');
```
Однако, это правило не относится к массивам.
```php
/* Плохо */

function deleteUsersByIds(array $ids = [], bool $someOption = false) {

}

deleteUsersByIds(null, true);

/* Хорошо */

deleteUsersByIds([], true);
```

Итого: использование пустой строки почти всегда является ошибкой.
***
## **Правила разделения бизнес-логики**
### 📖 Сервисы
**Сервис** – это класс без состояния, содержащий бизнес-логику. Данные для обработки сервис получает либо в виде параметров публичных методов, либо других сервисов.
_**Сервис не может использовать в качестве источника данных глобальные переменные или окружение:**_
```php
/* Плохо */

class User {

    public function loadUsers() {
        $path = getenv('DATA_PATH'); // так нельзя!
        // ...
    }
}

/* Хорошо */

class Env {
    
    public function getDataPath(): string {
        return getenv('DATA_PATH');
    }
}

class User {

    /**
     * @var Env
     */
    private $_env;
    
    public function __construct(Env $env) {
        $this->_env = $env;
    }

    public function loadUsers() {
        $path = $this->_env->getDataPath();
        // ...
    }
}
```
Однако, это правило не работает, если получение данных из внешних источников — **единственная бизнес-логика сервиса.**
Для работы с хранилищем мы используем репозиторий. Это частный случай сервиса, но он получает данные из БД через адаптеры.
### 📖 Контроллеры
**Контроллер** принимает и обрабатывает запросы. Он получает параметры на вход, запрашивает данные из сервисов и возвращает представление.
### 📖 Модели
**Модель** — простой объект со свойствами, не содержащий никакой другой бизнес-логики, кроме геттеров и сеттеров. Геттер — метод, позволяющий получить какую-то информацию из внутреннего состояния объекта. Это не обязательно поле, как оно есть. Он может брать значения нескольких полей и делать простые манипуляции с ними (не запрашивая внешней продуктовой бизнес-логики). Сеттер — аналогично может изменять внутреннее состояние одного или нескольких полей без запросов «наружу».
```php
/* Условный упрощенный пример */

class Bill {
    public $id;
    public $sum;
    public $isPaid;
    public $paidDate
    // ...
    
    public function markAsPaid(\DateTime $paymentDate) {
        $this->isPaid = true;
        $this->paidDate = $paymentDate;
    }
}
```

Желательно делать модели неизменяемыми, см. **Работа с объектами** . Хотите больше гибкости — можно использовать **chain-объекты**.
### 📖 Представления
Представлением в зависимости от требуемого ответа сервера может быть HTML-шаблон, API-объект или что-то иное. Обратите внимание, API-объект и модель данных это разные сущности, даже если у них совпадает название и все поля. Нельзя просто вернуть в JSON-ответе сервера модель из хранилища:
```php
/* Плохо */

public function actionUsers(): Response {
    $users = $repository->loadUsers();
    return new Response(['data' => $users]);
}
```
Свойства модели хранилища могут поменяться из-за новых технических требований, но объект API это продукт, вы должны изменять его явно:
```php
/* Хорошо */

// src/Entity/Api/User.php
namespace Entity\Api;

class User {
    public $id;
    public $name;
}

// src/Controller/Api/User.php
public function actionUsers(): Response {
    $users = $repository->loadUsers();
    $apiUsers = array_map(function ($user) {
        return $this->_convertUserToApiObject($user);
    }, $users);
    return new Response(['data' => $apiUsers);
}

private function _convertUserToApiObject(Entity\Mapper\User $user): Entity\Api\User {
    // ...
}
```

***
## Работа с файлами
### 📖 Названия файлов пишутся строчными буквами через underscore
Кроме случаев, когда внутри файла содержится класс. В таком случае файл должен повторять названия класса, то есть должен быть написан в стиле `UpperCamelCase`. Аналогично обычные директории и пространства имён.
### 📖 Файлы классов должны быть расположены в директориях в соответствии со стандартом PSR-0
(Только PSR-0 считается устаревшим).
***
## Работа с переменными
### 📖 Название переменных пишутся через `camelCase`
### 📖 Название переменных должно соответствовать содержанию
Нельзя писать короткие названия, например `$c`. Нельзя назвать переменную `$day` и хранить в ней массив статистических данных за этот день.
### 📖 Часто упоминаемые объекты именуются одинаково во всем проекте
```php
/* Плохо */

$customer = new User();
$client = new User();
$object = new User();

/* Хорошо */

$user = new User();
```
### 📖 Признак объекта добавляется к названию
Если это отфильтрованный по какому-то признаку проект, то признак добавляется к названию. Например, `$unpaidProject`.
### 📖 Переменные, отражающие свойства объекта, должны включать название объекта
```php
/* Плохо */

$project = new Project();
$name = $project->name;
$project = $project->name;

/* Хорошо */

$project = new Project();
$projectName = $project->name;
```
### 📖 Переменные по возможности должны называться на корректном английском
```php
/* Плохо */

$usersStored = [];

/* Хорошо */

$storedUsers = [];
```
Исключение: сгруппированные по некому признаку поля или константы. В этом случае можно использовать префикс.
```php
class ProjectInfo {
    const STATUS_READY = 1;
    const STATUS_BLOCKED = 2;

    public $billingIsPaid;
    public $billingPaidDate;
    public $billingSum;
}
```
### 📖 К переменной нельзя обращаться по ссылке (через &)
Амперсанды могут использоваться только как логические или битовые операторы.
```php
/* Плохо */

function removePrefix(string &$name) {

}

/* Хорошо */

function removePrefix(string $name): string {

	return $result;
}
```
### 📖 Переменные и свойства объекта должны являться существительными и называться так, чтобы они правильно читались при использовании, а не при инициализации.
```php
/* Плохо */

$object->expire_at
$object->setExpireAt($date);
$object->getExpireAt();

/* Хорошо */

$object->expiration_date;
$object->setExpirationDate($date);
$object->getExpirationDate();
```
### 📖 В названии переменной не должно быть указания типа
Нельзя писать `$projectsArray`, надо писать просто `$projects`. Это же касается и форматов (JSON, XML и т.п.), и любой другой не относящейся к предметной области информации.
```php
/* Плохо */

$projectsList = $repository->loadProjects();
$projectsListIds = $utils->extractField('id', $projectsList);

/* Хорошо */

$projects = $repository->loadProjects();
$projectsIds = $utils->extractField('id', $projects);
```
### 📖 Нельзя изменять переменные, которые передаются в метод на вход
Исключение — если эта переменная объект.
```php
/* Плохо */

function parseText(string $text) {
    $text = trim($text);

}

/* Хорошо */

function parseText(string $text) {
    $trimmedText = trim($text);

}
```
### 📖 Каждая переменная должна быть объявлена на новой строке
```php
/* Плохо */

$foo = false; $bar = true;

/* Хорошо */
$foo = false;
$bar = true;
```
### 📖 Нельзя нескольким переменным присваивать одно и то же значение
```php
/* Плохо */

function loadUsers(array $ids) {
    $usersIds = $ids;

}
```
### 📖 Оператор `clone` должен использоваться только в тех случаях, когда без него не обойтись
Также можно использовать `clone`, если без него код серьёзно усложнится, а с ним станет понятным и очевидным. Простой пример — клонирование объектов `DateTime`. Или использование клонирования для сравнения двух версий объекта: старой и новой.
```php
/* Плохо */

function loadAnalyticsData(\DateTime $intervalStart) {
    $intervalEnd = new \DateTime($intervalStart->format('Y-m-d H:i:s'));
    $intervalEnd->modify('+1 day');
}

function updateUser(User $user) {
    $oldUser = new User();
    $oldUser->id = $user->id;
    $oldUser->name = $user->name;

	logObjectDiff($user, $oldUser);
}

/* Хорошо */

function loadAnalyticsData(\DateTime $intervalStart) {
    $intervalEnd = clone $intervalStart;
    $intervalEnd->modify('+1 day');
}

function updateUser(User $user) {
    $oldUser = clone $user;

	logObjectDiff($user, $oldUser);
}
```
### 📖 Запрещено использовать результат операции присваивания
```php
/* Плохо */

$foo = $bar = strlen($someVar);

/* Хорошо */

$bar = strlen($someVar);
$foo = $bar;

/* Плохо */

$this->_callSomeFunc($bar = strlen($foo));

/* Хорошо */

$bar = strlen($foo);
$this->_callSomeFunc($bar);

/* Плохо */

if (strlen($foo = json_encode($bar)) > 100) {

}

/* Хорошо */

$foo = json_encode($bar);
if (strlen($foo) > 100) {

}
```
### 📖 Нельзя использовать константы через метод `constant`
```php
/* Плохо */

public function getProjectDir(): string {
    $prefix = 'ACME_';
    $name = $prefix . 'PROJECT_DIR';
    return constant($name);
}

public function getProjectDir(): string {
    return constant('ACME_PROJECT_DIR');
}

/* Хорошо */

public function getProjectDir(): string {
    return ACME_PROJECT_DIR;
}
```
***
## Логические переменные и методы
### 📖 Названия boolean методов и переменных должны содержать глагол `is`, `has` или `can`
Переменные правильно называть, описывая их содержимое, а методы — задавая вопрос. Если переменная содержит свойство объекта, следуем правилу **признак объекта добавляется к названию**.
```php
/* Плохо */

$isUserValid = $user->valid();
$isProjectAnalytics = $accessManager->getProjectAccess($project, 'analytics');

/* Хорошо */

$userIsValid = $user->isValid();
$projectCanAccessAnalytics = $accessManager->canProjectAccess($project, 'analytics');
```
Геттеры именуются аналогично переменным:
```php
/* Пример */

class User {
    private $_billingIsPaid;
    private $_isEnabled;

    public function isEnabled() {
        return $this->_isEnabled;
    }

    public function billingIsPaid() {
        return $this->_billingIsPaid;
    }
}
```
Такое именование позволяет легче читать условия:
```php
/* Пример */

// if user is valid, then do something
if ($userIsValid) {
    // do something
}
```
### 📖 Запрещены отрицательные логические названия
```php
/* Плохо */

if ($project->isInvalid()) {

}
if ($project->isNotValid()) {

}
if ($accessManager->isAccessDenied()) {

}

/* Хорошо */

if (!$project->isValid()) {

}
if (!$accessManager->isAccessAllowed()) {

}
if ($accessManager->canAccess()) {

}
```
### 📖 Не используйте boolean переменные (флаги) как параметры функции
Флаг в качестве параметра это признак того, что функция делает больше одной вещи, нарушая [принцип единственной ответственности (Single Responsibility Principle или SRP)](https://ru.wikipedia.org/wiki/%D0%9F%D1%80%D0%B8%D0%BD%D1%86%D0%B8%D0%BF_%D0%B5%D0%B4%D0%B8%D0%BD%D1%81%D1%82%D0%B2%D0%B5%D0%BD%D0%BD%D0%BE%D0%B9_%D0%BE%D1%82%D0%B2%D0%B5%D1%82%D1%81%D1%82%D0%B2%D0%B5%D0%BD%D0%BD%D0%BE%D1%81%D1%82%D0%B8). Избавляйтесь от них, выделяя код внутри логических блоков в отдельные ветви выполнения.
Свойства объекта при этом могут иметь логический тип. Сам объект, являясь лишь контейнером для данных, не выполняет логики, а значит о нарушении принципа единственной ответственности речь не идет. Это означает, что **аргумент конструктора** такого объекта тоже **имеет право быть логического типа**.
```php
/* Плохо */

function someMethod() {
    // ...
    $projectNotificationIsEnabled = $notificationManager->isProjectNotificationEnabled($project);
    storeUser($user, $projectNotificationIsEnabled);
}

function storeUser(User $user, bool $isNotificationEnabled) {
    // ...
    if ($isNotificationEnabled) {
        notify('new user');
    }
}


/* Хорошо */

function someMethod() {
    // ...
    storeUser($user);
    if ($notificationManager->isProjectNotificationEnabled($project)) {
        notify('new user');
    }
}

function storeUser(User $user) {
    // ...
}

// Использование флага в конструкторе для инициализации свойства логического типа
class SchrodingerCat {
    private $_isAlive;
    public function __construct(bool $isAlive) {
        $this->_isAlive = $isAlive;
    }
}
```

***
## Работа с массивами
### 📖 Для конкатенации массивов запрещено использовать оператор `+`.
Обратите внимание, что `array_merge` все числовые ключи приводит к `int`, даже если они записаны строкой.
```php 
/* Плохо */

return $initialData + $loadedData;

/* Хорошо */

namespace Service;

class ArrayUtils {

    public function mergeArrays(array $array1, array $array2): array {
        return array_merge($array1, $array2);
    }
}

public function someMethod() {
    return $this->_arrayUtils->mergeArrays($initialData, $loadedData);
}
```
### 📖 Для проверки наличия ключа в ассоциативном массиве используем `array_key_exists`, а не `isset`
`isset` проверяет не ключ на его наличие, а значение этого ключа, если он есть. Это разные методы с разным поведением и назначением. Если вы хотите проверить значение ключа, то делайте это явно. Сначала явно проверьте наличие ключа через `array_key_exists` и обработайте ситуацию его отсутствия, затем приступайте к работе со значением.
```php
/* Плохо */

function processRequestData(array $requestData) {
    $data = [];
    if (isset($requestData['project_key'])) {
    	// ...
    }
    return $data;
}

/* Хорошо */

function processRequestData(array $requestData) {
    $data = [];
    if (array_key_exists('project_key', $requestData)) {
    	// ...
    }
    return $data;
}
```
Допустимо использовать сокращенный вариант `??`, с явным указанием дефолтного значения.
```php
/* Пример */

function getProjectKey(array $requestData) {
    return $requestData['project_key'] ?? null;
}
```
### 📖 Ассоциативный массив мы используем как hashmap
То есть не применяем разные встроенные в PHP инструменты. Приведем несколько очевидных примеров (однако, правило ими не исчерпывается):
#### 📖 Нельзя сортировать ассоциативные массивы
```php
/* Плохо */

$arr = [
    'project_key' => 'foo',
    'key' => 'bar',
    'user_id' => 300,
];

uasort($arr);
```
#### 📖 Нельзя смешивать в массиве строковые и числовые ключи
```php
/* Плохо */

$arr = [
    'project_key' => 'foo',
    'key' => 'bar',
    'user_id' => 300,
    1 => 'value1',
    2 => 'value2',
];

$arr[3] = 'value3';
```
#### 📖 Для проверки наличия значения по индексу в обычных (не ассоциативных) массивах используем `count($array) > N`
```php
/* Плохо */

if (array_key_exists(1, $users)) {

}
if (isset($users[1])) {

}

/* Хорошо */

if (count($users) > 1) {

}
```
***
## Работа со строками
### 📖 Строки обрамляются одинарными кавычками
Двойные кавычки используются только, если:
- Внутри строки должны быть одинарные кавычки
- Внутри строки используется подстановка переменных
- Внутри строки используются спец. символы `\n`, `\r`, `\t` и т.д.
```php
/* Плохо */

$string = "Some string";
$string = 'Some \'string\'';
$string = "\t".'Some string'."\n";

/* Хорошо */

$string = 'Some string';
$string = "Some 'string'";
$string = "\tSome string\n";
```
### 📖 Вместо лишней конкатенации используем подстановку переменных в двойных кавычках с помощью фигурных скобок
```php
/* Плохо */

$string = 'Object with type "' . $object->type() . '" has been removed';

/* Хорошо */

$string = "Object with type \"{$object->type()}\" has been removed";
```
***
## Работа с датами
### 📖 Дата всегда должна быть представлена `DateTime`, интервал как `DateInterval`
```php
/* Плохо */

$date = $request->get('date');
$interval = 86400*30;
loadSomeData($date, $interval);

/* Хорошо */

$date = $this->_dateService->instance($request->get('date'));
$interval = new \DateInterval('P30D');
loadSomeData($date, $interval);
```
### 📖 Запрещено создавать объект даты при помощи `new \DateTime()`
В проекте для этого должен быть фабричный метод в сервисе для работы с датами.
```php
/* Плохо */

$date = new \DateTime();

/* Хорошо */

$date = $this->_dateService->instance();
```
(А нужен ли сервис работы с датами в маленьком проекте, или для вызова один раз конвенция умалчивает (А в Laravel вшит Carbon - а в Битрике своя обертка)).
### 📖 Если дата должна быть представлена скалярным значением, необходимо использовать строку
- строка с датой и временем должна быть везде в одинаковом формате
- формат не должен включать временную зону, если для этого нет особых требований
- при прочих равных в дате без часового пояса всегда подразумевается UTC0
- если строку по какой-то причине невозможно использовать, используем `int`
```php
/* Плохо */

class User {
    public $creation_time;
}

$user->creation_time = time();

/* Хорошо */

class User {
    /**
     * @type string
     */
    public string $creation_date;
}

$user->creation_date = '2018-01-18 12:54:11';
```
### 📖 При работе с интервалами/периодами запрещено указывать месяц или год
В зависимости от текущей даты месяц и год могут принимать разные временные промежутки (високосный и обычный год, разное количество дней в месяце). Вместо этого в качестве указания интервала используем дни, часы, минуты, секунды.
```php
/* Плохо */

$dateTime = new \DateTime('-2 month');
$dateInterval = new \DateInterval('P2M');

/* Хорошо */

$dateTime = new $this->_dateTime->instance('-60 days');
$dateInterval = new \DateInterval('P60D');
```
Месяц или год необходимо использовать, если это напрямую указано в требованиях задачи как календарный месяц или календарный год.
***
## Работа с пространствами имён
### 📖 Все пространства имён должны быть подключены через `use` в начале файла. В самом коде не должно быть обратного слеша перед названием пространства имён
```php
/* Плохо */

$object = new \Some\Object();

/* Хорошо */

use Some;
$object = new Some\Object();
```
### 📖 В свою очередь обычные классы без пространства имён не должны быть подключены через `use`
```php
/* Плохо */

use TimeZone;
$date = new TimeZone('Europe\Moscow');

/* Хорошо */

$date = new \TimeZone('Europe\Moscow');
```
### 📖 Нельзя подключать несколько классов из одного пространства имён через `use`
```php
/* Плохо */

use Entity\User;
use Entity\Project;
 
$user = new User();
$project = new Project();

/* Хорошо */

use Entity;
  
$user = new Entity\User();
$project = new Entity\Project();
```
### 📖 Следует избегать использования псевдонима (alias)
Они запутывают код и его понимание. Если у вас совпадают названия пространств имён, то, скорее всего, вы делаете что-то не так. Допустимо использовать псевдоним, если другое решение будет слишком сложным.
```php
/* Плохо */

use Component\User;
use Entity\User as UserEntity;

$user = new UserEntity();

/* Хорошо */

use Component\User;
use Entity;

$user = new Entity\User();
```
***
## **Работа с методами**
### 📖 Должна быть использована максимально возможная типизация для вашей версии PHP. Все параметры и их типы должны быть описаны в объявлении метода либо в `PHPDoc`. Возвращаемое значение тоже.
```php
/* Плохо */

/**
 * @param $id
 * @param $name
 * @param $tags
 * @return mixed
 */
function storeUser($id, $name, $tags = []) {
    // ...
}

/* Хорошо */

// для PHP 7.1
function makeCoffee(string $type, int $volume): Coffee {
	// ...
}

// в PHP 7.1 тип элементов массива в объявлении метода указать нельзя, поэтому добавляем PHPDoc
/**
 * @param int $id
 * @param string $name
 * @param string[] $tags
 * @return User|null
 */
function storeUser(int $id, string $name, array $tags = []): ?User {
    // ...
}

// если метод возвращает смешанный тип данных, то необходимо явно это указать
/**
 * @param callable $callback
 * @return mixed
 */
function execute(callable $callback) {
    // ...
}

// для PHP 5.6
// без строгой типизации возвращаемых типов любой метод
// может вернуть null, так что можно его не указывать в PHPDoc
/**
 * @param int $id
 * @param string $name
 * @param string[] $tags
 * @return User
 */
function storeUser($id, $name, array $tags = []) {
    // ...
}
```
(Старая конвенция, в PHP 8 все гораздо проще).
### 📖 Все возможные типы должны быть определены в `PHPDoc`
Наибольшую пользу это приносит при работе с массивами:
```php
/* Плохо */

/**
 * @param array $users
 * @param mixed $project
 * @param int $timestamp
 * @return mixed
 */ 
public function someMethod($users, $project, $timestmap) {
    foreach ($users as $user) {
        // IDE не сможет определить тип $user
    }
    // ...
}

/* Хорошо */

/**
 * @param Users[] $users
 * @param Project $project
 * @param int $timestamp
 * @return Foo
 */
public function someMethod(array $users, Project $project, int $timestmap): Foo {
    foreach ($users as $user) {
        // подсказки IDE и рефакторинг работают корректно
    }
    // ...
}
```
### 📖 Название метода должно начинаться с глагола и соответствовать правилам именования переменных.
```php
/* Плохо */

public function items() {
    // ...
}
public function convertedDataObject(array $data) {
    // ...
}

/* Хорошо */

public function loadItems() {
    // ...
}
public function convertDataToObject(array $data) {
    // ...
}
```
### 📖 Нельзя использовать глагол get в геттерах
Например, вместо `getDate()` следует писать `date()`. Геттер — метод, работающий только с полями своего объекта.
```php
/* Плохо */

class User {
    private $_date;
    private $_customFields;

    public function getDate() {
        return $this->_date;
    }

    public function getCustomFields() {
        return json_decode($this->_customFields);
    }
}

/* Хорошо */

class User {
    private $_date;
    private $_customFields;

    public function date() {
        return $this->_date;
    }

    public function decodedCustomFields() {
        return json_decode($this->_customFields);
    }
}
```
### 📖 Методы названия, которых начинаются c `check` и `validate`, должны выбрасывать исключения и не возвращать значения
```php
/* Плохо */

public function validateRequestData(array $requestData): bool {
    if (!array_key_exists('key', $requestData)) {
        return false;
    }
    // ...
    return true;
}

/* Хорошо */

public function validateRequestData(array $requestData): void {
    if (!array_key_exists('key', $requestData)) {
        throw new ValidationError('Field "key" not found');
    }
    // ...
}
```
### 📖 Все методы класса по умолчанию должны быть private
Если метод используется наследниками класса, то он объявляется `protected`. Если используется сторонними классами, тогда `public`.
### 📖 Использование рекурсий допускается только в исключительном случае
Если код без рекурсии будет очень сложен для написания и понимания и при этом рекурсия гарантированно не выйдет за ограничения стека вызовов.
### 📖 Запрещается кешировать данные в статических переменных метода
Для кеширование в памяти используем свойство объекта.
```php
/* Плохо */

public function loadData() {
    static $_cachedData;
    if ($_cachedData === null) {
        $_cachedData = [];
    }
    return $_cachedData;
}

/* Хорошо */

private $_cachedData = [];

public function loadData() {
    if ($this->_cachedData === null) {
        $this->_cachedData = [];
    }
    return $this->_cachedData;
}
```
### 📖 Параметры в методах должны следовать в следующем порядке: обязательные → часто используемые → редко используемые
Нужно соблюдать читаемость при написании вызова.
```php
/* Плохо */

public function method($required, $practicallyUnused = 5, $often = [], $lessOften = null)
public function filter($value, $name, $operator) // ...$service->filter(15, "id", "=")

/* Хорошо */

public function method($required, $often = [], $lessOften = null, $practicallyUnused = 5)
public function filter($name, $operator, $value) // ...$service->filter("id", "=", 15)
```
### 📖 Nullable параметры должны быть помечены `?`, даже если указано значение по умолчанию.
```php
/* Плохо */

function f(int $number = null) {}

/* Хорошо */

function f(?int $number = null) {}
function f(?int $number) {}
```
***
## **Возврат результата работы метода**
### 📖 Метод всегда должен возвращать только одну структуру данных (или `null`) или ничего не возвращать
Метод не может в разных ситуациях возвращать разные типы данных.
```php
/* Плохо */

function loadUser() {
    if ($someCondition) {
        return ['id' => 1];
    }
    return new User();
}

/* Хорошо */

function loadUser(): User {
    if ($someCondition) {
        $user = new User();
        $user->id = 1;
        return $user;
    }
    return new User();
}
```
### 📖 Если метод возвращает один объект (или скалярный тип), то в случае, если объект не найден, возвращается `null`
Если же метод возвращает список объектов, то в случае, когда список пуст, возвращает пустой массив. Нельзя возвращать вместо пустого списка `null`.
```php
/* Плохо */

function loadUsers() {
    if ($someCondition) {
        return null;
    }
    return [new User()];
}

/* Хорошо */

/**
 * @return User[]
 */
function loadUsers(): array {
    if ($someCondition) {
        return [];
    }
    return [new User()];
}
```
Однако, бывают ситуации, когда надо явно указать, что данные отсутствуют, а не содержат пустой список.
**Пример:** значения полей объекта задаются пользователем. Возможны две ситуации:
- пользователь не знает, каким категориям принадлежит объект — `null`
- пользователь знает, что объект не принадлежит ни одной категории — пустой массив (`[]`)
Тогда для получения категорий объекта будет правильным такой код:
```php
/* Пример */

/**
 * для PHP 5.6
 * @return array|null
 */
function getObjectCategories($object) {
    if ($object->categories === null) {
        return null;
    }
    return parseCategories($object->categories);
}

// для PHP 7.1
function getObjectCategories($object): ?array {
    if ($object->categories === null) {
        return null;
    }
    return parseCategories($object->categories);
}
```
### 📖 В больших методах возвращаемая переменная должна называться `$result`
Если у вас большой метод (больше 15 строк), возвращаемая переменная должна называться `$result`, если с ней могут происходить изменения в середине работы метода. В любом месте в методе должно быть понятно, где вы оперируете результатом, а где локальными переменными.
```php
/* Плохо */

function loadUsers(): array {
    $users = [];
    // ... много кода, изменяющего переменную $users
    return $users;
}

/* Хорошо */

function loadUsers(): array {
    $result = [];
    // ... много кода, изменяющего переменную $result
    return $result;
}
```
### 📖 Метод должен явно отличать нормальные ситуации от исключительных
Если никакой ошибки не произошло, но отсутствует результат, то это `null` (или пустой массив), однако если все же произошла исключительная ситуация, которая не заложена системой, то должно кидаться исключение.
```php
/* Плохо */

function loadUsers(): array {
    if ($connectionError !== null) {
        return []; // потеряли ошибку, никто не узнает о проблемах с подключением
    }
    // ...
    if (count($data) === 0) {
        return [];
    }
    // ...
    return $result;
}

/* Хорошо */

function loadUsers(): array {
    if ($connectionError !== null) {
        throw new Exception\ConnectionError();
    }
    // ...
    if (count($data) === 0) {
        return [];
    }
    // ...
    return $result;
}
```
### 📖 Метод должен придерживаться следующей структуры: Проверка параметров → Получение данных → Работа → Результат
Во время проверки параметров и получения необходимых данных метод должен возвращать соответствующее пустое значение или кидать исключение. После того как метод получил все необходимые данные и приступил к работе выход из метода крайне нежелателен. Возможны редкие исключения, облегчающие понимание и читаемость кода.
```php
/* Плохо */

public function someMethod(): int {
    $isValid = $this->_someCheck();
    if ($isValid) {
        $tmp = 0;
        $someValue = $this->_getSomeValue();
        if ($someValue > 0) {
            $tmp = $someValue;
        }
        $anotherValue = $this->_getAnotherValue();
        if ($anotherValue > 0) {
            return $tmp + $anotherValue;
        } else {
            return $someValue;
        }
    } else {
        throw new \Exception('Invalid condition');
    }
}

/* Хорошо */

/**
 * @throws \Exception
 */
public function someMethod(): int {
    $result = 0;
     
    $isValid = $this->_someCheck();
    if (!$isValid) {
        throw new \Exception('Invalid condition');
    }
  
    $someValue = $this->_getSomeValue();
    if ($someValue > 0) {
        $result += $someValue;
    }
    $anotherValue = $this->_getAnotherValue();
    if ($anotherValue > 0) {
        $result += $anotherValue;
    }
    return $result;
}
```
***
## **Работа с классами**
### 📖 Трейты имеют постфикс Trait
```php
/* Хорошо */

trait AjaxResponseTrait {
    // ...
}
```
### 📖 Интерфейсы имеют постфикс Interface
```php
/* Хорошо */

interface ApplicationInterface {
    // ...
}
```
### 📖 Абстрактные классы имеют префикс Abstract
```php
/* Хорошо */

abstract class AbstractApplication {
    // ...
}
```
### 📖 Все свойства и константы класса по умолчанию должны быть private
Если свойство используется наследниками класса, то оно объявляется `protected`. Если используется сторонними классами, тогда `public`.
```php
/* Плохо */

abstract class Loader {
    public $data = [];

    public function getData() {
        return $this->data;
    }

    public function init() {
        $this->data = $this->load();
    }

    abstract public function load();
}

/* Хорошо */

abstract class Loader {
    
    /**
     * @type array
     */
    private $_cachedData = [];

    public function getData(): array {
        return $this->_cachedData;
    }

    public function init(): void {
        $this->_cachedData = $this->_load();
    }

    abstract protected function _load(): array;
}
```
### 📖 Методы и свойства в классе должны быть отсортированы по уровням видимости и по порядку использования сверху вниз
Уровни видимости: `public` -> `protected` -> `private`.
```php
/* Плохо */

class SomeClass {
    private $_privPropA;   
    public $pubPropA;
    protected $_protPropA;
 
    protected function _protA() {
    }
 
 
    public function pubB() {
    }
 
 
    private function _privA() {
        return $this->_protA();
    }
  
    public function pubA() {
        $this->_privA();
        return $this->pubB();
    }
}

/* Хорошо */

class SomeClass {
    public $pubPropA;
    protected $_protPropA;
    private $_privPropA;
 
    public function pubA() {
        $this->_privA();
        return $this->pubB();
    }
 
    public function pubB() {
    }
 
    protected function _protA() {
    }
 
    private function _privA() {
        return $this->_protA();
    }
}
```
***
## Работа с объектами
### 📖 Все объекты должны быть неизменяемыми (immutable), если от них не требуется обратного
```php
/* Плохо */

class SomeObject {
    /**
     * @var int
     */
    public $id;
}

/* Хорошо */

class SomeObject {
    /**
     * @var int
     */ 
    private $_id;
  
    public function __construct(int $id) {
        $this->_id = $id;
    }
  
    public function id(): int {
        return $this->_id;
    }
}
```
### 📖 Статические вызовы можно делать только у самого класса. У экземпляра можно обращаться только к его свойствам и методам
```php
/* Плохо */

$type = $user::TYPE;

/* Хорошо */

$type = User::TYPE;
```
***
## Комментирование кода
### 📖 В общем случае комментарии запрещены
Желание добавить комментарий — признак плохо читаемого кода. Любой участок кода, который вы хотели бы выделить или прокомментировать, надо выносить в отдельный метод.
Фразу, которую вы хотели написать в комментарии, надо привести в простой вид и сделать ее названием метода.
```php
/* Плохо */

public function deleteApprovedUsers() {
    // load users filter them by approval
    $users = $repository->loadUsers();
    array_filter($users, function($user) {
        return $user->is_approved;
    });

    foreach ($users as $user) {
        // ...
    }
}

/* Хорошо */

public function deleteApprovedUsers() {
    $users = $this->loadApprovedUsers();
    foreach ($users as $user) {
        // ...
    }
}

public function loadApprovedUsers(): array {
    $users = $repository->loadUsers();
    array_filter($users, function($user) {
        return $user->is_approved;
    });
}
```
### 📖 Вынужденные хаки должны быть помечены комментариями
Лучше соблюдать одинаковый формат в рамках проекта
```php
/* Хорошо */

function loadUsers(): array {
    $result = $repository->loadUsers();
    // hack: status field was removed from storage 
    foreach ($result as $user) {
        $user->status = 'active';
    }
    // hack end
    return $result;
}
```
### 📖 Готовые алгоритмы, взятые из внешнего источника, должны быть помечены ссылкой на источник
```php
/* Хорошо */

/**
 * https://en.wikipedia.org/wiki/Quicksort
 */
function quickSort(array $arr): array {
    // ...
}

/**
 * https://habrahabr.ru/post/320140/
 */
function generateRandomMaze() {
    // ...
}
```
### 📖 При разработке прототипа допустимо помечать участки кода `@todo`
```php
/* Хорошо */

function loadUsers(): array {
    $result = $repository->loadUsers();
    // @todo: delete the hack when field will be restored
    // hack: status field was removed from storage
    foreach ($result as $user) {
        $user->status = 'active';
    }
    // hack end
    return $result;
}
```
***
## Работа с исключениями
### 📖 На каждом уровне бизнес-логики (проект, компонент, библиотека) должно быть абстрактное базовое исключение
### 📖 Исключения сторонних библиотек должны быть перехвачены сразу
Далее либо обработаны, либо на их основании должно бросаться свое исключение. Новое исключение должно содержать предыдущее.
```php
/* Хорошо */

namespace Service\Facebook;

use Exception;
use FacebookAds;

public function requestData() {
    // ...
    try {
        $objects = $facebookAds->requestData($params);
    } catch (FacebookAds\Exception\Exception $e) {
        throw new Exception\ExternalServiceError("Facebook error", 0, $e);
    }
    //..
}
```
### 📖 По умолчанию тексты исключений не должны показываться пользователю
Они предназначены для логирования и отладки. Текст исключения можно показать пользователю, если оно явно для этого предназначено: например, реализует интерфейс `HumanReadableInterface`.
```php
/* Хорошо */

interface HumanReadableInterface {
    
    public function getUserMessage(): string;
}

public function handleException(\Throwable $exception): void {
    if ($exception instanceof HumanReadableInterface) {
        echo $exception->getUserMessage();
        return;
    }
    // ...
}
```
***
## Работа с внешним хранилищем данных
### 📖 Нельзя делать запросы к внешнему хранилищу внутри цикла с заведомо большим кол-вом итераций
```php
/* Плохо */

$users = loadUsers();
foreach ($users as $user) {
    $userProjects = loadUserProjects($user);
    // ...
}

/* Хорошо */

$users = loadUsers();
$projects = loadProjects();
$indexedProjects = [];
foreach ($projects as $project) {
    if (!array_key_exists($project->user_id, $indexedProjects)) {
        $indexedProjects[$project->user_id] = [];
    }
    $indexedProjects[$project->user_id][] = $project;
}
foreach ($users as $user) {
    if (!array_key_exists($user->id, $indexedProjects)) {
        continue;
    }
    $userProjects = $indexedProjects[$user->id];
}
```
### 📖 Для каждой записи в хранилище должно быть понятна дата ее создания
То есть должна быть колонка `date/creation_date`. Или должен быть зависимый объект (связь 1 к 1), у которого есть такая колонка. Редактируемые записи должны иметь и дату редактирования: `update_date` или `modification_date`.
***
## Особенности Pull Request (PR)
### 📖 PR должен содержать как можно меньше строк кода
Любая атомарная часть кода должна выделяться в отдельную подзадачу и отдельный PR.
### 📖 Нельзя смешивать перенос методов в другие классы и места и последующий рефакторинг между собой
Перенос методов в другие классы и места должны быть выделены в отдельный PR. Последующий рефакторинг после переноса тоже должен быть в отдельном PR.
### 📖 В случае большого PR — ответственность за долгий просмотр несет сам разработчик, сделавший такой PR
Нормальный объем кода — 1-300 строк в зависимости от его сложности. PR заглушек и архитектуры может содержать много формального кода, который легко быстро проверить. PR же конкретного метода может содержать много сложностей даже в 10 строчках.
### 📖 Нельзя накапливать изменения в какой-то своей ветке и потом делать большой PR в master
Все что можно смержить в master без последствий (даже если это еще не готовый результат, а только заглушки или часть, но они скрыты от юзеров и никому не мешают), должен мержиться в master и PR должен создаваться в master.
### 📖 В Pull Request не должно попадать кода, не относящегося к задаче
Также не должно быть забытых комментариев, бессмысленных переносов строк и прочего "строительного мусора". Каждое изменение, которое вы предлагаете сделать в master-ветке, должно так или иначе относиться к решению поставленной вам задачи.
***
## **Работа с шаблонами**
### 📖 В шаблонах не должны вызываться методы объектов (геттеры не в счет)
Все необходимые данные должны быть загружены до рендера и переданы в виде параметров шаблона.
***
## Работа с литералами
### 📖 Назначение всех числовых литералов должно быть понятным из контекста
Они должны быть или вынесены в переменную или константу, или сравниваться с переменной, или передаваться на вход методу с понятной сигнатурой. В коде должен присутствовать в явном виде ответ: `за что отвечает это число и почему оно именно такое?`
```php
/* Плохо */

$isOnlyDeleted = 1;
if ($object->is_deleted === $isOnlyDeleted) {
    // ...
}
 
for ($i = 0; $i < 5; $i++) {
    // ...
}

/* Хорошо */

if ($object->is_deleted === 1) {
    // ...
}
 
$apiMaxRetryLimit = 5;
for ($i = 0; $i < $apiMaxRetryLimit; $i++) {
    // ...
} 
```
***
## Работа с условиями
### 📖 В условном операторе должно проверяться исключительно `boolean` значение
```php
/* Плохо */

if (count($userProjects)) {
    // ...
}
if ($project) {
    // ...
}

/* Хорошо */

if ($isResponseError) { // $isResponseError = true
    // ...
}
if ($response->isError()) { // isError method returns boolean
    // ...
}
if (count($userProjects) > 0) {
    // ...
}
```
### 📖 В сравнении не boolean переменных используется строгое сравнение с приведением типа (`===`), автоматическое приведение и нестрогое сравнение не используются
```php
/* Плохо */

if ($project) {
    // ...
}
if ($request->postData('sum') == 100) {
    // ...
}
if (!$request->postData('sum')) {
    // ...
}
if (!$bill->comment) {
    // ...
}

/* Хорошо */

if ($project === null) { // $project is an object
    // ...
}
if ((int)$request->postData('sum') === 100) {
    // ...
}
if ($bill->comment === '') {
    // ...
}
```
### 📖 Автоматическое приведение типов разрешено только, когда один из операндов — литерал с фиксированным типом
При сравнении двух переменных с неизвестными типами для читающего код человека не очевидно, к чему они будут приведены интерпретатором. Если же тип одного из операндов известен, то всё становится очевидно и ручное приведение типов не требуется.
Если вы хотите проверить значение `boolean` пришедшее извне, то делается это так:

```php
/* Плохо */

if ((int)$request->get('is_something') > 0) {
    // ...
}
if ((int)$request->get('is_something') === 1) {
    // ...
}
if ((int)$user->is_registered === 0) {
    // ...
}

/* Хорошо */

if ($request->get('is_something') > 0) {
    // ...
}
if ($user->is_registered) {
    // ...
}
if (!$user->is_registered) {
    // ...
}
```
#### 📖 Не надо сравнивать `boolean` с `true`/`false`
Это нарушает запрет на бесполезный код.
```php
/* Плохо */

if ($bill->isPaid() == true) {
    // ...
}
if ($bill->isPaid() !== false) {
    // ...
}
if (!$bill->isPaid() === true) {
    // ...
}
if (!(!$bill->isPaid() === true)) {
    // ...
}
if ((bool)$phone->is_external === true) {
    // ...
}

/* Хорошо */

if ($bill->isPaid()) {
    // ...
}
```
### 📖 Проверять переменные надо на наличие позитивного вхождения, а не отсутствие негативного
Если вам нужна строка, то проверять надо на то, что переменная является строкой. Не надо проверять на то, что она не является числом или чем-то еще. Перечислять все возможные варианты, чем переменная не должна быть, значит повышать риск ошибки и усложнять поддержку кода.
```php
/* Плохо */

if (!is_numeric($value) && !is_object($value)) {
    // ...
}

/* Хорошо */

if (is_string($value) && $value !== '') {
    // ...
}
```
### 📖 Если вы используете встроенную функцию PHP, которая возвращает `0`, `1` и, возможно, `false`, то при возможности результат ее работы используем в условии как `bool` без дополнительных сравнений
Это не касается случая, когда вам нужно отделить два разных результата между собой, например отдельно отработать `0` и `false`.
```php
/* Плохо */

if (preg_match($pattern, $subject) === 1) {
    // ...
}
if (!strpos($search, $text)) {
    // ...
}

/* Хорошо */

if (preg_match($pattern, $subject)) { 
    // handle success
}
if (!preg_match($pattern, $subject)) {
    // handle not success
}
if (preg_match($pattern, $subject) === false) {
    // handle error
}
if (strpos($search, $text) === false) {
    // handle not success
}
```
### 📖 При использовании в условном выражении одновременно операторов И и ИЛИ обязательно выделять приоритет скобками
Обратите внимание на различие в значении двух вариантов правильного использования
```php
/* Плохо */

if ($isMobile || $isSizeTooBig && $isAllowedToShrink) {
    // ...
}

/* Хорошо */

if (($isMobile || $isSizeTooBig) && $isAllowedToShrink) {
    // ...
}
if ($isMobile || ($isSizeTooBig && $isAllowedToShrink)) {
    // ...
}
```
***
## Работа с тернарными операторами
### 📖 При использовании тернарных операторов действуют те же правила, что и при использовании условий
### 📖 Тернарный оператор следует использовать, если обе ветви условия предназначены для установки одной переменной одним языковым выражением
При наличии логики в ветках условия следует рассмотреть возможность вынести ее в отдельный метод.
```php
/* Плохо */

if ($isExternal) {
    $bill = $this->loadExternalBill();
} else {
    $bill = $this->loadInternalBill();
}

/* Хорошо */

$bill = $isExternal ? $this->loadExternalBill() : $this->loadInternalBill();
```
### 📖 Использовать цепочки из тернарных операторов `?:` допустимо только при указании значения по умолчанию
```php
/* Плохо */

$contact = $this->loadContactByPhone() ?: $this->loadContactByEmail() ?: $this->loadContactByName();

/* Хорошо */

$lead = $this->loadLeadFromCache() ?: $this->loadLeadFromDB();
$contact = $this->loadContactByPhone() ?: $this->loadContactByEmail() ?: $this->loadContactByName() ?: null;
```
***
## Про тесты
### 📖 Тесты являются таким же production-кодом, как и любой другой код
Они должны быть написаны с соблюдением соглашений, описанных в этом документе.
### 📖 В дата провайдерах для тестов надо писать комментарий или ассоциативный массив к структуре отдаваемого массива значений
```php
/* Плохо */

public function isEmailAddressData(): array {
    return [
        ['test@test.ru',            true ],
        // ...
    ]
}

/* Хорошо */

public function isEmailAddressData(): array {
    return [
        //    email               isValid
        ['test@test.ru',            true],
        ['@test.ru',                false],
        ['invalidEmail',            false],
        // ...
    ]
}

/* Или */

public function isEmailAddressData(): array {
    return [
        'valid' =>          ['email' => 'test@test.ru', 'isValid' => true],
        'invalid with @' => ['email' => '@test.ru',     'isValid' => false],
        'invalid'        => ['email' => 'invalidEmail', 'isValid' => false],
        // ...
    ]
}

/* Или */

public function isEmailAddressData(): \Generator {
    yield 'valid' => ['email' => 'test@test.ru', 'isValid' => true];
    yield 'invalid with @' => ['email' => '@test.ru',     'isValid' => false];
}
```
***
## Использование chain-объектов
### 📖 Метод с большим количеством необязательных параметров (А) может быть заменен chain-объектом
Метод с большим количеством необязательных параметров (А) может быть заменен chain-объектом. В объекте конструктор принимает все обязательные параметры, а все необязательные реализуются сеттерами без глагола set (только существительное), возвращающими текущий объект (chaining методов). Метод-глагол у объекта один без параметров, он завершает использование объекта и выполняет действие, которое должен был выполнить метод А.

```php
/* Старый метод */

function send($method, $url, $body = null, $headers = null, $retries = 1, $timeout = 300) {}

/* Должен замениться на chain-объект */

public function __construct($method, $url) {
    // ...
}

public function body($body) {
    return $this;
}
// остальные методы с необязательными параметрами

public function send();

/* Новый объект используется так */

new $sender($method, $url)->body($body)->retries(10)->timeout(25)->send();
```
***
## Работа со скриптами
### 📖 Любой скрипт, который изменяет данные, должен иметь подтверждение перед выполнением действий с данными и `debug` по результатам работы
```php
/* Плохо */

// cli/delete_items.php
$repository->deleteItems();

/* Исправим, чтобы случайный запуск не удалил элементы */

/* Хорошо */

// cli/delete_items.php
$totalItems = $repository->countItems();
if (!confirm("Do you want to delete {$totalItems} item(s)?")) {
    echo 'Delete canceled, exit';
    exit(1);
}

$repository->deleteItems();

function confirm(string $question): bool {
    return readline("{$question} [y/n]: ") === 'y'
}
```
***
