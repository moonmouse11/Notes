# Clean Code Rules
***
## Consistency
Следуйте принципам последовательного форматирования вашего кода. Стиль кода должен соответствовать стандарту `PER`, основанному на стандартах `PSR-1`, `PSR-2` и `PSR-12`, а также любым внутренним правилам вашей команды разработки.
Важно, чтобы стиль был единым по всему проекту.
Для автоматизации этого процесса вы можете использовать различные инструменты, такие как `Laravel Pint` и `PHP-CS-Fixer`. Эти инструменты помогут поддерживать согласованный стиль кода и сделают его более читаемым и легким для понимания всеми участниками команды разработки.
``` php
// Плохо ❌
class ChirpController extends Controller {
  public  function index (){
        $chirps = Chirp::with('user')->latest()->get();
        return view('chirps.index',[
'chirps' => $chirps]);
    }
    
    public function  update(Request $request , Chirp $chirp)     {
        $chirp->update($request->validated());

        return redirect()->route('chirps.index');
    }
}
```

```php
// Хорошо ✅
class ChirpController extends Controller
{
    public function index()
    {
        $chirps = Chirp::with('user')->latest()->get();

        return view('chirps.index', [
            'chirps' => $chirps,
        ]);
    }

    public function update(Request $request, Chirp $chirp)
    {
        $chirp->update($request->validated());

        return redirect()->route('chirps.index');
    }
```
***
## Naming
Хорошие имена помогают понять код и упрощают его поддержку и развитие. Например, если вы встретите такие имена в большой области контекста, то не получите понимания о том, что происходит, а после перехода в другой участок кода вам снова придётся вникать в суть переменной или метода, что займёт много времени и сил:
```php
// Переменные ❌
$data;
$var;
$info;

// Методы ❌
$user->run();
$user->handleData();
$user->process();
```
Старайтесь использовать информативные имена, которые отражают суть того, что они представляют, например:
```php
// Хорошо ✅
$user->latestPosts();
$user->sendEmail(...);
```
Использование сокращений может показаться удобным для быстрого написания кода, но они могут привести к путанице и усложнить поддержку кода.
Рассмотрим следующий пример:
```php
// Плохо ❌
$usr = User::find($id);

// Хорошо ✅
$currentUser = User::find($userId);
```
Здесь переменная `$usr` представляет объект пользователя. Однако, сокращённое имя `$usr` не даёт понимания того, что именно хранится в этой переменной. Более ясное имя, например, `$currentUser`, немедленно указывает на её предназначение.
```php
// Плохо ❌
class UsrCtrl extends Controller {
    public function f1() {
        // ...
    }
}
```
В данном примере имя класса `UsrCtrl` не информативно. Разработчику, сталкивающемуся с этим классом впервые, будет трудно понять его назначение. Название класса должно чётко отражать его функциональность, например, `ProfileController`.
```php
// Хорошо ✅
class ProfileController extends Controller
{
    public function get()
    {
        // ...
    }
}
```
Теперь рассмотрим пример именования с единицами измерений
```php
// Плохо ❌
// Мы не знаем, что представляет собой число 100
$averageTime = 100;

// Хорошо ✅
// Мы понимаем что значение имеет величину 100мс
$averageTimeInMs = 100;
```
Другой способ справиться с этим — создать специальные объекты. Представьте, что вам нужно работать с процентами.
```php
// Плохо ❌
$percentage = 0.5;
$percentage = 50;
```
Встретив такую переменную, вы не сможете сказать какое значение ожидает ваше приложение. Давайте теперь воспользуемся объектом со статическим конструктором, по одному для каждой возможности.
```php
class Percentage
{
    public static function fromInt(int $percentage): self
    {
        return new self($percentage);
    }

    public static function fromFloat(float $percentage): self
    {
        return new self($percentage * 100);
    }

    private function __construct(
        public int $value;
    ) {};
}
```
Использование класса `Percentage` поясняет, что ожидается целое число.
```php
// Хорошо ✅
$percentage = Percentage::fromFloat(0.5);
$percentage = Percentage::fromInt(50);
```
***
## Escape Magic Numbers
При написании кода важно избегать использования магических чисел, так как они могут усложнить его понимание и поддержку. Вместо этого, рекомендуется использовать именованные константы или перечисления для повышения читаемости и ясности кода.
Часто встречается ситуация, когда разработчики используют числовые значения напрямую в коде:
```php
// Плохо ❌
if ($status == 1) {
    // ...
}
```
В этом примере магическое число `1` используется для определения статуса активности. Однако, такой код может быть непонятным для других разработчиков, и в долгосрочной перспективе это усложняет поддержку и изменение кода.
Для решения этой проблемы лучше использовать именованные константы:
```php
// Хорошо ✅
const STATUS_ACTIVE = 1;

if ($status === STATUS_ACTIVE) {
    // ...
}
```
Теперь код стал более понятным и поддерживаемым. При чтении такого кода сразу становится понятно, что означает статус `1`.
Можно также использовать перечисления для явного определения различных значений:
```php
enum Status: string
    case ACTIVE = 'active';
    case INACTIVE = 'inactive';
    case ARCHIVED = 'archived';
}

$status = Status::ACTIVE;

if ($status === Status::ACTIVE) {
    // ...
}
```

```php
enum Status: int
    case ACTIVE = 1;
    case INACTIVE = 2;
    case ARCHIVED = 3;
}

$status = Status::ACTIVE;

if ($status === Status::ACTIVE) {
    // ...
}
```
Такой подход делает код более читаемым и позволяет явно указать доступные значения статуса и использовать как типизированное значение в методах, например:
```php
function myFunction(Status $status)
{
    // ...
}
```
Используя именованные константы или перечисления, мы делаем код более понятным и поддерживаемым, что важно для разработки масштабируемых приложений.
***
## Escape using `else`
При написании методов старайтесь избегать излишнего использования оператора `else`, так как это может сделать код менее читаемым и более сложным для поддержки. Вместо этого, предпочтительнее использовать более ясные и прямолинейные конструкции.
Давайте рассмотрим метод, который определяет, имеет ли доступ пользователь:
```php
// Плохо ❌
public function isUserAllowedToAccess(User $user): bool {
    if (!$user->isBanned()) {
        if ($user->isAdmin()) {
            // Пользователь не заблокирован и является администратором
            return true;
        } else {
            if($user->isGranted(GRANT::EDIT)) {
                // Пользователь не заблокирован и имеет разрешение на редактирование
                return true;
            } else {
                // Пользователь не заблокирован, но не является администратором и не имеет разрешения на редактирование
                return false;
            }
            // Недостижимый код, так как предыдущий блок уже возвращает результат
            return false;
        }
    } else {
        // Пользователь заблокирован
        return false;
    }
}
```
Использование `else` увеличивает глубину вложенности и делает код сложнее для понимания. Чтобы сделать код более простым и понятным, лучше использовать подход с ранним возвратом результата:
```php
// Хорошо ✅
public function isUserAllowedToAccess(User $user): bool
{
    if ($user->isBanned()) {
        // Пользователь заблокирован
        return false;
    }
    
    if ($user->isAdmin() || $user->isGranted(GRANT::EDIT)) {
        // Пользователь не заблокирован и является администратором или имеет разрешение на редактирование
        return true;
    }
    
    // Пользователь не заблокирован, но не является администратором и не имеет разрешения на редактирование
    return false;
}
```
Этот подход сокращает глубину вложенности и делает код более читаемым и понятным, что облегчает его поддержку и развитие.
***
## Happy way
Следует стремиться к минимизации глубины вложенности кода и предпочтительно располагать позитивные сценарии выполнения функции без вложенности. Это упрощает чтение и понимание кода, делает его более структурированным и лёгким для поддержки.
```php
// Плохо ❌
public function myFunction(User $user): void {
    if ($condition) {
        // много кода
    }

    throw new Exception;
}
```
В этом плохом примере позитивный сценарий выполнения функции сразу оказывается внутри условия, а исключение находится в основной части функции. Это делает код сложным для восприятия и усложняет его понимание.
```php
// Хорошо ✅
public function myFunction(User $user): void
{
    if (! $condition) {
        throw new Exception;
    }
    
    // много кода
}
```
В хорошем примере мы сначала проверяем условие и только затем желаемое действие. Позитивный сценарий выполнения функции оказывается в основной части функции, что делает код более читабельным и легким для понимания.