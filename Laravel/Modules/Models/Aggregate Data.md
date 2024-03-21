# Aggregate Data
***
## Get Data
Объект модели, хранящий выбранную запись, предоставляет набор свойств, содержащих значения отдельных полей этой записи. 
Именя этих свойств совпадают с именами полей таблицы.
Пример кода работы с моделью.
``` php
use App\Models\Catagory;

$category = Category::first();

$title = $category->title;
$id = $category->id;
$update = $category->updated_at;
```
Обращение к свойству, чье имя совпадает с именем метода, создающего "прямую" связь - возвращает коллекцию связанных записей вторичной таблицы, представленных в виде объектов соответсвующей модели.
###### Связь "Один со многими".
``` php
// Посмотрим все объявления из рубрики "Дома" 
$category = Category::firstOrNew(['name' => 'Дома']); 
// В модели Category "прямую" связь создает метод posts(), поэтому для получения коллекции связанных объявлений обращаемся к свойству posts
foreach ($category->posts as $post) {
	echo $post->title;
}
```
Вызов метода, создающий прямую связь, возвращает объект "прямой" связи. (Логично). Возвращается один объект - не коллекция.
###### Связь "Один со одним из многих".
``` php
use App\Models\Category;
// Пример реализации. Да совсем не очевидный.
$category = Category::firstOrNew(['title' => 'Category title']);
echo $category->latestPost->title . ': ' . $category->latestPost->content;

echo $category->latestMinPricePost->title . ': ';
echo $category->latestMinPricePost->content;
```
###### Связь "Один к одному".
Оба объекта связанных записей поддерживают свойства, одноименные создающие связи методам.
``` php
use App\Models\User;

// Пример реализации связи "Один к одному".
$user = User::firstOrNew(['name' => 'admin']);
echo $user->account->first_name . ' ' . $user->account->last_name;

// Еще один пример.
use App\Models\Account;

$account = Account::firstOrNew(['last_name' => 'Last name example']);
echo $account->user->name . ': ' . $account->user->email;
```
###### Связь "Многие со многими".
Используются методы, создающие связи и одноименные им свойста.
``` php
// Пример реализации связи "Многие со многими".
use App\Models\Machine;

$machine = Machine::first();
echo $machine->name;
// Парсинг и вывод деталей машины $machine
foreach ($machine->spares as $spare) {
	echo $spare->name;
}
foreach ($machine->spares->orderBy('name', 'desc')->get() as $spare) {
	echo $spare->name;
}

use App\Models\Spare;
$spare->Spare::firstOrNew(['title' => 'nail']);
foreach ($spare->machines as $machine) {
	echo $machine->name;
}
```
Объект связанной записи поддерживает свойство, хранящее объект с записью связующей таблицы. По умолчанию это свойство имеет имя `pivot` - можно изменить с помощью метода `as()`. _**(`pivot` хранит свойства, объявленные в методе модели `withPivot`).**_
``` php
// Парсинг полей с помощью свойства pivot.
foreach ($machine->spares as $spare) {
	echo $spare->name . ' - ' . $spare->pivot->cnt . ' count.';
}
```
***
## Aggregate records. Basics
Получить объект построителя запросов можно через модель. Наследуемые методы.
###### Get all data.
- `all(array|string $columns = ['*']): Collection` - статический метод, возвращает все записи из БД с указанными в параметре полями.
``` php
use App\Models\Category;
// Пример использования метода all()
$categories = Category::all(['name']);
foreach ($categories as $category) {
	echo $category->name;
}
```
- `get(array|string $columns = ['*'])` - метод  возвращвет записи из базы данных. 
``` php
// Пример и разница между all() и get()
$activeCustomers = Customer::where('active', 1)->get(); //query builder
$activeCustomers = Customer::all()->where('active', 1); //Laravel collection
```
###### Get first record.
Методы для извлечения первой записи из таблицы базы данных.
- `first(array|string $columns = ['*'])` - статический метод, возвращает первую запись из БД с указанными в параметре полями.
- `firstOrFail(array|string $columns = ['*'])` - метод возвращает первую запись из базы данных с указанными полями, или возвращает ошибку.
- `sole(array|string $columns = ['*'])` - метод аналогичен методу выше, но если найдено больше одной записи в базе данных - выбрасываетс исключение.
- `firstOr(Closure|array|string $columns = ['*'], Closure $callback = null)` - аналогичен методу `first`. В случае отсутвия записей в таблицу возвращается результат, заданный callback функцией в параметрах.
###### Searching records.

