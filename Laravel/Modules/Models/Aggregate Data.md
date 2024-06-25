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
Методы для поиска записей в базе данных:
- `find($key, $default = null)` - метод ищет запись по заданному ключу.
- `findMany(Arrayable|array $ids, array|string $columns = ['*'])` - метод ищет записи, по заданному ключу.
- `findOrFail(mixed $id, array|string $columns = ['*'])` - метод ищет запись по заданному ключу, если запись не найдена - выбрасывается исключение `ModelNotFoundException`.
- `findOrNew(mixed $id, array|string $columns = ['*'])` - метод ищет запись по заданному ключу, если запись не найдена - создает новую, с полями из аргументов.
- `firstWhere(Closure|string|array|Expression $column, mixed $operator = null, mixed $value = null, string $boolean = 'and')` -  метод ищет первую запись, удовлетворяющую заданному условию. Возвращает объект найденной записи.
###### Filter records.
- `where(Closure|string|array|Expression $column, mixed $operator = null, mixed $value = null, string $boolean = 'and')` - метод отбирает записи, удовлетворяющие заданным в параметрах условиям. Формирует SQL-запрос с командой `WHERE`.
``` php
// Пример использования метода where()
// SELECT * FROM posts WHERE price >= 1000;
$posts = Post::where('price', '>=', 1000)->get();

// SELECT * FROM posts WHERE price >= 1000 AND category_id = 1;
$posts = Post::where('price', '>=', 1000)
	->where('category_id', 1)->get();

// SELECT * FROM posts WHERE price >= 1000 OR category_id = 1;
$posts = Post::where('price', '>=', 1000)
	->where('category_id','=', 1, 'or')->get();
// Another way
$posts = Post::where(['price', '>=', 1000],
					['category_id', '=', 2, 'or'])->get();

// SELECT * FROM posts WHERE category_id = 1 OR (price >= 1000 AND price <= 5000);
$posts = Post::where('category_id', 1)
	->where(static function ($query) {
		$query->where('price', '>=', 1000)
			->where('price', '<=', 5000);
	}, null, null, or)->get();
```
- `orWhere(Closure|array|string|Expression $column, mixed $operator = null, mixed $value = null)` - аналогичен методу выше, только задаваемое условие объединяется с предыдущим с использованием логического опертатора `OR`. 
``` php
// Пример использования метода orWhere()
// SELECT * FROM posts WHERE price >= 1000 OR category_id = 1;
$posts = Posts::where('price', '>=', 1000)
	->orWhere('category_id', '=', 1)->get();
```
- `whereNot(Closure|string|array|Expression $column, mixed $operator = null, mixed $value = null, string $boolean = 'and')` - метод отбирает записи, не подходящие под заданные в параметрах условия.
``` php
// Пример использования метода whereNot()
$posts = Posts::whereNot('price', '>=', 10000000)->get();
```
- `orWhereNot(Closure|array|string|Expression $column, mixed $operator = null, mixed $value = null)` - метод фильтрует записи, не подходящие под переданные с параметры и объединяет условие с предыдущим с использованием логического опертатора `OR`.
- `whereColumn(Expression|string|array $first, string|null $operator = null, string|null $second = null, string|null $boolean = 'and')` - метод фильтрует записи аналогично методу `where()`, только сравнивает значение одного поля со значением другого поля.
``` php
// Пример использования метода whereColumn()
// SELECT * FROM posts WHERE 'created_at' = 'updated_at';
$posts = Posts::whereColumn('created_at', 'updated_at')->get();
```
- `orWhereColumn(Expression|string|array $first, string|null $operator = null, string|null $second = null)` -
- `whereDate(Expression|string $column, DateTimeInterface|string|null $operator, DateTimeInterface|string|null $value = null, string $boolean = 'and')` -
- `orWhereDate(Expression|string $column, DateTimeInterface|string|null $operator, DateTimeInterface|string|null $value = null)` -
- `whereDay(Expression|string $column, DateTimeInterface|string|int|null $operator, DateTimeInterface|string|int|null $value = null, string $boolean = 'and')` - 
- `orWhereDay(Expression|string $column, DateTimeInterface|string|int|null $operator, DateTimeInterface|string|int|null $value = null)` - 
- `whereMonth(Expression|string $column, DateTimeInterface|string|int|null $operator, DateTimeInterface|string|int|null $value = null, string $boolean = 'and')` -
- `orWhereMonth(Expression|string $column, DateTimeInterface|string|int|null $operator, DateTimeInterface|string|int|null $value = null)` -
- `whereTime(Expression|string $column, DateTimeInterface|string|null $operator, DateTimeInterface|string|null $value = null, string $boolean = 'and')` -
- `orWhereTime(Expression|string $column, DateTimeInterface|string|null $operator, DateTimeInterface|string|null $value = null)` -
- `whereBetween(Expression|string $column, iterable $values, string $boolean = 'and', bool $not = false)` -
- `whereNotBetween(Expression|string $column, iterable $values, string $boolean = 'and')` -
- `orWhereBetween(Expression|string $column, iterable $values)` -
- `orWhereNotBetween(Expression|string $column, iterable $values)` -
- `whereBetweenColumns(Expression|string $column, array $values, string $boolean = 'and', bool $not = false)` -
- `whereNotBetweenColumns(Expression|string $column, array $values, string $boolean = 'and')` - 
- `orWhereBetweenColumns(Expression|string $column, array $values)` -
- `orWhereNotBetweenColumns(Expression|string $column, array $values)` - 
- `whereIn(Expression|string $column, mixed $values, string $boolean = 'and', bool $not = false)` - 
- `whereIntegerInRaw(string $column, Arrayable|array $values, string $boolean = 'and', bool $not = false)` -
- `whereNotIn(Expression|string $column, mixed $values, string $boolean = 'and')` - 
- `whereIntegerNotInRaw(string $column, Arrayable|array $values, string $boolean = 'and')` -
- `orWhereIn(Expression|string $column, mixed $values)` - 
- `orWhereIntegerInRaw(string $column, Arrayable|array $values)` - 
- `orWhereNotIn(Expression|string $column, mixed $values)` - 
- `orWhereIntegerNotInRaw(string $column, Arrayable|array $values)` - 
- `whereNull(string|array|Expression $columns, string $boolean = 'and', bool $not = false)` - 
- `whereNotNull(string|array|Expression $columns, string $boolean = 'and')` - 
- `orWhereNull(string|array|Expression $column)` - 
- `orWhereNotNull(Expression|string $column)` - 