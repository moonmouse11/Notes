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

[](https://github.com/roistat/php-code-conventions?tab=readme-ov-file#-%D0%B4%D0%BB%D1%8F-%D0%BA%D0%BE%D0%BD%D0%BA%D0%B0%D1%82%D0%B5%D0%BD%D0%B0%D1%86%D0%B8%D0%B8-%D0%BC%D0%B0%D1%81%D1%81%D0%B8%D0%B2%D0%BE%D0%B2-%D0%B7%D0%B0%D0%BF%D1%80%D0%B5%D1%89%D0%B5%D0%BD%D0%BE-%D0%B8%D1%81%D0%BF%D0%BE%D0%BB%D1%8C%D0%B7%D0%BE%D0%B2%D0%B0%D1%82%D1%8C-%D0%BE%D0%BF%D0%B5%D1%80%D0%B0%D1%82%D0%BE%D1%80-)

Обратите внимание, что `array_merge` все числовые ключи приводит к `int`, даже если они записаны строкой.

Плохо:

```html
return $initialData + $loadedData;
```

Хорошо:

```html
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

[](https://github.com/roistat/php-code-conventions?tab=readme-ov-file#-%D0%B4%D0%BB%D1%8F-%D0%BF%D1%80%D0%BE%D0%B2%D0%B5%D1%80%D0%BA%D0%B8-%D0%BD%D0%B0%D0%BB%D0%B8%D1%87%D0%B8%D1%8F-%D0%BA%D0%BB%D1%8E%D1%87%D0%B0-%D0%B2-%D0%B0%D1%81%D1%81%D0%BE%D1%86%D0%B8%D0%B0%D1%82%D0%B8%D0%B2%D0%BD%D0%BE%D0%BC-%D0%BC%D0%B0%D1%81%D1%81%D0%B8%D0%B2%D0%B5-%D0%B8%D1%81%D0%BF%D0%BE%D0%BB%D1%8C%D0%B7%D1%83%D0%B5%D0%BC-array_key_exists-%D0%B0-%D0%BD%D0%B5-isset)

`isset` проверяет не ключ на его наличие, а значение этого ключа, если он есть. Это разные методы с разным поведением и назначением. Если вы хотите проверить значение ключа, то делайте это явно. Сначала явно проверьте наличие ключа через `array_key_exists` и обработайте ситуацию его отсутствия, затем приступайте к работе со значением.

Плохо:

```html
function processRequestData(array $requestData) {
    $data = [];
    if (isset($requestData['project_key'])) {
    	// ...
    }
    return $data;
}
```

Хорошо:

```html
function processRequestData(array $requestData) {
    $data = [];
    if (array_key_exists('project_key', $requestData)) {
    	// ...
    }
    return $data;
}
```

Допустимо использовать сокращенный вариант `??`, с явным указанием дефолтного значения.

```html
function getProjectKey(array $requestData) {
    return $requestData['project_key'] ?? null;
}
```

### 📖 Ассоциативный массив мы используем как hashmap

[](https://github.com/roistat/php-code-conventions?tab=readme-ov-file#-%D0%B0%D1%81%D1%81%D0%BE%D1%86%D0%B8%D0%B0%D1%82%D0%B8%D0%B2%D0%BD%D1%8B%D0%B9-%D0%BC%D0%B0%D1%81%D1%81%D0%B8%D0%B2-%D0%BC%D1%8B-%D0%B8%D1%81%D0%BF%D0%BE%D0%BB%D1%8C%D0%B7%D1%83%D0%B5%D0%BC-%D0%BA%D0%B0%D0%BA-hashmap)

То есть не применяем разные встроенные в PHP инструменты. Приведем несколько очевидных примеров (однако, правило ими не исчерпывается):

#### 📖 Нельзя сортировать ассоциативные массивы

[](https://github.com/roistat/php-code-conventions?tab=readme-ov-file#-%D0%BD%D0%B5%D0%BB%D1%8C%D0%B7%D1%8F-%D1%81%D0%BE%D1%80%D1%82%D0%B8%D1%80%D0%BE%D0%B2%D0%B0%D1%82%D1%8C-%D0%B0%D1%81%D1%81%D0%BE%D1%86%D0%B8%D0%B0%D1%82%D0%B8%D0%B2%D0%BD%D1%8B%D0%B5-%D0%BC%D0%B0%D1%81%D1%81%D0%B8%D0%B2%D1%8B)

Плохо:

```html
$arr = [
    'project_key' => 'foo',
    'key' => 'bar',
    'user_id' => 300,
];

uasort($arr);
```

#### 📖 Нельзя смешивать в массиве строковые и числовые ключи

[](https://github.com/roistat/php-code-conventions?tab=readme-ov-file#-%D0%BD%D0%B5%D0%BB%D1%8C%D0%B7%D1%8F-%D1%81%D0%BC%D0%B5%D1%88%D0%B8%D0%B2%D0%B0%D1%82%D1%8C-%D0%B2-%D0%BC%D0%B0%D1%81%D1%81%D0%B8%D0%B2%D0%B5-%D1%81%D1%82%D1%80%D0%BE%D0%BA%D0%BE%D0%B2%D1%8B%D0%B5-%D0%B8-%D1%87%D0%B8%D1%81%D0%BB%D0%BE%D0%B2%D1%8B%D0%B5-%D0%BA%D0%BB%D1%8E%D1%87%D0%B8)

Плохо:

```html
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

[](https://github.com/roistat/php-code-conventions?tab=readme-ov-file#-%D0%B4%D0%BB%D1%8F-%D0%BF%D1%80%D0%BE%D0%B2%D0%B5%D1%80%D0%BA%D0%B8-%D0%BD%D0%B0%D0%BB%D0%B8%D1%87%D0%B8%D1%8F-%D0%B7%D0%BD%D0%B0%D1%87%D0%B5%D0%BD%D0%B8%D1%8F-%D0%BF%D0%BE-%D0%B8%D0%BD%D0%B4%D0%B5%D0%BA%D1%81%D1%83-%D0%B2-%D0%BE%D0%B1%D1%8B%D1%87%D0%BD%D1%8B%D1%85-%D0%BD%D0%B5-%D0%B0%D1%81%D1%81%D0%BE%D1%86%D0%B8%D0%B0%D1%82%D0%B8%D0%B2%D0%BD%D1%8B%D1%85-%D0%BC%D0%B0%D1%81%D1%81%D0%B8%D0%B2%D0%B0%D1%85-%D0%B8%D1%81%D0%BF%D0%BE%D0%BB%D1%8C%D0%B7%D1%83%D0%B5%D0%BC-countarray--n)

Плохо:

```html
if (array_key_exists(1, $users)) {
    // ...
}
if (isset($users[1])) {
    // ...
}
```

Хорошо:

```html
if (count($users) > 1) {
   // ... 
}
```

**[⬆ наверх](https://github.com/roistat/php-code-conventions?tab=readme-ov-file#%D0%A1%D0%BE%D0%B4%D0%B5%D1%80%D0%B6%D0%B0%D0%BD%D0%B8%D0%B5)**

## **Работа со строками**

[](https://github.com/roistat/php-code-conventions?tab=readme-ov-file#%D1%80%D0%B0%D0%B1%D0%BE%D1%82%D0%B0-%D1%81%D0%BE-%D1%81%D1%82%D1%80%D0%BE%D0%BA%D0%B0%D0%BC%D0%B8)

### 📖 Строки обрамляются одинарными кавычками

[](https://github.com/roistat/php-code-conventions?tab=readme-ov-file#-%D1%81%D1%82%D1%80%D0%BE%D0%BA%D0%B8-%D0%BE%D0%B1%D1%80%D0%B0%D0%BC%D0%BB%D1%8F%D1%8E%D1%82%D1%81%D1%8F-%D0%BE%D0%B4%D0%B8%D0%BD%D0%B0%D1%80%D0%BD%D1%8B%D0%BC%D0%B8-%D0%BA%D0%B0%D0%B2%D1%8B%D1%87%D0%BA%D0%B0%D0%BC%D0%B8)

Двойные кавычки используются только, если:

- Внутри строки должны быть одинарные кавычки
- Внутри строки используется подстановка переменных
- Внутри строки используются спец. символы `\n`, `\r`, `\t` и т.д.

Плохо:

```html
$string = "Some string";
$string = 'Some \'string\'';
$string = "\t".'Some string'."\n";
```

Хорошо:

```html
$string = 'Some string';
$string = "Some 'string'";
$string = "\tSome string\n";
```

### 📖 Вместо лишней конкатенации используем подстановку переменных в двойных кавычках с помощью фигурных скобок

[](https://github.com/roistat/php-code-conventions?tab=readme-ov-file#-%D0%B2%D0%BC%D0%B5%D1%81%D1%82%D0%BE-%D0%BB%D0%B8%D1%88%D0%BD%D0%B5%D0%B9-%D0%BA%D0%BE%D0%BD%D0%BA%D0%B0%D1%82%D0%B5%D0%BD%D0%B0%D1%86%D0%B8%D0%B8-%D0%B8%D1%81%D0%BF%D0%BE%D0%BB%D1%8C%D0%B7%D1%83%D0%B5%D0%BC-%D0%BF%D0%BE%D0%B4%D1%81%D1%82%D0%B0%D0%BD%D0%BE%D0%B2%D0%BA%D1%83-%D0%BF%D0%B5%D1%80%D0%B5%D0%BC%D0%B5%D0%BD%D0%BD%D1%8B%D1%85-%D0%B2-%D0%B4%D0%B2%D0%BE%D0%B9%D0%BD%D1%8B%D1%85-%D0%BA%D0%B0%D0%B2%D1%8B%D1%87%D0%BA%D0%B0%D1%85-%D1%81-%D0%BF%D0%BE%D0%BC%D0%BE%D1%89%D1%8C%D1%8E-%D1%84%D0%B8%D0%B3%D1%83%D1%80%D0%BD%D1%8B%D1%85-%D1%81%D0%BA%D0%BE%D0%B1%D0%BE%D0%BA)

Плохо:

```html
$string = 'Object with type "' . $object->type() . '" has been removed';
```

Хорошо:

```html
$string = "Object with type \"{$object->type()}\" has been removed";
```

**[⬆ наверх](https://github.com/roistat/php-code-conventions?tab=readme-ov-file#%D0%A1%D0%BE%D0%B4%D0%B5%D1%80%D0%B6%D0%B0%D0%BD%D0%B8%D0%B5)**

## **Работа с датами**

[](https://github.com/roistat/php-code-conventions?tab=readme-ov-file#%D1%80%D0%B0%D0%B1%D0%BE%D1%82%D0%B0-%D1%81-%D0%B4%D0%B0%D1%82%D0%B0%D0%BC%D0%B8)

### 📖 Дата всегда должна быть представлена DateTime, интервал как DateInterval

[](https://github.com/roistat/php-code-conventions?tab=readme-ov-file#-%D0%B4%D0%B0%D1%82%D0%B0-%D0%B2%D1%81%D0%B5%D0%B3%D0%B4%D0%B0-%D0%B4%D0%BE%D0%BB%D0%B6%D0%BD%D0%B0-%D0%B1%D1%8B%D1%82%D1%8C-%D0%BF%D1%80%D0%B5%D0%B4%D1%81%D1%82%D0%B0%D0%B2%D0%BB%D0%B5%D0%BD%D0%B0-datetime-%D0%B8%D0%BD%D1%82%D0%B5%D1%80%D0%B2%D0%B0%D0%BB-%D0%BA%D0%B0%D0%BA-dateinterval)

Плохо:

```html
$date = $request->get('date');
$interval = 86400*30;
loadSomeData($date, $interval);
```

Хорошо:

```html
$date = $this->_dateService->instance($request->get('date'));
$interval = new \DateInterval('P30D');
loadSomeData($date, $interval);
```

### 📖 Запрещено создавать объект даты при помощи `new \DateTime()`

[](https://github.com/roistat/php-code-conventions?tab=readme-ov-file#-%D0%B7%D0%B0%D0%BF%D1%80%D0%B5%D1%89%D0%B5%D0%BD%D0%BE-%D1%81%D0%BE%D0%B7%D0%B4%D0%B0%D0%B2%D0%B0%D1%82%D1%8C-%D0%BE%D0%B1%D1%8A%D0%B5%D0%BA%D1%82-%D0%B4%D0%B0%D1%82%D1%8B-%D0%BF%D1%80%D0%B8-%D0%BF%D0%BE%D0%BC%D0%BE%D1%89%D0%B8-new-datetime)

В проекте для этого должен быть фабричный метод в сервисе для работы с датами.

Плохо:

```html
$date = new \DateTime();
```

Хорошо:

```html
$date = $this->_dateService->instance();
```

### 📖 Если дата должна быть представлена скалярным значением, необходимо использовать строку

[](https://github.com/roistat/php-code-conventions?tab=readme-ov-file#-%D0%B5%D1%81%D0%BB%D0%B8-%D0%B4%D0%B0%D1%82%D0%B0-%D0%B4%D0%BE%D0%BB%D0%B6%D0%BD%D0%B0-%D0%B1%D1%8B%D1%82%D1%8C-%D0%BF%D1%80%D0%B5%D0%B4%D1%81%D1%82%D0%B0%D0%B2%D0%BB%D0%B5%D0%BD%D0%B0-%D1%81%D0%BA%D0%B0%D0%BB%D1%8F%D1%80%D0%BD%D1%8B%D0%BC-%D0%B7%D0%BD%D0%B0%D1%87%D0%B5%D0%BD%D0%B8%D0%B5%D0%BC-%D0%BD%D0%B5%D0%BE%D0%B1%D1%85%D0%BE%D0%B4%D0%B8%D0%BC%D0%BE-%D0%B8%D1%81%D0%BF%D0%BE%D0%BB%D1%8C%D0%B7%D0%BE%D0%B2%D0%B0%D1%82%D1%8C-%D1%81%D1%82%D1%80%D0%BE%D0%BA%D1%83)

- строка с датой и временем должна быть везде в одинаковом формате
- формат не должен включать временную зону, если для этого нет особых требований
- при прочих равных в дате без часового пояса всегда подразумевается UTC0
- если строку по какой-то причине невозможно использовать, используем `int`

Плохо:

```html
class User {
    public $creation_time;
}

$user->creation_time = time();
```

Хорошо:

```html
class User {
    /**
     * @type string
     */
    public $creation_date;
}

$user->creation_date = '2018-01-18 12:54:11';
```

### 📖 При работе с интервалами/периодами запрещено указывать месяц или год

[](https://github.com/roistat/php-code-conventions?tab=readme-ov-file#-%D0%BF%D1%80%D0%B8-%D1%80%D0%B0%D0%B1%D0%BE%D1%82%D0%B5-%D1%81-%D0%B8%D0%BD%D1%82%D0%B5%D1%80%D0%B2%D0%B0%D0%BB%D0%B0%D0%BC%D0%B8%D0%BF%D0%B5%D1%80%D0%B8%D0%BE%D0%B4%D0%B0%D0%BC%D0%B8-%D0%B7%D0%B0%D0%BF%D1%80%D0%B5%D1%89%D0%B5%D0%BD%D0%BE-%D1%83%D0%BA%D0%B0%D0%B7%D1%8B%D0%B2%D0%B0%D1%82%D1%8C-%D0%BC%D0%B5%D1%81%D1%8F%D1%86-%D0%B8%D0%BB%D0%B8-%D0%B3%D0%BE%D0%B4)

В зависимости от текущей даты месяц и год могут принимать разные временные промежутки (високосный и обычный год, разное количество дней в месяце). Вместо этого в качестве указания интервала используем дни, часы, минуты, секунды.

Плохо:

```html
$dateTime = new \DateTime('-2 month');
$dateInterval = new \DateInterval('P2M');
```

Хорошо:

```html
$dateTime = new $this->_dateTime->instance('-60 days');
$dateInterval = new \DateInterval('P60D');
```

Месяц или год необходимо использовать, если это напрямую указано в требованиях задачи как календарный месяц или календарный год.

**[⬆ наверх](https://github.com/roistat/php-code-conventions?tab=readme-ov-file#%D0%A1%D0%BE%D0%B4%D0%B5%D1%80%D0%B6%D0%B0%D0%BD%D0%B8%D0%B5)**

## **Работа с пространствами имён**

[](https://github.com/roistat/php-code-conventions?tab=readme-ov-file#%D1%80%D0%B0%D0%B1%D0%BE%D1%82%D0%B0-%D1%81-%D0%BF%D1%80%D0%BE%D1%81%D1%82%D1%80%D0%B0%D0%BD%D1%81%D1%82%D0%B2%D0%B0%D0%BC%D0%B8-%D0%B8%D0%BC%D1%91%D0%BD)

### 📖 Все пространства имён должны быть подключены через `use` в начале файла. В самом коде не должно быть обратного слеша перед названием пространства имён

[](https://github.com/roistat/php-code-conventions?tab=readme-ov-file#-%D0%B2%D1%81%D0%B5-%D0%BF%D1%80%D0%BE%D1%81%D1%82%D1%80%D0%B0%D0%BD%D1%81%D1%82%D0%B2%D0%B0-%D0%B8%D0%BC%D1%91%D0%BD-%D0%B4%D0%BE%D0%BB%D0%B6%D0%BD%D1%8B-%D0%B1%D1%8B%D1%82%D1%8C-%D0%BF%D0%BE%D0%B4%D0%BA%D0%BB%D1%8E%D1%87%D0%B5%D0%BD%D1%8B-%D1%87%D0%B5%D1%80%D0%B5%D0%B7-use-%D0%B2-%D0%BD%D0%B0%D1%87%D0%B0%D0%BB%D0%B5-%D1%84%D0%B0%D0%B9%D0%BB%D0%B0-%D0%B2-%D1%81%D0%B0%D0%BC%D0%BE%D0%BC-%D0%BA%D0%BE%D0%B4%D0%B5-%D0%BD%D0%B5-%D0%B4%D0%BE%D0%BB%D0%B6%D0%BD%D0%BE-%D0%B1%D1%8B%D1%82%D1%8C-%D0%BE%D0%B1%D1%80%D0%B0%D1%82%D0%BD%D0%BE%D0%B3%D0%BE-%D1%81%D0%BB%D0%B5%D1%88%D0%B0-%D0%BF%D0%B5%D1%80%D0%B5%D0%B4-%D0%BD%D0%B0%D0%B7%D0%B2%D0%B0%D0%BD%D0%B8%D0%B5%D0%BC-%D0%BF%D1%80%D0%BE%D1%81%D1%82%D1%80%D0%B0%D0%BD%D1%81%D1%82%D0%B2%D0%B0-%D0%B8%D0%BC%D1%91%D0%BD)

Плохо:

```html
$object = new \Some\Object();
```

Хорошо:

```html
use Some;
$object = new Some\Object();
```

### 📖 В свою очередь обычные классы без пространства имён не должны быть подключены через `use`

[](https://github.com/roistat/php-code-conventions?tab=readme-ov-file#-%D0%B2-%D1%81%D0%B2%D0%BE%D1%8E-%D0%BE%D1%87%D0%B5%D1%80%D0%B5%D0%B4%D1%8C-%D0%BE%D0%B1%D1%8B%D1%87%D0%BD%D1%8B%D0%B5-%D0%BA%D0%BB%D0%B0%D1%81%D1%81%D1%8B-%D0%B1%D0%B5%D0%B7-%D0%BF%D1%80%D0%BE%D1%81%D1%82%D1%80%D0%B0%D0%BD%D1%81%D1%82%D0%B2%D0%B0-%D0%B8%D0%BC%D1%91%D0%BD-%D0%BD%D0%B5-%D0%B4%D0%BE%D0%BB%D0%B6%D0%BD%D1%8B-%D0%B1%D1%8B%D1%82%D1%8C-%D0%BF%D0%BE%D0%B4%D0%BA%D0%BB%D1%8E%D1%87%D0%B5%D0%BD%D1%8B-%D1%87%D0%B5%D1%80%D0%B5%D0%B7-use)

Плохо:

```html
use TimeZone;
$date = new TimeZone('Europe\Moscow');
```

Хорошо:

```html
$date = new \TimeZone('Europe\Moscow');
```

### 📖 Нельзя подключать несколько классов из одного пространства имён через `use`

[](https://github.com/roistat/php-code-conventions?tab=readme-ov-file#-%D0%BD%D0%B5%D0%BB%D1%8C%D0%B7%D1%8F-%D0%BF%D0%BE%D0%B4%D0%BA%D0%BB%D1%8E%D1%87%D0%B0%D1%82%D1%8C-%D0%BD%D0%B5%D1%81%D0%BA%D0%BE%D0%BB%D1%8C%D0%BA%D0%BE-%D0%BA%D0%BB%D0%B0%D1%81%D1%81%D0%BE%D0%B2-%D0%B8%D0%B7-%D0%BE%D0%B4%D0%BD%D0%BE%D0%B3%D0%BE-%D0%BF%D1%80%D0%BE%D1%81%D1%82%D1%80%D0%B0%D0%BD%D1%81%D1%82%D0%B2%D0%B0-%D0%B8%D0%BC%D1%91%D0%BD-%D1%87%D0%B5%D1%80%D0%B5%D0%B7-use)

Плохо:

```html
use Entity\User;
use Entity\Project;
 
$user = new User();
$project = new Project();
```

Хорошо:

```html
use Entity;
  
$user = new Entity\User();
$project = new Entity\Project();
```

### 📖 Следует избегать использования псевдонима (alias)

[](https://github.com/roistat/php-code-conventions?tab=readme-ov-file#-%D1%81%D0%BB%D0%B5%D0%B4%D1%83%D0%B5%D1%82-%D0%B8%D0%B7%D0%B1%D0%B5%D0%B3%D0%B0%D1%82%D1%8C-%D0%B8%D1%81%D0%BF%D0%BE%D0%BB%D1%8C%D0%B7%D0%BE%D0%B2%D0%B0%D0%BD%D0%B8%D1%8F-%D0%BF%D1%81%D0%B5%D0%B2%D0%B4%D0%BE%D0%BD%D0%B8%D0%BC%D0%B0-alias)

Они запутывают код и его понимание. Если у вас совпадают названия пространств имён, то, скорее всего, вы делаете что-то не так. Допустимо использовать псевдоним, если другое решение будет слишком сложным.

Плохо:

```html
use Component\User;
use Entity\User as UserEntity;

$user = new UserEntity();
```

Хорошо:

```html
use Component\User;
use Entity;

$user = new Entity\User();
```

**[⬆ наверх](https://github.com/roistat/php-code-conventions?tab=readme-ov-file#%D0%A1%D0%BE%D0%B4%D0%B5%D1%80%D0%B6%D0%B0%D0%BD%D0%B8%D0%B5)**

## **Работа с методами**

[](https://github.com/roistat/php-code-conventions?tab=readme-ov-file#%D1%80%D0%B0%D0%B1%D0%BE%D1%82%D0%B0-%D1%81-%D0%BC%D0%B5%D1%82%D0%BE%D0%B4%D0%B0%D0%BC%D0%B8)

### 📖 Должна быть использована максимально возможная типизация для вашей версии PHP. Все параметры и их типы должны быть описаны в объявлении метода либо в PHPDoc. Возвращаемое значение тоже.

[](https://github.com/roistat/php-code-conventions?tab=readme-ov-file#-%D0%B4%D0%BE%D0%BB%D0%B6%D0%BD%D0%B0-%D0%B1%D1%8B%D1%82%D1%8C-%D0%B8%D1%81%D0%BF%D0%BE%D0%BB%D1%8C%D0%B7%D0%BE%D0%B2%D0%B0%D0%BD%D0%B0-%D0%BC%D0%B0%D0%BA%D1%81%D0%B8%D0%BC%D0%B0%D0%BB%D1%8C%D0%BD%D0%BE-%D0%B2%D0%BE%D0%B7%D0%BC%D0%BE%D0%B6%D0%BD%D0%B0%D1%8F-%D1%82%D0%B8%D0%BF%D0%B8%D0%B7%D0%B0%D1%86%D0%B8%D1%8F-%D0%B4%D0%BB%D1%8F-%D0%B2%D0%B0%D1%88%D0%B5%D0%B9-%D0%B2%D0%B5%D1%80%D1%81%D0%B8%D0%B8-php-%D0%B2%D1%81%D0%B5-%D0%BF%D0%B0%D1%80%D0%B0%D0%BC%D0%B5%D1%82%D1%80%D1%8B-%D0%B8-%D0%B8%D1%85-%D1%82%D0%B8%D0%BF%D1%8B-%D0%B4%D0%BE%D0%BB%D0%B6%D0%BD%D1%8B-%D0%B1%D1%8B%D1%82%D1%8C-%D0%BE%D0%BF%D0%B8%D1%81%D0%B0%D0%BD%D1%8B-%D0%B2-%D0%BE%D0%B1%D1%8A%D1%8F%D0%B2%D0%BB%D0%B5%D0%BD%D0%B8%D0%B8-%D0%BC%D0%B5%D1%82%D0%BE%D0%B4%D0%B0-%D0%BB%D0%B8%D0%B1%D0%BE-%D0%B2-phpdoc-%D0%B2%D0%BE%D0%B7%D0%B2%D1%80%D0%B0%D1%89%D0%B0%D0%B5%D0%BC%D0%BE%D0%B5-%D0%B7%D0%BD%D0%B0%D1%87%D0%B5%D0%BD%D0%B8%D0%B5-%D1%82%D0%BE%D0%B6%D0%B5)

Плохо:

```html
/**
 * @param $id
 * @param $name
 * @param $tags
 * @return mixed
 */
function storeUser($id, $name, $tags = []) {
    // ...
}
```

Хорошо:

```html
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

### 📖 Все возможные типы должны быть определены в PHPDoc

[](https://github.com/roistat/php-code-conventions?tab=readme-ov-file#-%D0%B2%D1%81%D0%B5-%D0%B2%D0%BE%D0%B7%D0%BC%D0%BE%D0%B6%D0%BD%D1%8B%D0%B5-%D1%82%D0%B8%D0%BF%D1%8B-%D0%B4%D0%BE%D0%BB%D0%B6%D0%BD%D1%8B-%D0%B1%D1%8B%D1%82%D1%8C-%D0%BE%D0%BF%D1%80%D0%B5%D0%B4%D0%B5%D0%BB%D0%B5%D0%BD%D1%8B-%D0%B2-phpdoc)

Наибольшую пользу это приносит при работе с массивами:

Плохо:

```html
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
```

Хорошо:

```html
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

[](https://github.com/roistat/php-code-conventions?tab=readme-ov-file#-%D0%BD%D0%B0%D0%B7%D0%B2%D0%B0%D0%BD%D0%B8%D0%B5-%D0%BC%D0%B5%D1%82%D0%BE%D0%B4%D0%B0-%D0%B4%D0%BE%D0%BB%D0%B6%D0%BD%D0%BE-%D0%BD%D0%B0%D1%87%D0%B8%D0%BD%D0%B0%D1%82%D1%8C%D1%81%D1%8F-%D1%81-%D0%B3%D0%BB%D0%B0%D0%B3%D0%BE%D0%BB%D0%B0-%D0%B8-%D1%81%D0%BE%D0%BE%D1%82%D0%B2%D0%B5%D1%82%D1%81%D1%82%D0%B2%D0%BE%D0%B2%D0%B0%D1%82%D1%8C-%D0%BF%D1%80%D0%B0%D0%B2%D0%B8%D0%BB%D0%B0%D0%BC-%D0%B8%D0%BC%D0%B5%D0%BD%D0%BE%D0%B2%D0%B0%D0%BD%D0%B8%D1%8F-%D0%BF%D0%B5%D1%80%D0%B5%D0%BC%D0%B5%D0%BD%D0%BD%D1%8B%D1%85)

Плохо:

```html
public function items() {
    // ...
}
public function convertedDataObject(array $data) {
    // ...
}
```

Хорошо:

```html
public function loadItems() {
    // ...
}
public function convertDataToObject(array $data) {
    // ...
}
```

### 📖 Нельзя использовать глагол get в геттерах

[](https://github.com/roistat/php-code-conventions?tab=readme-ov-file#-%D0%BD%D0%B5%D0%BB%D1%8C%D0%B7%D1%8F-%D0%B8%D1%81%D0%BF%D0%BE%D0%BB%D1%8C%D0%B7%D0%BE%D0%B2%D0%B0%D1%82%D1%8C-%D0%B3%D0%BB%D0%B0%D0%B3%D0%BE%D0%BB-get-%D0%B2-%D0%B3%D0%B5%D1%82%D1%82%D0%B5%D1%80%D0%B0%D1%85)

Например, вместо `getDate()` следует писать `date()`. Геттер — метод, работающий только с полями своего объекта.

Плохо:

```html
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
```

Хорошо:

```html
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

[](https://github.com/roistat/php-code-conventions?tab=readme-ov-file#-%D0%BC%D0%B5%D1%82%D0%BE%D0%B4%D1%8B-%D0%BD%D0%B0%D0%B7%D0%B2%D0%B0%D0%BD%D0%B8%D1%8F-%D0%BA%D0%BE%D1%82%D0%BE%D1%80%D1%8B%D1%85-%D0%BD%D0%B0%D1%87%D0%B8%D0%BD%D0%B0%D1%8E%D1%82%D1%81%D1%8F-c-check-%D0%B8-validate-%D0%B4%D0%BE%D0%BB%D0%B6%D0%BD%D1%8B-%D0%B2%D1%8B%D0%B1%D1%80%D0%B0%D1%81%D1%8B%D0%B2%D0%B0%D1%82%D1%8C-%D0%B8%D1%81%D0%BA%D0%BB%D1%8E%D1%87%D0%B5%D0%BD%D0%B8%D1%8F-%D0%B8-%D0%BD%D0%B5-%D0%B2%D0%BE%D0%B7%D0%B2%D1%80%D0%B0%D1%89%D0%B0%D1%82%D1%8C-%D0%B7%D0%BD%D0%B0%D1%87%D0%B5%D0%BD%D0%B8%D1%8F)

Плохо:

```html
public function validateRequestData(array $requestData): bool {
    if (!array_key_exists('key', $requestData)) {
        return false;
    }
    // ...
    return true;
}
```

Хорошо:

```html
public function validateRequestData(array $requestData): void {
    if (!array_key_exists('key', $requestData)) {
        throw new ValidationError('Field "key" not found');
    }
    // ...
}
```

### 📖 Все методы класса по умолчанию должны быть private

[](https://github.com/roistat/php-code-conventions?tab=readme-ov-file#-%D0%B2%D1%81%D0%B5-%D0%BC%D0%B5%D1%82%D0%BE%D0%B4%D1%8B-%D0%BA%D0%BB%D0%B0%D1%81%D1%81%D0%B0-%D0%BF%D0%BE-%D1%83%D0%BC%D0%BE%D0%BB%D1%87%D0%B0%D0%BD%D0%B8%D1%8E-%D0%B4%D0%BE%D0%BB%D0%B6%D0%BD%D1%8B-%D0%B1%D1%8B%D1%82%D1%8C-private)

Если метод используется наследниками класса, то он объявляется `protected`. Если используется сторонними классами, тогда `public`.

### 📖 Использование рекурсий допускается только в исключительном случае

[](https://github.com/roistat/php-code-conventions?tab=readme-ov-file#-%D0%B8%D1%81%D0%BF%D0%BE%D0%BB%D1%8C%D0%B7%D0%BE%D0%B2%D0%B0%D0%BD%D0%B8%D0%B5-%D1%80%D0%B5%D0%BA%D1%83%D1%80%D1%81%D0%B8%D0%B9-%D0%B4%D0%BE%D0%BF%D1%83%D1%81%D0%BA%D0%B0%D0%B5%D1%82%D1%81%D1%8F-%D1%82%D0%BE%D0%BB%D1%8C%D0%BA%D0%BE-%D0%B2-%D0%B8%D1%81%D0%BA%D0%BB%D1%8E%D1%87%D0%B8%D1%82%D0%B5%D0%BB%D1%8C%D0%BD%D0%BE%D0%BC-%D1%81%D0%BB%D1%83%D1%87%D0%B0%D0%B5)

Если код без рекурсии будет очень сложен для написания и понимания и при этом рекурсия гарантированно не выйдет за ограничения стека вызовов.

### 📖 Запрещается кешировать данные в статических переменных метода

[](https://github.com/roistat/php-code-conventions?tab=readme-ov-file#-%D0%B7%D0%B0%D0%BF%D1%80%D0%B5%D1%89%D0%B0%D0%B5%D1%82%D1%81%D1%8F-%D0%BA%D0%B5%D1%88%D0%B8%D1%80%D0%BE%D0%B2%D0%B0%D1%82%D1%8C-%D0%B4%D0%B0%D0%BD%D0%BD%D1%8B%D0%B5-%D0%B2-%D1%81%D1%82%D0%B0%D1%82%D0%B8%D1%87%D0%B5%D1%81%D0%BA%D0%B8%D1%85-%D0%BF%D0%B5%D1%80%D0%B5%D0%BC%D0%B5%D0%BD%D0%BD%D1%8B%D1%85-%D0%BC%D0%B5%D1%82%D0%BE%D0%B4%D0%B0)

Для кеширование в памяти используем свойство объекта.

Плохо:

```html
public function loadData() {
    static $_cachedData;
    if ($_cachedData === null) {
        $_cachedData = [];
    }
    return $_cachedData;
}
```

Хорошо:

```html
private $_cachedData = [];

public function loadData() {
    if ($this->_cachedData === null) {
        $this->_cachedData = [];
    }
    return $this->_cachedData;
}
```

### 📖 Параметры в методах должны следовать в следующем порядке: обязательные → часто используемые → редко используемые

[](https://github.com/roistat/php-code-conventions?tab=readme-ov-file#-%D0%BF%D0%B0%D1%80%D0%B0%D0%BC%D0%B5%D1%82%D1%80%D1%8B-%D0%B2-%D0%BC%D0%B5%D1%82%D0%BE%D0%B4%D0%B0%D1%85-%D0%B4%D0%BE%D0%BB%D0%B6%D0%BD%D1%8B-%D1%81%D0%BB%D0%B5%D0%B4%D0%BE%D0%B2%D0%B0%D1%82%D1%8C-%D0%B2-%D1%81%D0%BB%D0%B5%D0%B4%D1%83%D1%8E%D1%89%D0%B5%D0%BC-%D0%BF%D0%BE%D1%80%D1%8F%D0%B4%D0%BA%D0%B5-%D0%BE%D0%B1%D1%8F%D0%B7%D0%B0%D1%82%D0%B5%D0%BB%D1%8C%D0%BD%D1%8B%D0%B5--%D1%87%D0%B0%D1%81%D1%82%D0%BE-%D0%B8%D1%81%D0%BF%D0%BE%D0%BB%D1%8C%D0%B7%D1%83%D0%B5%D0%BC%D1%8B%D0%B5--%D1%80%D0%B5%D0%B4%D0%BA%D0%BE-%D0%B8%D1%81%D0%BF%D0%BE%D0%BB%D1%8C%D0%B7%D1%83%D0%B5%D0%BC%D1%8B%D0%B5)

Нужно соблюдать читаемость при написании вызова.

Плохо:

```html
public function method($required, $practicallyUnused = 5, $often = [], $lessOften = null)
public function filter($value, $name, $operator) // ...$service->filter(15, "id", "=")
```

Хорошо:

```html
public function method($required, $often = [], $lessOften = null, $practicallyUnused = 5)
public function filter($name, $operator, $value) // ...$service->filter("id", "=", 15)
```

### 📖 Nullable параметры должны быть помечены `?`, даже если указано значение по умолчанию.

[](https://github.com/roistat/php-code-conventions?tab=readme-ov-file#-nullable-%D0%BF%D0%B0%D1%80%D0%B0%D0%BC%D0%B5%D1%82%D1%80%D1%8B-%D0%B4%D0%BE%D0%BB%D0%B6%D0%BD%D1%8B-%D0%B1%D1%8B%D1%82%D1%8C-%D0%BF%D0%BE%D0%BC%D0%B5%D1%87%D0%B5%D0%BD%D1%8B--%D0%B4%D0%B0%D0%B6%D0%B5-%D0%B5%D1%81%D0%BB%D0%B8-%D1%83%D0%BA%D0%B0%D0%B7%D0%B0%D0%BD%D0%BE-%D0%B7%D0%BD%D0%B0%D1%87%D0%B5%D0%BD%D0%B8%D0%B5-%D0%BF%D0%BE-%D1%83%D0%BC%D0%BE%D0%BB%D1%87%D0%B0%D0%BD%D0%B8%D1%8E)

Плохо:

```html
function f(int $number = null) {}
```

Хорошо:

```html
function f(?int $number = null) {}
function f(?int $number) {}
```

**[⬆ наверх](https://github.com/roistat/php-code-conventions?tab=readme-ov-file#%D0%A1%D0%BE%D0%B4%D0%B5%D1%80%D0%B6%D0%B0%D0%BD%D0%B8%D0%B5)**

## **Возврат результата работы метода**

[](https://github.com/roistat/php-code-conventions?tab=readme-ov-file#%D0%B2%D0%BE%D0%B7%D0%B2%D1%80%D0%B0%D1%82-%D1%80%D0%B5%D0%B7%D1%83%D0%BB%D1%8C%D1%82%D0%B0%D1%82%D0%B0-%D1%80%D0%B0%D0%B1%D0%BE%D1%82%D1%8B-%D0%BC%D0%B5%D1%82%D0%BE%D0%B4%D0%B0)

### 📖 Метод всегда должен возвращать только одну структуру данных (или `null`) или ничего не возвращать

[](https://github.com/roistat/php-code-conventions?tab=readme-ov-file#-%D0%BC%D0%B5%D1%82%D0%BE%D0%B4-%D0%B2%D1%81%D0%B5%D0%B3%D0%B4%D0%B0-%D0%B4%D0%BE%D0%BB%D0%B6%D0%B5%D0%BD-%D0%B2%D0%BE%D0%B7%D0%B2%D1%80%D0%B0%D1%89%D0%B0%D1%82%D1%8C-%D1%82%D0%BE%D0%BB%D1%8C%D0%BA%D0%BE-%D0%BE%D0%B4%D0%BD%D1%83-%D1%81%D1%82%D1%80%D1%83%D0%BA%D1%82%D1%83%D1%80%D1%83-%D0%B4%D0%B0%D0%BD%D0%BD%D1%8B%D1%85-%D0%B8%D0%BB%D0%B8-null-%D0%B8%D0%BB%D0%B8-%D0%BD%D0%B8%D1%87%D0%B5%D0%B3%D0%BE-%D0%BD%D0%B5-%D0%B2%D0%BE%D0%B7%D0%B2%D1%80%D0%B0%D1%89%D0%B0%D1%82%D1%8C)

Метод не может в разных ситуациях возвращать разные типы данных.

Плохо:

```html
function loadUser() {
    if ($someCondition) {
        return ['id' => 1];
    }
    return new User();
}
```

Хорошо:

```html
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

[](https://github.com/roistat/php-code-conventions?tab=readme-ov-file#-%D0%B5%D1%81%D0%BB%D0%B8-%D0%BC%D0%B5%D1%82%D0%BE%D0%B4-%D0%B2%D0%BE%D0%B7%D0%B2%D1%80%D0%B0%D1%89%D0%B0%D0%B5%D1%82-%D0%BE%D0%B4%D0%B8%D0%BD-%D0%BE%D0%B1%D1%8A%D0%B5%D0%BA%D1%82-%D0%B8%D0%BB%D0%B8-%D1%81%D0%BA%D0%B0%D0%BB%D1%8F%D1%80%D0%BD%D1%8B%D0%B9-%D1%82%D0%B8%D0%BF-%D1%82%D0%BE-%D0%B2-%D1%81%D0%BB%D1%83%D1%87%D0%B0%D0%B5-%D0%B5%D1%81%D0%BB%D0%B8-%D0%BE%D0%B1%D1%8A%D0%B5%D0%BA%D1%82-%D0%BD%D0%B5-%D0%BD%D0%B0%D0%B9%D0%B4%D0%B5%D0%BD-%D0%B2%D0%BE%D0%B7%D0%B2%D1%80%D0%B0%D1%89%D0%B0%D0%B5%D1%82%D1%81%D1%8F-null)

Если же метод возвращает список объектов, то в случае, когда список пуст, возвращает пустой массив. Нельзя возвращать вместо пустого списка `null`.

Плохо:

```html
function loadUsers() {
    if ($someCondition) {
        return null;
    }
    return [new User()];
}
```

Хорошо:

```html
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

Пример: значения полей объекта задаются пользователем. Возможны две ситуации:

- пользователь не знает, каким категориям принадлежит объект — `null`
- пользователь знает, что объект не принадлежит ни одной категории — пустой массив (`[]`)

Тогда для получения категорий объекта будет правильным такой код:

```html
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

[](https://github.com/roistat/php-code-conventions?tab=readme-ov-file#-%D0%B2-%D0%B1%D0%BE%D0%BB%D1%8C%D1%88%D0%B8%D1%85-%D0%BC%D0%B5%D1%82%D0%BE%D0%B4%D0%B0%D1%85-%D0%B2%D0%BE%D0%B7%D0%B2%D1%80%D0%B0%D1%89%D0%B0%D0%B5%D0%BC%D0%B0%D1%8F-%D0%BF%D0%B5%D1%80%D0%B5%D0%BC%D0%B5%D0%BD%D0%BD%D0%B0%D1%8F-%D0%B4%D0%BE%D0%BB%D0%B6%D0%BD%D0%B0-%D0%BD%D0%B0%D0%B7%D1%8B%D0%B2%D0%B0%D1%82%D1%8C%D1%81%D1%8F-result)

Если у вас большой метод (больше 15 строк), возвращаемая переменная должна называться `$result`, если с ней могут происходить изменения в середине работы метода. В любом месте в методе должно быть понятно, где вы оперируете результатом, а где локальными переменными.

Плохо:

```html
function loadUsers(): array {
    $users = [];
    // ... много кода, изменяющего переменную $users
    return $users;
}
```

Хорошо:

```html
function loadUsers(): array {
    $result = [];
    // ... много кода, изменяющего переменную $result
    return $result;
}
```

### 📖 Метод должен явно отличать нормальные ситуации от исключительных

[](https://github.com/roistat/php-code-conventions?tab=readme-ov-file#-%D0%BC%D0%B5%D1%82%D0%BE%D0%B4-%D0%B4%D0%BE%D0%BB%D0%B6%D0%B5%D0%BD-%D1%8F%D0%B2%D0%BD%D0%BE-%D0%BE%D1%82%D0%BB%D0%B8%D1%87%D0%B0%D1%82%D1%8C-%D0%BD%D0%BE%D1%80%D0%BC%D0%B0%D0%BB%D1%8C%D0%BD%D1%8B%D0%B5-%D1%81%D0%B8%D1%82%D1%83%D0%B0%D1%86%D0%B8%D0%B8-%D0%BE%D1%82-%D0%B8%D1%81%D0%BA%D0%BB%D1%8E%D1%87%D0%B8%D1%82%D0%B5%D0%BB%D1%8C%D0%BD%D1%8B%D1%85)

Если никакой ошибки не произошло, но отсутствует результат, то это `null` (или пустой массив), однако если все же произошла исключительная ситуация, которая не заложена системой, то должно кидаться исключение.

Плохо:

```html
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
```

Хорошо:

```html
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

[](https://github.com/roistat/php-code-conventions?tab=readme-ov-file#-%D0%BC%D0%B5%D1%82%D0%BE%D0%B4-%D0%B4%D0%BE%D0%BB%D0%B6%D0%B5%D0%BD-%D0%BF%D1%80%D0%B8%D0%B4%D0%B5%D1%80%D0%B6%D0%B8%D0%B2%D0%B0%D1%82%D1%8C%D1%81%D1%8F-%D1%81%D0%BB%D0%B5%D0%B4%D1%83%D1%8E%D1%89%D0%B5%D0%B9-%D1%81%D1%82%D1%80%D1%83%D0%BA%D1%82%D1%83%D1%80%D1%8B-%D0%BF%D1%80%D0%BE%D0%B2%D0%B5%D1%80%D0%BA%D0%B0-%D0%BF%D0%B0%D1%80%D0%B0%D0%BC%D0%B5%D1%82%D1%80%D0%BE%D0%B2--%D0%BF%D0%BE%D0%BB%D1%83%D1%87%D0%B5%D0%BD%D0%B8%D0%B5-%D0%B4%D0%B0%D0%BD%D0%BD%D1%8B%D1%85--%D1%80%D0%B0%D0%B1%D0%BE%D1%82%D0%B0--%D1%80%D0%B5%D0%B7%D1%83%D0%BB%D1%8C%D1%82%D0%B0%D1%82)

Во время проверки параметров и получения необходимых данных метод должен возвращать соответствующее пустое значение или кидать исключение. После того как метод получил все необходимые данные и приступил к работе выход из метода крайне нежелателен. Возможны редкие исключения, облегчающие понимание и читаемость кода.

Плохо:

```html
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
```

Хорошо:

```html
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

**[⬆ наверх](https://github.com/roistat/php-code-conventions?tab=readme-ov-file#%D0%A1%D0%BE%D0%B4%D0%B5%D1%80%D0%B6%D0%B0%D0%BD%D0%B8%D0%B5)**

## **Работа с классами**

[](https://github.com/roistat/php-code-conventions?tab=readme-ov-file#%D1%80%D0%B0%D0%B1%D0%BE%D1%82%D0%B0-%D1%81-%D0%BA%D0%BB%D0%B0%D1%81%D1%81%D0%B0%D0%BC%D0%B8)

### 📖 Трейты имеют постфикс Trait

[](https://github.com/roistat/php-code-conventions?tab=readme-ov-file#-%D1%82%D1%80%D0%B5%D0%B9%D1%82%D1%8B-%D0%B8%D0%BC%D0%B5%D1%8E%D1%82-%D0%BF%D0%BE%D1%81%D1%82%D1%84%D0%B8%D0%BA%D1%81-trait)

Хорошо:

```html
trait AjaxResponseTrait {
    // ...
}
```

### 📖 Интерфейсы имеют постфикс Interface

[](https://github.com/roistat/php-code-conventions?tab=readme-ov-file#-%D0%B8%D0%BD%D1%82%D0%B5%D1%80%D1%84%D0%B5%D0%B9%D1%81%D1%8B-%D0%B8%D0%BC%D0%B5%D1%8E%D1%82-%D0%BF%D0%BE%D1%81%D1%82%D1%84%D0%B8%D0%BA%D1%81-interface)

Хорошо:

```html
interface ApplicationInterface {
    // ...
}
```

### 📖 Абстрактные классы имеют префикс Abstract

[](https://github.com/roistat/php-code-conventions?tab=readme-ov-file#-%D0%B0%D0%B1%D1%81%D1%82%D1%80%D0%B0%D0%BA%D1%82%D0%BD%D1%8B%D0%B5-%D0%BA%D0%BB%D0%B0%D1%81%D1%81%D1%8B-%D0%B8%D0%BC%D0%B5%D1%8E%D1%82-%D0%BF%D1%80%D0%B5%D1%84%D0%B8%D0%BA%D1%81-abstract)

Хорошо:

```html
abstract class AbstractApplication {
    // ...
}
```

### 📖 Все свойства и константы класса по умолчанию должны быть private

[](https://github.com/roistat/php-code-conventions?tab=readme-ov-file#-%D0%B2%D1%81%D0%B5-%D1%81%D0%B2%D0%BE%D0%B9%D1%81%D1%82%D0%B2%D0%B0-%D0%B8-%D0%BA%D0%BE%D0%BD%D1%81%D1%82%D0%B0%D0%BD%D1%82%D1%8B-%D0%BA%D0%BB%D0%B0%D1%81%D1%81%D0%B0-%D0%BF%D0%BE-%D1%83%D0%BC%D0%BE%D0%BB%D1%87%D0%B0%D0%BD%D0%B8%D1%8E-%D0%B4%D0%BE%D0%BB%D0%B6%D0%BD%D1%8B-%D0%B1%D1%8B%D1%82%D1%8C-private)

Если свойство используется наследниками класса, то оно объявляется `protected`. Если используется сторонними классами, тогда `public`.

Плохо:

```html
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
```

Хорошо:

```html
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

[](https://github.com/roistat/php-code-conventions?tab=readme-ov-file#-%D0%BC%D0%B5%D1%82%D0%BE%D0%B4%D1%8B-%D0%B8-%D1%81%D0%B2%D0%BE%D0%B9%D1%81%D1%82%D0%B2%D0%B0-%D0%B2-%D0%BA%D0%BB%D0%B0%D1%81%D1%81%D0%B5-%D0%B4%D0%BE%D0%BB%D0%B6%D0%BD%D1%8B-%D0%B1%D1%8B%D1%82%D1%8C-%D0%BE%D1%82%D1%81%D0%BE%D1%80%D1%82%D0%B8%D1%80%D0%BE%D0%B2%D0%B0%D0%BD%D1%8B-%D0%BF%D0%BE-%D1%83%D1%80%D0%BE%D0%B2%D0%BD%D1%8F%D0%BC-%D0%B2%D0%B8%D0%B4%D0%B8%D0%BC%D0%BE%D1%81%D1%82%D0%B8-%D0%B8-%D0%BF%D0%BE-%D0%BF%D0%BE%D1%80%D1%8F%D0%B4%D0%BA%D1%83-%D0%B8%D1%81%D0%BF%D0%BE%D0%BB%D1%8C%D0%B7%D0%BE%D0%B2%D0%B0%D0%BD%D0%B8%D1%8F-%D1%81%D0%B2%D0%B5%D1%80%D1%85%D1%83-%D0%B2%D0%BD%D0%B8%D0%B7)

Уровни видимости: `public` -> `protected` -> `private`.

Плохо:

```html
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
```

Хорошо:

```html
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

**[⬆ наверх](https://github.com/roistat/php-code-conventions?tab=readme-ov-file#%D0%A1%D0%BE%D0%B4%D0%B5%D1%80%D0%B6%D0%B0%D0%BD%D0%B8%D0%B5)**

## **Работа с объектами**

[](https://github.com/roistat/php-code-conventions?tab=readme-ov-file#%D1%80%D0%B0%D0%B1%D0%BE%D1%82%D0%B0-%D1%81-%D0%BE%D0%B1%D1%8A%D0%B5%D0%BA%D1%82%D0%B0%D0%BC%D0%B8)

### 📖 Все объекты должны быть неизменяемыми (immutable), если от них не требуется обратного

[](https://github.com/roistat/php-code-conventions?tab=readme-ov-file#-%D0%B2%D1%81%D0%B5-%D0%BE%D0%B1%D1%8A%D0%B5%D0%BA%D1%82%D1%8B-%D0%B4%D0%BE%D0%BB%D0%B6%D0%BD%D1%8B-%D0%B1%D1%8B%D1%82%D1%8C-%D0%BD%D0%B5%D0%B8%D0%B7%D0%BC%D0%B5%D0%BD%D1%8F%D0%B5%D0%BC%D1%8B%D0%BC%D0%B8-immutable-%D0%B5%D1%81%D0%BB%D0%B8-%D0%BE%D1%82-%D0%BD%D0%B8%D1%85-%D0%BD%D0%B5-%D1%82%D1%80%D0%B5%D0%B1%D1%83%D0%B5%D1%82%D1%81%D1%8F-%D0%BE%D0%B1%D1%80%D0%B0%D1%82%D0%BD%D0%BE%D0%B3%D0%BE)

Плохо:

```html
class SomeObject {
    /**
     * @var int
     */
    public $id;
}
```

Хорошо:

```html
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

[](https://github.com/roistat/php-code-conventions?tab=readme-ov-file#-%D1%81%D1%82%D0%B0%D1%82%D0%B8%D1%87%D0%B5%D1%81%D0%BA%D0%B8%D0%B5-%D0%B2%D1%8B%D0%B7%D0%BE%D0%B2%D1%8B-%D0%BC%D0%BE%D0%B6%D0%BD%D0%BE-%D0%B4%D0%B5%D0%BB%D0%B0%D1%82%D1%8C-%D1%82%D0%BE%D0%BB%D1%8C%D0%BA%D0%BE-%D1%83-%D1%81%D0%B0%D0%BC%D0%BE%D0%B3%D0%BE-%D0%BA%D0%BB%D0%B0%D1%81%D1%81%D0%B0-%D1%83-%D1%8D%D0%BA%D0%B7%D0%B5%D0%BC%D0%BF%D0%BB%D1%8F%D1%80%D0%B0-%D0%BC%D0%BE%D0%B6%D0%BD%D0%BE-%D0%BE%D0%B1%D1%80%D0%B0%D1%89%D0%B0%D1%82%D1%8C%D1%81%D1%8F-%D1%82%D0%BE%D0%BB%D1%8C%D0%BA%D0%BE-%D0%BA-%D0%B5%D0%B3%D0%BE-%D1%81%D0%B2%D0%BE%D0%B9%D1%81%D1%82%D0%B2%D0%B0%D0%BC-%D0%B8-%D0%BC%D0%B5%D1%82%D0%BE%D0%B4%D0%B0%D0%BC)

Плохо:

```html
$type = $user::TYPE;
```

Хорошо:

```html
$type = User::TYPE;
```

**[⬆ наверх](https://github.com/roistat/php-code-conventions?tab=readme-ov-file#%D0%A1%D0%BE%D0%B4%D0%B5%D1%80%D0%B6%D0%B0%D0%BD%D0%B8%D0%B5)**

## **Комментирование кода**

[](https://github.com/roistat/php-code-conventions?tab=readme-ov-file#%D0%BA%D0%BE%D0%BC%D0%BC%D0%B5%D0%BD%D1%82%D0%B8%D1%80%D0%BE%D0%B2%D0%B0%D0%BD%D0%B8%D0%B5-%D0%BA%D0%BE%D0%B4%D0%B0)

### 📖 В общем случае комментарии запрещены

[](https://github.com/roistat/php-code-conventions?tab=readme-ov-file#-%D0%B2-%D0%BE%D0%B1%D1%89%D0%B5%D0%BC-%D1%81%D0%BB%D1%83%D1%87%D0%B0%D0%B5-%D0%BA%D0%BE%D0%BC%D0%BC%D0%B5%D0%BD%D1%82%D0%B0%D1%80%D0%B8%D0%B8-%D0%B7%D0%B0%D0%BF%D1%80%D0%B5%D1%89%D0%B5%D0%BD%D1%8B)

Желание добавить комментарий — признак плохо читаемого кода. Любой участок кода, который вы хотели бы выделить или прокомментировать, надо выносить в отдельный метод.

Фразу, которую вы хотели написать в комментарии, надо привести в простой вид и сделать ее названием метода.

Плохо:

```html
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
```

Хорошо:

```html
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

[](https://github.com/roistat/php-code-conventions?tab=readme-ov-file#-%D0%B2%D1%8B%D0%BD%D1%83%D0%B6%D0%B4%D0%B5%D0%BD%D0%BD%D1%8B%D0%B5-%D1%85%D0%B0%D0%BA%D0%B8-%D0%B4%D0%BE%D0%BB%D0%B6%D0%BD%D1%8B-%D0%B1%D1%8B%D1%82%D1%8C-%D0%BF%D0%BE%D0%BC%D0%B5%D1%87%D0%B5%D0%BD%D1%8B-%D0%BA%D0%BE%D0%BC%D0%BC%D0%B5%D0%BD%D1%82%D0%B0%D1%80%D0%B8%D1%8F%D0%BC%D0%B8)

Лучше соблюдать одинаковый формат в рамках проекта

Хорошо:

```html
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

[](https://github.com/roistat/php-code-conventions?tab=readme-ov-file#-%D0%B3%D0%BE%D1%82%D0%BE%D0%B2%D1%8B%D0%B5-%D0%B0%D0%BB%D0%B3%D0%BE%D1%80%D0%B8%D1%82%D0%BC%D1%8B-%D0%B2%D0%B7%D1%8F%D1%82%D1%8B%D0%B5-%D0%B8%D0%B7-%D0%B2%D0%BD%D0%B5%D1%88%D0%BD%D0%B5%D0%B3%D0%BE-%D0%B8%D1%81%D1%82%D0%BE%D1%87%D0%BD%D0%B8%D0%BA%D0%B0-%D0%B4%D0%BE%D0%BB%D0%B6%D0%BD%D1%8B-%D0%B1%D1%8B%D1%82%D1%8C-%D0%BF%D0%BE%D0%BC%D0%B5%D1%87%D0%B5%D0%BD%D1%8B-%D1%81%D1%81%D1%8B%D0%BB%D0%BA%D0%BE%D0%B9-%D0%BD%D0%B0-%D0%B8%D1%81%D1%82%D0%BE%D1%87%D0%BD%D0%B8%D0%BA)

Хорошо:

```html
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

### 📖 При разработке прототипа допустимо помечать участки кода @todo

[](https://github.com/roistat/php-code-conventions?tab=readme-ov-file#-%D0%BF%D1%80%D0%B8-%D1%80%D0%B0%D0%B7%D1%80%D0%B0%D0%B1%D0%BE%D1%82%D0%BA%D0%B5-%D0%BF%D1%80%D0%BE%D1%82%D0%BE%D1%82%D0%B8%D0%BF%D0%B0-%D0%B4%D0%BE%D0%BF%D1%83%D1%81%D1%82%D0%B8%D0%BC%D0%BE-%D0%BF%D0%BE%D0%BC%D0%B5%D1%87%D0%B0%D1%82%D1%8C-%D1%83%D1%87%D0%B0%D1%81%D1%82%D0%BA%D0%B8-%D0%BA%D0%BE%D0%B4%D0%B0-todo)

Хорошо:

```html
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

**[⬆ наверх](https://github.com/roistat/php-code-conventions?tab=readme-ov-file#%D0%A1%D0%BE%D0%B4%D0%B5%D1%80%D0%B6%D0%B0%D0%BD%D0%B8%D0%B5)**

## **Работа с исключениями**

[](https://github.com/roistat/php-code-conventions?tab=readme-ov-file#%D1%80%D0%B0%D0%B1%D0%BE%D1%82%D0%B0-%D1%81-%D0%B8%D1%81%D0%BA%D0%BB%D1%8E%D1%87%D0%B5%D0%BD%D0%B8%D1%8F%D0%BC%D0%B8)

### 📖 На каждом уровне бизнес-логики (проект, компонент, библиотека) должно быть абстрактное базовое исключение

[](https://github.com/roistat/php-code-conventions?tab=readme-ov-file#-%D0%BD%D0%B0-%D0%BA%D0%B0%D0%B6%D0%B4%D0%BE%D0%BC-%D1%83%D1%80%D0%BE%D0%B2%D0%BD%D0%B5-%D0%B1%D0%B8%D0%B7%D0%BD%D0%B5%D1%81-%D0%BB%D0%BE%D0%B3%D0%B8%D0%BA%D0%B8-%D0%BF%D1%80%D0%BE%D0%B5%D0%BA%D1%82-%D0%BA%D0%BE%D0%BC%D0%BF%D0%BE%D0%BD%D0%B5%D0%BD%D1%82-%D0%B1%D0%B8%D0%B1%D0%BB%D0%B8%D0%BE%D1%82%D0%B5%D0%BA%D0%B0-%D0%B4%D0%BE%D0%BB%D0%B6%D0%BD%D0%BE-%D0%B1%D1%8B%D1%82%D1%8C-%D0%B0%D0%B1%D1%81%D1%82%D1%80%D0%B0%D0%BA%D1%82%D0%BD%D0%BE%D0%B5-%D0%B1%D0%B0%D0%B7%D0%BE%D0%B2%D0%BE%D0%B5-%D0%B8%D1%81%D0%BA%D0%BB%D1%8E%D1%87%D0%B5%D0%BD%D0%B8%D0%B5)

### 📖 Исключения сторонних библиотек должны быть перехвачены сразу

[](https://github.com/roistat/php-code-conventions?tab=readme-ov-file#-%D0%B8%D1%81%D0%BA%D0%BB%D1%8E%D1%87%D0%B5%D0%BD%D0%B8%D1%8F-%D1%81%D1%82%D0%BE%D1%80%D0%BE%D0%BD%D0%BD%D0%B8%D1%85-%D0%B1%D0%B8%D0%B1%D0%BB%D0%B8%D0%BE%D1%82%D0%B5%D0%BA-%D0%B4%D0%BE%D0%BB%D0%B6%D0%BD%D1%8B-%D0%B1%D1%8B%D1%82%D1%8C-%D0%BF%D0%B5%D1%80%D0%B5%D1%85%D0%B2%D0%B0%D1%87%D0%B5%D0%BD%D1%8B-%D1%81%D1%80%D0%B0%D0%B7%D1%83)

Далее либо обработаны, либо на их основании должно бросаться свое исключение. Новое исключение должно содержать предыдущее.

Хорошо:

```html
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

[](https://github.com/roistat/php-code-conventions?tab=readme-ov-file#-%D0%BF%D0%BE-%D1%83%D0%BC%D0%BE%D0%BB%D1%87%D0%B0%D0%BD%D0%B8%D1%8E-%D1%82%D0%B5%D0%BA%D1%81%D1%82%D1%8B-%D0%B8%D1%81%D0%BA%D0%BB%D1%8E%D1%87%D0%B5%D0%BD%D0%B8%D0%B9-%D0%BD%D0%B5-%D0%B4%D0%BE%D0%BB%D0%B6%D0%BD%D1%8B-%D0%BF%D0%BE%D0%BA%D0%B0%D0%B7%D1%8B%D0%B2%D0%B0%D1%82%D1%8C%D1%81%D1%8F-%D0%BF%D0%BE%D0%BB%D1%8C%D0%B7%D0%BE%D0%B2%D0%B0%D1%82%D0%B5%D0%BB%D1%8E)

Они предназначены для логирования и отладки. Текст исключения можно показать пользователю, если оно явно для этого предназначено: например, реализует интерфейс `HumanReadableInterface`.

```html
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

**[⬆ наверх](https://github.com/roistat/php-code-conventions?tab=readme-ov-file#%D0%A1%D0%BE%D0%B4%D0%B5%D1%80%D0%B6%D0%B0%D0%BD%D0%B8%D0%B5)**

## **Работа с внешним хранилищем данных**

[](https://github.com/roistat/php-code-conventions?tab=readme-ov-file#%D1%80%D0%B0%D0%B1%D0%BE%D1%82%D0%B0-%D1%81-%D0%B2%D0%BD%D0%B5%D1%88%D0%BD%D0%B8%D0%BC-%D1%85%D1%80%D0%B0%D0%BD%D0%B8%D0%BB%D0%B8%D1%89%D0%B5%D0%BC-%D0%B4%D0%B0%D0%BD%D0%BD%D1%8B%D1%85)

### 📖 Нельзя делать запросы к внешнему хранилищу внутри цикла с заведомо большим кол-вом итераций

[](https://github.com/roistat/php-code-conventions?tab=readme-ov-file#-%D0%BD%D0%B5%D0%BB%D1%8C%D0%B7%D1%8F-%D0%B4%D0%B5%D0%BB%D0%B0%D1%82%D1%8C-%D0%B7%D0%B0%D0%BF%D1%80%D0%BE%D1%81%D1%8B-%D0%BA-%D0%B2%D0%BD%D0%B5%D1%88%D0%BD%D0%B5%D0%BC%D1%83-%D1%85%D1%80%D0%B0%D0%BD%D0%B8%D0%BB%D0%B8%D1%89%D1%83-%D0%B2%D0%BD%D1%83%D1%82%D1%80%D0%B8-%D1%86%D0%B8%D0%BA%D0%BB%D0%B0-%D1%81-%D0%B7%D0%B0%D0%B2%D0%B5%D0%B4%D0%BE%D0%BC%D0%BE-%D0%B1%D0%BE%D0%BB%D1%8C%D1%88%D0%B8%D0%BC-%D0%BA%D0%BE%D0%BB-%D0%B2%D0%BE%D0%BC-%D0%B8%D1%82%D0%B5%D1%80%D0%B0%D1%86%D0%B8%D0%B9)

Плохо:

```html
$users = loadUsers();
foreach ($users as $user) {
    $userProjects = loadUserProjects($user);
    // ...
}
```

Хорошо:

```html
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

[](https://github.com/roistat/php-code-conventions?tab=readme-ov-file#-%D0%B4%D0%BB%D1%8F-%D0%BA%D0%B0%D0%B6%D0%B4%D0%BE%D0%B9-%D0%B7%D0%B0%D0%BF%D0%B8%D1%81%D0%B8-%D0%B2-%D1%85%D1%80%D0%B0%D0%BD%D0%B8%D0%BB%D0%B8%D1%89%D0%B5-%D0%B4%D0%BE%D0%BB%D0%B6%D0%BD%D0%BE-%D0%B1%D1%8B%D1%82%D1%8C-%D0%BF%D0%BE%D0%BD%D1%8F%D1%82%D0%BD%D0%B0-%D0%B4%D0%B0%D1%82%D0%B0-%D0%B5%D0%B5-%D1%81%D0%BE%D0%B7%D0%B4%D0%B0%D0%BD%D0%B8%D1%8F)

То есть должна быть колонка `date/creation_date`. Или должен быть зависимый объект (связь 1 к 1), у которого есть такая колонка. Редактируемые записи должны иметь и дату редактирования: `update_date` или `modification_date`.

**[⬆ наверх](https://github.com/roistat/php-code-conventions?tab=readme-ov-file#%D0%A1%D0%BE%D0%B4%D0%B5%D1%80%D0%B6%D0%B0%D0%BD%D0%B8%D0%B5)**

## **Особенности Pull Request (PR)**

[](https://github.com/roistat/php-code-conventions?tab=readme-ov-file#%D0%BE%D1%81%D0%BE%D0%B1%D0%B5%D0%BD%D0%BD%D0%BE%D1%81%D1%82%D0%B8-pull-request-pr)

### 📖 PR должен содержать как можно меньше строк кода

[](https://github.com/roistat/php-code-conventions?tab=readme-ov-file#-pr-%D0%B4%D0%BE%D0%BB%D0%B6%D0%B5%D0%BD-%D1%81%D0%BE%D0%B4%D0%B5%D1%80%D0%B6%D0%B0%D1%82%D1%8C-%D0%BA%D0%B0%D0%BA-%D0%BC%D0%BE%D0%B6%D0%BD%D0%BE-%D0%BC%D0%B5%D0%BD%D1%8C%D1%88%D0%B5-%D1%81%D1%82%D1%80%D0%BE%D0%BA-%D0%BA%D0%BE%D0%B4%D0%B0)

Любая атомарная часть кода должна выделяться в отдельную подзадачу и отдельный PR.

### 📖 Нельзя смешивать перенос методов в другие классы и места и последующий рефакторинг между собой

[](https://github.com/roistat/php-code-conventions?tab=readme-ov-file#-%D0%BD%D0%B5%D0%BB%D1%8C%D0%B7%D1%8F-%D1%81%D0%BC%D0%B5%D1%88%D0%B8%D0%B2%D0%B0%D1%82%D1%8C-%D0%BF%D0%B5%D1%80%D0%B5%D0%BD%D0%BE%D1%81-%D0%BC%D0%B5%D1%82%D0%BE%D0%B4%D0%BE%D0%B2-%D0%B2-%D0%B4%D1%80%D1%83%D0%B3%D0%B8%D0%B5-%D0%BA%D0%BB%D0%B0%D1%81%D1%81%D1%8B-%D0%B8-%D0%BC%D0%B5%D1%81%D1%82%D0%B0-%D0%B8-%D0%BF%D0%BE%D1%81%D0%BB%D0%B5%D0%B4%D1%83%D1%8E%D1%89%D0%B8%D0%B9-%D1%80%D0%B5%D1%84%D0%B0%D0%BA%D1%82%D0%BE%D1%80%D0%B8%D0%BD%D0%B3-%D0%BC%D0%B5%D0%B6%D0%B4%D1%83-%D1%81%D0%BE%D0%B1%D0%BE%D0%B9)

Перенос методов в другие классы и места должны быть выделены в отдельный PR. Последующий рефакторинг после переноса тоже должен быть в отдельном PR.

### 📖 В случае большого PR — ответственность за долгий просмотр несет сам разработчик, сделавший такой PR

[](https://github.com/roistat/php-code-conventions?tab=readme-ov-file#-%D0%B2-%D1%81%D0%BB%D1%83%D1%87%D0%B0%D0%B5-%D0%B1%D0%BE%D0%BB%D1%8C%D1%88%D0%BE%D0%B3%D0%BE-pr--%D0%BE%D1%82%D0%B2%D0%B5%D1%82%D1%81%D1%82%D0%B2%D0%B5%D0%BD%D0%BD%D0%BE%D1%81%D1%82%D1%8C-%D0%B7%D0%B0-%D0%B4%D0%BE%D0%BB%D0%B3%D0%B8%D0%B9-%D0%BF%D1%80%D0%BE%D1%81%D0%BC%D0%BE%D1%82%D1%80-%D0%BD%D0%B5%D1%81%D0%B5%D1%82-%D1%81%D0%B0%D0%BC-%D1%80%D0%B0%D0%B7%D1%80%D0%B0%D0%B1%D0%BE%D1%82%D1%87%D0%B8%D0%BA-%D1%81%D0%B4%D0%B5%D0%BB%D0%B0%D0%B2%D1%88%D0%B8%D0%B9-%D1%82%D0%B0%D0%BA%D0%BE%D0%B9-pr)

Нормальный объем кода — 1-300 строк в зависимости от его сложности. PR заглушек и архитектуры может содержать много формального кода, который легко быстро проверить. PR же конкретного метода может содержать много сложностей даже в 10 строчках.

### 📖 Нельзя накапливать изменения в какой-то своей ветке и потом делать большой PR в master

[](https://github.com/roistat/php-code-conventions?tab=readme-ov-file#-%D0%BD%D0%B5%D0%BB%D1%8C%D0%B7%D1%8F-%D0%BD%D0%B0%D0%BA%D0%B0%D0%BF%D0%BB%D0%B8%D0%B2%D0%B0%D1%82%D1%8C-%D0%B8%D0%B7%D0%BC%D0%B5%D0%BD%D0%B5%D0%BD%D0%B8%D1%8F-%D0%B2-%D0%BA%D0%B0%D0%BA%D0%BE%D0%B9-%D1%82%D0%BE-%D1%81%D0%B2%D0%BE%D0%B5%D0%B9-%D0%B2%D0%B5%D1%82%D0%BA%D0%B5-%D0%B8-%D0%BF%D0%BE%D1%82%D0%BE%D0%BC-%D0%B4%D0%B5%D0%BB%D0%B0%D1%82%D1%8C-%D0%B1%D0%BE%D0%BB%D1%8C%D1%88%D0%BE%D0%B9-pr-%D0%B2-master)

Все что можно смержить в master без последствий (даже если это еще не готовый результат, а только заглушки или часть, но они скрыты от юзеров и никому не мешают), должен мержиться в master и PR должен создаваться в master.

### 📖 В Pull Request не должно попадать кода, не относящегося к задаче

[](https://github.com/roistat/php-code-conventions?tab=readme-ov-file#-%D0%B2-pull-request-%D0%BD%D0%B5-%D0%B4%D0%BE%D0%BB%D0%B6%D0%BD%D0%BE-%D0%BF%D0%BE%D0%BF%D0%B0%D0%B4%D0%B0%D1%82%D1%8C-%D0%BA%D0%BE%D0%B4%D0%B0-%D0%BD%D0%B5-%D0%BE%D1%82%D0%BD%D0%BE%D1%81%D1%8F%D1%89%D0%B5%D0%B3%D0%BE%D1%81%D1%8F-%D0%BA-%D0%B7%D0%B0%D0%B4%D0%B0%D1%87%D0%B5)

Также не должно быть забытых комментариев, бессмысленных переносов строк и прочего "строительного мусора". Каждое изменение, которое вы предлагаете сделать в master-ветке, должно так или иначе относиться к решению поставленной вам задачи.

**[⬆ наверх](https://github.com/roistat/php-code-conventions?tab=readme-ov-file#%D0%A1%D0%BE%D0%B4%D0%B5%D1%80%D0%B6%D0%B0%D0%BD%D0%B8%D0%B5)**

## **Работа с шаблонами**

[](https://github.com/roistat/php-code-conventions?tab=readme-ov-file#%D1%80%D0%B0%D0%B1%D0%BE%D1%82%D0%B0-%D1%81-%D1%88%D0%B0%D0%B1%D0%BB%D0%BE%D0%BD%D0%B0%D0%BC%D0%B8)

### 📖 В шаблонах не должны вызываться методы объектов (геттеры не в счет)

[](https://github.com/roistat/php-code-conventions?tab=readme-ov-file#-%D0%B2-%D1%88%D0%B0%D0%B1%D0%BB%D0%BE%D0%BD%D0%B0%D1%85-%D0%BD%D0%B5-%D0%B4%D0%BE%D0%BB%D0%B6%D0%BD%D1%8B-%D0%B2%D1%8B%D0%B7%D1%8B%D0%B2%D0%B0%D1%82%D1%8C%D1%81%D1%8F-%D0%BC%D0%B5%D1%82%D0%BE%D0%B4%D1%8B-%D0%BE%D0%B1%D1%8A%D0%B5%D0%BA%D1%82%D0%BE%D0%B2-%D0%B3%D0%B5%D1%82%D1%82%D0%B5%D1%80%D1%8B-%D0%BD%D0%B5-%D0%B2-%D1%81%D1%87%D0%B5%D1%82)

Все необходимые данные должны быть загружены до рендера и переданы в виде параметров шаблона.

**[⬆ наверх](https://github.com/roistat/php-code-conventions?tab=readme-ov-file#%D0%A1%D0%BE%D0%B4%D0%B5%D1%80%D0%B6%D0%B0%D0%BD%D0%B8%D0%B5)**

## **Работа с литералами**

[](https://github.com/roistat/php-code-conventions?tab=readme-ov-file#%D1%80%D0%B0%D0%B1%D0%BE%D1%82%D0%B0-%D1%81-%D0%BB%D0%B8%D1%82%D0%B5%D1%80%D0%B0%D0%BB%D0%B0%D0%BC%D0%B8)

### 📖 Назначение всех числовых литералов должно быть понятным из контекста

[](https://github.com/roistat/php-code-conventions?tab=readme-ov-file#-%D0%BD%D0%B0%D0%B7%D0%BD%D0%B0%D1%87%D0%B5%D0%BD%D0%B8%D0%B5-%D0%B2%D1%81%D0%B5%D1%85-%D1%87%D0%B8%D1%81%D0%BB%D0%BE%D0%B2%D1%8B%D1%85-%D0%BB%D0%B8%D1%82%D0%B5%D1%80%D0%B0%D0%BB%D0%BE%D0%B2-%D0%B4%D0%BE%D0%BB%D0%B6%D0%BD%D0%BE-%D0%B1%D1%8B%D1%82%D1%8C-%D0%BF%D0%BE%D0%BD%D1%8F%D1%82%D0%BD%D1%8B%D0%BC-%D0%B8%D0%B7-%D0%BA%D0%BE%D0%BD%D1%82%D0%B5%D0%BA%D1%81%D1%82%D0%B0)

Они должны быть или вынесены в переменную или константу, или сравниваться с переменной, или передаваться на вход методу с понятной сигнатурой. В коде должен присутствовать в явном виде ответ: `за что отвечает это число и почему оно именно такое?`

Плохо:

```html
$isOnlyDeleted = 1;
if ($object->is_deleted === $isOnlyDeleted) {
    // ...
}
 
for ($i = 0; $i < 5; $i++) {
    // ...
}
```

Хорошо:

```html
if ($object->is_deleted === 1) {
    // ...
}
 
$apiMaxRetryLimit = 5;
for ($i = 0; $i < $apiMaxRetryLimit; $i++) {
    // ...
} 
```

**[⬆ наверх](https://github.com/roistat/php-code-conventions?tab=readme-ov-file#%D0%A1%D0%BE%D0%B4%D0%B5%D1%80%D0%B6%D0%B0%D0%BD%D0%B8%D0%B5)**

## **Работа с условиями**

[](https://github.com/roistat/php-code-conventions?tab=readme-ov-file#%D1%80%D0%B0%D0%B1%D0%BE%D1%82%D0%B0-%D1%81-%D1%83%D1%81%D0%BB%D0%BE%D0%B2%D0%B8%D1%8F%D0%BC%D0%B8)

### 📖 В условном операторе должно проверяться исключительно `boolean` значение

[](https://github.com/roistat/php-code-conventions?tab=readme-ov-file#-%D0%B2-%D1%83%D1%81%D0%BB%D0%BE%D0%B2%D0%BD%D0%BE%D0%BC-%D0%BE%D0%BF%D0%B5%D1%80%D0%B0%D1%82%D0%BE%D1%80%D0%B5-%D0%B4%D0%BE%D0%BB%D0%B6%D0%BD%D0%BE-%D0%BF%D1%80%D0%BE%D0%B2%D0%B5%D1%80%D1%8F%D1%82%D1%8C%D1%81%D1%8F-%D0%B8%D1%81%D0%BA%D0%BB%D1%8E%D1%87%D0%B8%D1%82%D0%B5%D0%BB%D1%8C%D0%BD%D0%BE-boolean-%D0%B7%D0%BD%D0%B0%D1%87%D0%B5%D0%BD%D0%B8%D0%B5)

Плохо:

```html
if (count($userProjects)) {
    // ...
}
if ($project) {
    // ...
}
```

Хорошо:

```html
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

### 📖 В сравнении не boolean переменных используется строгое сравнение с приведением типа (===), автоматическое приведение и нестрогое сравнение не используются

[](https://github.com/roistat/php-code-conventions?tab=readme-ov-file#-%D0%B2-%D1%81%D1%80%D0%B0%D0%B2%D0%BD%D0%B5%D0%BD%D0%B8%D0%B8-%D0%BD%D0%B5-boolean-%D0%BF%D0%B5%D1%80%D0%B5%D0%BC%D0%B5%D0%BD%D0%BD%D1%8B%D1%85-%D0%B8%D1%81%D0%BF%D0%BE%D0%BB%D1%8C%D0%B7%D1%83%D0%B5%D1%82%D1%81%D1%8F-%D1%81%D1%82%D1%80%D0%BE%D0%B3%D0%BE%D0%B5-%D1%81%D1%80%D0%B0%D0%B2%D0%BD%D0%B5%D0%BD%D0%B8%D0%B5-%D1%81-%D0%BF%D1%80%D0%B8%D0%B2%D0%B5%D0%B4%D0%B5%D0%BD%D0%B8%D0%B5%D0%BC-%D1%82%D0%B8%D0%BF%D0%B0--%D0%B0%D0%B2%D1%82%D0%BE%D0%BC%D0%B0%D1%82%D0%B8%D1%87%D0%B5%D1%81%D0%BA%D0%BE%D0%B5-%D0%BF%D1%80%D0%B8%D0%B2%D0%B5%D0%B4%D0%B5%D0%BD%D0%B8%D0%B5-%D0%B8-%D0%BD%D0%B5%D1%81%D1%82%D1%80%D0%BE%D0%B3%D0%BE%D0%B5-%D1%81%D1%80%D0%B0%D0%B2%D0%BD%D0%B5%D0%BD%D0%B8%D0%B5-%D0%BD%D0%B5-%D0%B8%D1%81%D0%BF%D0%BE%D0%BB%D1%8C%D0%B7%D1%83%D1%8E%D1%82%D1%81%D1%8F)

Плохо:

```html
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
```

Хорошо:

```html
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

[](https://github.com/roistat/php-code-conventions?tab=readme-ov-file#-%D0%B0%D0%B2%D1%82%D0%BE%D0%BC%D0%B0%D1%82%D0%B8%D1%87%D0%B5%D1%81%D0%BA%D0%BE%D0%B5-%D0%BF%D1%80%D0%B8%D0%B2%D0%B5%D0%B4%D0%B5%D0%BD%D0%B8%D0%B5-%D1%82%D0%B8%D0%BF%D0%BE%D0%B2-%D1%80%D0%B0%D0%B7%D1%80%D0%B5%D1%88%D0%B5%D0%BD%D0%BE-%D1%82%D0%BE%D0%BB%D1%8C%D0%BA%D0%BE-%D0%BA%D0%BE%D0%B3%D0%B4%D0%B0-%D0%BE%D0%B4%D0%B8%D0%BD-%D0%B8%D0%B7-%D0%BE%D0%BF%D0%B5%D1%80%D0%B0%D0%BD%D0%B4%D0%BE%D0%B2--%D0%BB%D0%B8%D1%82%D0%B5%D1%80%D0%B0%D0%BB-%D1%81-%D1%84%D0%B8%D0%BA%D1%81%D0%B8%D1%80%D0%BE%D0%B2%D0%B0%D0%BD%D0%BD%D1%8B%D0%BC-%D1%82%D0%B8%D0%BF%D0%BE%D0%BC)

При сравнении двух переменных с неизвестными типами для читающего код человека не очевидно, к чему они будут приведены интерпретатором. Если же тип одного из операндов известен, то всё становится очевидно и ручное приведение типов не требуется.

Если вы хотите проверить значение `boolean` пришедшее извне, то делается это так:

Плохо:

```html
if ((int)$request->get('is_something') > 0) {
    // ...
}
if ((int)$request->get('is_something') === 1) {
    // ...
}
if ((int)$user->is_registered === 0) {
    // ...
}
```

Хорошо:

```html
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

[](https://github.com/roistat/php-code-conventions?tab=readme-ov-file#-%D0%BD%D0%B5-%D0%BD%D0%B0%D0%B4%D0%BE-%D1%81%D1%80%D0%B0%D0%B2%D0%BD%D0%B8%D0%B2%D0%B0%D1%82%D1%8C-boolean-%D1%81-truefalse)

Это нарушает запрет на бесполезный код.

Плохо:

```html
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
```

Хорошо:

```html
if ($bill->isPaid()) {
    // ...
}
```

### 📖 Проверять переменные надо на наличие позитивного вхождения, а не отсутствие негативного

[](https://github.com/roistat/php-code-conventions?tab=readme-ov-file#-%D0%BF%D1%80%D0%BE%D0%B2%D0%B5%D1%80%D1%8F%D1%82%D1%8C-%D0%BF%D0%B5%D1%80%D0%B5%D0%BC%D0%B5%D0%BD%D0%BD%D1%8B%D0%B5-%D0%BD%D0%B0%D0%B4%D0%BE-%D0%BD%D0%B0-%D0%BD%D0%B0%D0%BB%D0%B8%D1%87%D0%B8%D0%B5-%D0%BF%D0%BE%D0%B7%D0%B8%D1%82%D0%B8%D0%B2%D0%BD%D0%BE%D0%B3%D0%BE-%D0%B2%D1%85%D0%BE%D0%B6%D0%B4%D0%B5%D0%BD%D0%B8%D1%8F-%D0%B0-%D0%BD%D0%B5-%D0%BE%D1%82%D1%81%D1%83%D1%82%D1%81%D1%82%D0%B2%D0%B8%D0%B5-%D0%BD%D0%B5%D0%B3%D0%B0%D1%82%D0%B8%D0%B2%D0%BD%D0%BE%D0%B3%D0%BE)

Если вам нужна строка, то проверять надо на то, что переменная является строкой. Не надо проверять на то, что она не является числом или чем-то еще. Перечислять все возможные варианты, чем переменная не должна быть, значит повышать риск ошибки и усложнять поддержку кода.

Плохо:

```html
if (!is_numeric($value) && !is_object($value)) {
    // ...
}
```

Хорошо:

```html
if (is_string($value) && $value !== '') {
    // ...
}
```

### 📖 Если вы используете встроенную функцию PHP, которая возвращает `0`, `1` и, возможно, `false`, то при возможности результат ее работы используем в условии как `bool` без дополнительных сравнений

[](https://github.com/roistat/php-code-conventions?tab=readme-ov-file#-%D0%B5%D1%81%D0%BB%D0%B8-%D0%B2%D1%8B-%D0%B8%D1%81%D0%BF%D0%BE%D0%BB%D1%8C%D0%B7%D1%83%D0%B5%D1%82%D0%B5-%D0%B2%D1%81%D1%82%D1%80%D0%BE%D0%B5%D0%BD%D0%BD%D1%83%D1%8E-%D1%84%D1%83%D0%BD%D0%BA%D1%86%D0%B8%D1%8E-php-%D0%BA%D0%BE%D1%82%D0%BE%D1%80%D0%B0%D1%8F-%D0%B2%D0%BE%D0%B7%D0%B2%D1%80%D0%B0%D1%89%D0%B0%D0%B5%D1%82-0-1-%D0%B8-%D0%B2%D0%BE%D0%B7%D0%BC%D0%BE%D0%B6%D0%BD%D0%BE-false-%D1%82%D0%BE-%D0%BF%D1%80%D0%B8-%D0%B2%D0%BE%D0%B7%D0%BC%D0%BE%D0%B6%D0%BD%D0%BE%D1%81%D1%82%D0%B8-%D1%80%D0%B5%D0%B7%D1%83%D0%BB%D1%8C%D1%82%D0%B0%D1%82-%D0%B5%D0%B5-%D1%80%D0%B0%D0%B1%D0%BE%D1%82%D1%8B-%D0%B8%D1%81%D0%BF%D0%BE%D0%BB%D1%8C%D0%B7%D1%83%D0%B5%D0%BC-%D0%B2-%D1%83%D1%81%D0%BB%D0%BE%D0%B2%D0%B8%D0%B8-%D0%BA%D0%B0%D0%BA-bool-%D0%B1%D0%B5%D0%B7-%D0%B4%D0%BE%D0%BF%D0%BE%D0%BB%D0%BD%D0%B8%D1%82%D0%B5%D0%BB%D1%8C%D0%BD%D1%8B%D1%85-%D1%81%D1%80%D0%B0%D0%B2%D0%BD%D0%B5%D0%BD%D0%B8%D0%B9)

Это не касается случая, когда вам нужно отделить два разных результата между собой, например отдельно отработать `0` и `false`.

Плохо:

```html
if (preg_match($pattern, $subject) === 1) {
    // ...
}
if (!strpos($search, $text)) {
    // ...
}
```

Хорошо:

```html
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

[](https://github.com/roistat/php-code-conventions?tab=readme-ov-file#-%D0%BF%D1%80%D0%B8-%D0%B8%D1%81%D0%BF%D0%BE%D0%BB%D1%8C%D0%B7%D0%BE%D0%B2%D0%B0%D0%BD%D0%B8%D0%B8-%D0%B2-%D1%83%D1%81%D0%BB%D0%BE%D0%B2%D0%BD%D0%BE%D0%BC-%D0%B2%D1%8B%D1%80%D0%B0%D0%B6%D0%B5%D0%BD%D0%B8%D0%B8-%D0%BE%D0%B4%D0%BD%D0%BE%D0%B2%D1%80%D0%B5%D0%BC%D0%B5%D0%BD%D0%BD%D0%BE-%D0%BE%D0%BF%D0%B5%D1%80%D0%B0%D1%82%D0%BE%D1%80%D0%BE%D0%B2-%D0%B8-%D0%B8-%D0%B8%D0%BB%D0%B8-%D0%BE%D0%B1%D1%8F%D0%B7%D0%B0%D1%82%D0%B5%D0%BB%D1%8C%D0%BD%D0%BE-%D0%B2%D1%8B%D0%B4%D0%B5%D0%BB%D1%8F%D1%82%D1%8C-%D0%BF%D1%80%D0%B8%D0%BE%D1%80%D0%B8%D1%82%D0%B5%D1%82-%D1%81%D0%BA%D0%BE%D0%B1%D0%BA%D0%B0%D0%BC%D0%B8)

Обратите внимание на различие в значении двух вариантов правильного использования

Плохо:

```html
if ($isMobile || $isSizeTooBig && $isAllowedToShrink) {
    // ...
}
```

Хорошо:

```html
if (($isMobile || $isSizeTooBig) && $isAllowedToShrink) {
    // ...
}
if ($isMobile || ($isSizeTooBig && $isAllowedToShrink)) {
    // ...
}
```

**[⬆ наверх](https://github.com/roistat/php-code-conventions?tab=readme-ov-file#%D0%A1%D0%BE%D0%B4%D0%B5%D1%80%D0%B6%D0%B0%D0%BD%D0%B8%D0%B5)**

## **Работа с тернарными операторами**

[](https://github.com/roistat/php-code-conventions?tab=readme-ov-file#%D1%80%D0%B0%D0%B1%D0%BE%D1%82%D0%B0-%D1%81-%D1%82%D0%B5%D1%80%D0%BD%D0%B0%D1%80%D0%BD%D1%8B%D0%BC%D0%B8-%D0%BE%D0%BF%D0%B5%D1%80%D0%B0%D1%82%D0%BE%D1%80%D0%B0%D0%BC%D0%B8)

### 📖 При использовании тернарных операторов действуют те же правила, что и при использовании условий

[](https://github.com/roistat/php-code-conventions?tab=readme-ov-file#-%D0%BF%D1%80%D0%B8-%D0%B8%D1%81%D0%BF%D0%BE%D0%BB%D1%8C%D0%B7%D0%BE%D0%B2%D0%B0%D0%BD%D0%B8%D0%B8-%D1%82%D0%B5%D1%80%D0%BD%D0%B0%D1%80%D0%BD%D1%8B%D1%85-%D0%BE%D0%BF%D0%B5%D1%80%D0%B0%D1%82%D0%BE%D1%80%D0%BE%D0%B2-%D0%B4%D0%B5%D0%B9%D1%81%D1%82%D0%B2%D1%83%D1%8E%D1%82-%D1%82%D0%B5-%D0%B6%D0%B5-%D0%BF%D1%80%D0%B0%D0%B2%D0%B8%D0%BB%D0%B0-%D1%87%D1%82%D0%BE-%D0%B8-%D0%BF%D1%80%D0%B8-%D0%B8%D1%81%D0%BF%D0%BE%D0%BB%D1%8C%D0%B7%D0%BE%D0%B2%D0%B0%D0%BD%D0%B8%D0%B8-%D1%83%D1%81%D0%BB%D0%BE%D0%B2%D0%B8%D0%B9)

### 📖 Тернарный оператор следует использовать, если обе ветви условия предназначены для установки одной переменной одним языковым выражением

[](https://github.com/roistat/php-code-conventions?tab=readme-ov-file#-%D1%82%D0%B5%D1%80%D0%BD%D0%B0%D1%80%D0%BD%D1%8B%D0%B9-%D0%BE%D0%BF%D0%B5%D1%80%D0%B0%D1%82%D0%BE%D1%80-%D1%81%D0%BB%D0%B5%D0%B4%D1%83%D0%B5%D1%82-%D0%B8%D1%81%D0%BF%D0%BE%D0%BB%D1%8C%D0%B7%D0%BE%D0%B2%D0%B0%D1%82%D1%8C-%D0%B5%D1%81%D0%BB%D0%B8-%D0%BE%D0%B1%D0%B5-%D0%B2%D0%B5%D1%82%D0%B2%D0%B8-%D1%83%D1%81%D0%BB%D0%BE%D0%B2%D0%B8%D1%8F-%D0%BF%D1%80%D0%B5%D0%B4%D0%BD%D0%B0%D0%B7%D0%BD%D0%B0%D1%87%D0%B5%D0%BD%D1%8B-%D0%B4%D0%BB%D1%8F-%D1%83%D1%81%D1%82%D0%B0%D0%BD%D0%BE%D0%B2%D0%BA%D0%B8-%D0%BE%D0%B4%D0%BD%D0%BE%D0%B9-%D0%BF%D0%B5%D1%80%D0%B5%D0%BC%D0%B5%D0%BD%D0%BD%D0%BE%D0%B9-%D0%BE%D0%B4%D0%BD%D0%B8%D0%BC-%D1%8F%D0%B7%D1%8B%D0%BA%D0%BE%D0%B2%D1%8B%D0%BC-%D0%B2%D1%8B%D1%80%D0%B0%D0%B6%D0%B5%D0%BD%D0%B8%D0%B5%D0%BC)

При наличии логики в ветках условия следует рассмотреть возможность вынести ее в отдельный метод.

Плохо:

```html
if ($isExternal) {
    $bill = $this->loadExternalBill();
} else {
    $bill = $this->loadInternalBill();
}
```

Хорошо:

```html
$bill = $isExternal ? $this->loadExternalBill() : $this->loadInternalBill();
```

### 📖 Использовать цепочки из тернарных операторов `?:` допустимо только при указании значения по умолчанию

[](https://github.com/roistat/php-code-conventions?tab=readme-ov-file#-%D0%B8%D1%81%D0%BF%D0%BE%D0%BB%D1%8C%D0%B7%D0%BE%D0%B2%D0%B0%D1%82%D1%8C-%D1%86%D0%B5%D0%BF%D0%BE%D1%87%D0%BA%D0%B8-%D0%B8%D0%B7-%D1%82%D0%B5%D1%80%D0%BD%D0%B0%D1%80%D0%BD%D1%8B%D1%85-%D0%BE%D0%BF%D0%B5%D1%80%D0%B0%D1%82%D0%BE%D1%80%D0%BE%D0%B2--%D0%B4%D0%BE%D0%BF%D1%83%D1%81%D1%82%D0%B8%D0%BC%D0%BE-%D1%82%D0%BE%D0%BB%D1%8C%D0%BA%D0%BE-%D0%BF%D1%80%D0%B8-%D1%83%D0%BA%D0%B0%D0%B7%D0%B0%D0%BD%D0%B8%D0%B8-%D0%B7%D0%BD%D0%B0%D1%87%D0%B5%D0%BD%D0%B8%D1%8F-%D0%BF%D0%BE-%D1%83%D0%BC%D0%BE%D0%BB%D1%87%D0%B0%D0%BD%D0%B8%D1%8E)

Плохо:

```html
$contact = $this->loadContactByPhone() ?: $this->loadContactByEmail() ?: $this->loadContactByName();
```

Хорошо:

```html
$lead = $this->loadLeadFromCache() ?: $this->loadLeadFromDB();
$contact = $this->loadContactByPhone() ?: $this->loadContactByEmail() ?: $this->loadContactByName() ?: null;
```

**[⬆ наверх](https://github.com/roistat/php-code-conventions?tab=readme-ov-file#%D0%A1%D0%BE%D0%B4%D0%B5%D1%80%D0%B6%D0%B0%D0%BD%D0%B8%D0%B5)**

## **Про тесты**

[](https://github.com/roistat/php-code-conventions?tab=readme-ov-file#%D0%BF%D1%80%D0%BE-%D1%82%D0%B5%D1%81%D1%82%D1%8B)

### 📖 Тесты являются таким же production-кодом, как и любой другой код

[](https://github.com/roistat/php-code-conventions?tab=readme-ov-file#-%D1%82%D0%B5%D1%81%D1%82%D1%8B-%D1%8F%D0%B2%D0%BB%D1%8F%D1%8E%D1%82%D1%81%D1%8F-%D1%82%D0%B0%D0%BA%D0%B8%D0%BC-%D0%B6%D0%B5-production-%D0%BA%D0%BE%D0%B4%D0%BE%D0%BC-%D0%BA%D0%B0%D0%BA-%D0%B8-%D0%BB%D1%8E%D0%B1%D0%BE%D0%B9-%D0%B4%D1%80%D1%83%D0%B3%D0%BE%D0%B9-%D0%BA%D0%BE%D0%B4)

Они должны быть написаны с соблюдением соглашений, описанных в этом документе.

### 📖 В дата провайдерах для тестов надо писать комментарий или ассоциативный массив к структуре отдаваемого массива значений

[](https://github.com/roistat/php-code-conventions?tab=readme-ov-file#-%D0%B2-%D0%B4%D0%B0%D1%82%D0%B0-%D0%BF%D1%80%D0%BE%D0%B2%D0%B0%D0%B9%D0%B4%D0%B5%D1%80%D0%B0%D1%85-%D0%B4%D0%BB%D1%8F-%D1%82%D0%B5%D1%81%D1%82%D0%BE%D0%B2-%D0%BD%D0%B0%D0%B4%D0%BE-%D0%BF%D0%B8%D1%81%D0%B0%D1%82%D1%8C-%D0%BA%D0%BE%D0%BC%D0%BC%D0%B5%D0%BD%D1%82%D0%B0%D1%80%D0%B8%D0%B9-%D0%B8%D0%BB%D0%B8-%D0%B0%D1%81%D1%81%D0%BE%D1%86%D0%B8%D0%B0%D1%82%D0%B8%D0%B2%D0%BD%D1%8B%D0%B9-%D0%BC%D0%B0%D1%81%D1%81%D0%B8%D0%B2-%D0%BA-%D1%81%D1%82%D1%80%D1%83%D0%BA%D1%82%D1%83%D1%80%D0%B5-%D0%BE%D1%82%D0%B4%D0%B0%D0%B2%D0%B0%D0%B5%D0%BC%D0%BE%D0%B3%D0%BE-%D0%BC%D0%B0%D1%81%D1%81%D0%B8%D0%B2%D0%B0-%D0%B7%D0%BD%D0%B0%D1%87%D0%B5%D0%BD%D0%B8%D0%B9)

Плохо:

```html
public function isEmailAddressData(): array {
    return [
        ['test@test.ru',            true ],
        // ...
    ]
}
```

Хорошо:

```html
public function isEmailAddressData(): array {
    return [
        //    email               isValid
        ['test@test.ru',            true],
        ['@test.ru',                false],
        ['invalidEmail',            false],
        // ...
    ]
}

// Или:

public function isEmailAddressData(): array {
    return [
        'valid' =>          ['email' => 'test@test.ru', 'isValid' => true],
        'invalid with @' => ['email' => '@test.ru',     'isValid' => false],
        'invalid'        => ['email' => 'invalidEmail', 'isValid' => false],
        // ...
    ]
}

// Или:

public function isEmailAddressData(): \Generator {
    yield 'valid' => ['email' => 'test@test.ru', 'isValid' => true];
    yield 'invalid with @' => ['email' => '@test.ru',     'isValid' => false];
}
```

**[⬆ наверх](https://github.com/roistat/php-code-conventions?tab=readme-ov-file#%D0%A1%D0%BE%D0%B4%D0%B5%D1%80%D0%B6%D0%B0%D0%BD%D0%B8%D0%B5)**

## **Использование chain-объектов**

[](https://github.com/roistat/php-code-conventions?tab=readme-ov-file#%D0%B8%D1%81%D0%BF%D0%BE%D0%BB%D1%8C%D0%B7%D0%BE%D0%B2%D0%B0%D0%BD%D0%B8%D0%B5-chain-%D0%BE%D0%B1%D1%8A%D0%B5%D0%BA%D1%82%D0%BE%D0%B2)

### 📖 Метод с большим количеством необязательных параметров (А) может быть заменен chain-объектом

[](https://github.com/roistat/php-code-conventions?tab=readme-ov-file#-%D0%BC%D0%B5%D1%82%D0%BE%D0%B4-%D1%81-%D0%B1%D0%BE%D0%BB%D1%8C%D1%88%D0%B8%D0%BC-%D0%BA%D0%BE%D0%BB%D0%B8%D1%87%D0%B5%D1%81%D1%82%D0%B2%D0%BE%D0%BC-%D0%BD%D0%B5%D0%BE%D0%B1%D1%8F%D0%B7%D0%B0%D1%82%D0%B5%D0%BB%D1%8C%D0%BD%D1%8B%D1%85-%D0%BF%D0%B0%D1%80%D0%B0%D0%BC%D0%B5%D1%82%D1%80%D0%BE%D0%B2-%D0%B0-%D0%BC%D0%BE%D0%B6%D0%B5%D1%82-%D0%B1%D1%8B%D1%82%D1%8C-%D0%B7%D0%B0%D0%BC%D0%B5%D0%BD%D0%B5%D0%BD-chain-%D0%BE%D0%B1%D1%8A%D0%B5%D0%BA%D1%82%D0%BE%D0%BC)

Метод с большим количеством необязательных параметров (А) может быть заменен chain-объектом. В объекте конструктор принимает все обязательные параметры, а все необязательные реализуются сеттерами без глагола set (только существительное), возвращающими текущий объект (chaining методов). Метод-глагол у объекта один без параметров, он завершает использование объекта и выполняет действие, которое должен был выполнить метод А.

**Был метод:**

```html
function send($method, $url, $body = null, $headers = null, $retries = 1, $timeout = 300) {}
```

**Должен замениться на chain-объект:**

```html
public function __construct($method, $url) {
    // ...
}

public function body($body) {
    return $this;
}
// остальные методы с необязательными параметрами

public function send();
```

**Новый объект используется так:**

```html
new $sender($method, $url)->body($body)->retries(10)->timeout(25)->send();
```

**[⬆ наверх](https://github.com/roistat/php-code-conventions?tab=readme-ov-file#%D0%A1%D0%BE%D0%B4%D0%B5%D1%80%D0%B6%D0%B0%D0%BD%D0%B8%D0%B5)**

## **Работа со скриптами**

[](https://github.com/roistat/php-code-conventions?tab=readme-ov-file#%D1%80%D0%B0%D0%B1%D0%BE%D1%82%D0%B0-%D1%81%D0%BE-%D1%81%D0%BA%D1%80%D0%B8%D0%BF%D1%82%D0%B0%D0%BC%D0%B8)

### 📖 Любой скрипт, который изменяет данные, должен иметь подтверждение перед выполнением действий с данными и `debug` по результатам работы

[](https://github.com/roistat/php-code-conventions?tab=readme-ov-file#-%D0%BB%D1%8E%D0%B1%D0%BE%D0%B9-%D1%81%D0%BA%D1%80%D0%B8%D0%BF%D1%82-%D0%BA%D0%BE%D1%82%D0%BE%D1%80%D1%8B%D0%B9-%D0%B8%D0%B7%D0%BC%D0%B5%D0%BD%D1%8F%D0%B5%D1%82-%D0%B4%D0%B0%D0%BD%D0%BD%D1%8B%D0%B5-%D0%B4%D0%BE%D0%BB%D0%B6%D0%B5%D0%BD-%D0%B8%D0%BC%D0%B5%D1%82%D1%8C-%D0%BF%D0%BE%D0%B4%D1%82%D0%B2%D0%B5%D1%80%D0%B6%D0%B4%D0%B5%D0%BD%D0%B8%D0%B5-%D0%BF%D0%B5%D1%80%D0%B5%D0%B4-%D0%B2%D1%8B%D0%BF%D0%BE%D0%BB%D0%BD%D0%B5%D0%BD%D0%B8%D0%B5%D0%BC-%D0%B4%D0%B5%D0%B9%D1%81%D1%82%D0%B2%D0%B8%D0%B9-%D1%81-%D0%B4%D0%B0%D0%BD%D0%BD%D1%8B%D0%BC%D0%B8-%D0%B8-debug-%D0%BF%D0%BE-%D1%80%D0%B5%D0%B7%D1%83%D0%BB%D1%8C%D1%82%D0%B0%D1%82%D0%B0%D0%BC-%D1%80%D0%B0%D0%B1%D0%BE%D1%82%D1%8B)

Плохо:

```html
// cli/delete_items.php
$repository->deleteItems();
```

Исправим, чтобы случайный запуск не удалил элементы:

Хорошо:

```html
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

**[⬆ наверх](https://github.com/roistat/php-code-conventions?tab=readme-ov-file#%D0%A1%D0%BE%D0%B4%D0%B5%D1%80%D0%B6%D0%B0%D0%BD%D0%B8%D0%B5)**

## **Авторы**

[](https://github.com/roistat/php-code-conventions?tab=readme-ov-file#%D0%B0%D0%B2%D1%82%D0%BE%D1%80%D1%8B)

- Удодов Евгений ([flrnull](https://github.com/flrnull))
- Рудаченко Сергей ([m1nor](https://github.com/m1nor))
- Зюзькевич Юрий ([Farengier](https://github.com/Farengier))

## About

Code concepts, principles and examples for large long term projects

### Resources

 [Readme](https://github.com/roistat/php-code-conventions?tab=readme-ov-file#readme-ov-file)

### License

 [MIT license](https://github.com/roistat/php-code-conventions?tab=readme-ov-file#MIT-1-ov-file)

 [Activity](https://github.com/roistat/php-code-conventions/activity)

 [Custom properties](https://github.com/roistat/php-code-conventions/custom-properties)

### Stars

 [**286** stars](https://github.com/roistat/php-code-conventions/stargazers)

### Watchers

 [**34** watching](https://github.com/roistat/php-code-conventions/watchers)

### Forks

 [**92** forks](https://github.com/roistat/php-code-conventions/forks)

[Report repository](https://github.com/contact/report-content?content_url=https%3A%2F%2Fgithub.com%2Froistat%2Fphp-code-conventions&report=roistat+%28user%29)

## [Releases](https://github.com/roistat/php-code-conventions/releases)

No releases published

## [Packages](https://github.com/orgs/roistat/packages?repo_name=php-code-conventions)

No packages published  

## [Contributors21](https://github.com/roistat/php-code-conventions/graphs/contributors)

- [![@flrnull](https://avatars.githubusercontent.com/u/1926460?s=64&v=4)](https://github.com/flrnull)
- [![@m1nor](https://avatars.githubusercontent.com/u/1577160?s=64&v=4)](https://github.com/m1nor)
- [![@kzon](https://avatars.githubusercontent.com/u/6276455?s=64&v=4)](https://github.com/kzon)
- [![@buildie](https://avatars.githubusercontent.com/u/8582962?s=64&v=4)](https://github.com/buildie)
- [![@Farengier](https://avatars.githubusercontent.com/u/8510580?s=64&v=4)](https://github.com/Farengier)
- [![@Dangetsu](https://avatars.githubusercontent.com/u/29163698?s=64&v=4)](https://github.com/Dangetsu)
- [![@andriuhatm](https://avatars.githubusercontent.com/u/11005331?s=64&v=4)](https://github.com/andriuhatm)
- [![@lex111](https://avatars.githubusercontent.com/u/4408379?s=64&v=4)](https://github.com/lex111)
- [![@slyshkin](https://avatars.githubusercontent.com/u/19598461?s=64&v=4)](https://github.com/slyshkin)
- [![@sanmai](https://avatars.githubusercontent.com/u/139488?s=64&v=4)](https://github.com/sanmai)
- [![@Gasparchik](https://avatars.githubusercontent.com/u/7369393?s=64&v=4)](https://github.com/Gasparchik)
- [![@des1roer](https://avatars.githubusercontent.com/u/10970793?s=64&v=4)](https://github.com/des1roer)
- [![@rlshukhov](https://avatars.githubusercontent.com/u/32334495?s=64&v=4)](https://github.com/rlshukhov)
- [![@BrotifyPacha](https://avatars.githubusercontent.com/u/38500943?s=64&v=4)](https://github.com/BrotifyPacha)

[+ 7 contributors](https://github.com/roistat/php-code-conventions/graphs/contributors)

## Footer