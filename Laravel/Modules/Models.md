# Models
***
## Basics
**Модель** - программный модуль, служащий для вазимодействия с определенной базой данных.  Для извлечения значений полей, добавления, правки и удаления записей. 
Так же модель предоставляет прямой доступ к построителю запросов, посредством которого производится выборка записей.
Отдельный объект модели хранит значения полей отдельной записи таблицы. Через объект можно обратиться к значениям полей через одноименные свойства и предоставляет ряд методов для обработки записи.
***
## Creating Model
Для создания модели рекомендуется воспользоваться командой `php artisan make:model [model_name]`.
Класс модели объявляется в пространстве имен `App\Models` и наследуется от класса `Illuminate\Database\Eloquent\Model`. Так же использует трейт `Illuminate\Database\Eloquent\Factories\HasFactory` - используется при работе в фабриками.
Модуль работает в соответсвии со следующими соглашениями по умолчанию:
- База данных - задействуется указанная в настройках таблица.
- Обслуживаемая моделью таблица - должна иметь имя, совпадающее с именем класса модели во множественном числе. _**(Если имя класса модели состоит из нескольких слов, набранных вплотную, без пробелов между ними в PascalCase - имя таблицы должно представлять собой то же имя в snake_case)**_.
- id - имя ключевого поля.
- Тип ключевого поля - целочисленный автоинкремент.
- Поля отметок создания и правки.
- Имя поля отметки создания.
- Имя поля отметки правки.
- Формат записи временных отметок в эти поля - используемый по умолчанию.
***
## Model Parameters
#### Параметры полей таблицы заносятся в защищенные свойства класса модели:
- `protected $fillable = array` - массив с именами полей, доступных для заполнения.
- `protected $guarded = array` - массив с именами полей, заблокированных для заполнения.
- `protected $attributes = assoc_array` - ассоциативный массив, со значениями по умолчанию, заносимыми в поля сразу при создании записи. Ключи ассоциативного массива должны соответсвовать полям, значения значениям.
#### Параметры обслуживаемой таблицы:
- `protected $connection = string` - имя базы данных, из числа приведенных в настройках проекта.
- `protected $table = string` - имя таблицы.
- `protected $primaryKey = string` - имя первичного ключа.
- `protected $keyType = string` - наименование типа ключевого поля.
- `public $incrementing = bool` - является ли первичный ключ автоинкрементом. 
- `public $timestamps = bool` - в таблице есть поля создания и изменения записи.
- `protected $dateFormat = string` - формат записи даты.
- `const CREATED_AT = string` - имя поля отметки создания. 
- `const UPDATED_AT = string` - имя поля отметки изменения.
``` php
// Пример использования наследуемых полей модели.
class Post extends Model {
	protected $connection = 'pgsql',
	protected $table = 'post_table',
	protected $primaryKey = 'post_id',
	protected $keyType = 'uuid',
	public $incrementing = false,
	protected $dateFormat = 'U',
	const CREATED_AT = 'added',
	const UPDATED_AT = 'update',
}
```
#### Параметры преобразования типов:
Перед выдачей значения, считанного из базы данных, Laravel преобразует его в подходящий для PHP тип. 
Параметры преобразования типов задаются в защищенном свойства `casts`. Значение свойства - ассоциативный массив.
``` php
use Illuminte\Database\Eloquent\Casts\AsStringable;
// Пример работы со свойством casts
class Post extends Model {

	protected $casts = [
		'post_name' => AsStringable::class,
		'post_number' => 'integer',
	];
}
```
Примеры преобразования типов:
- `string` - обычная строка.
- `Illuminte\Database\Eloquent\Casts\AsStringable` - объект класса `Stringable`.
- `int`, `integer` - целочисленный тип.
- `float`, `double`, `real` - вещественное число.
- `decimal[:count_number]` - вещественное число высокой точности.
- `datetime[:date_format]`, `custom_datetime[:date_format]` - объект класса `Carbon`. Временная отметка. Можно указать формат, в котором извлеченное из базы отметка будет сериализовываться.
- `timestamp` - временная отметка формата `UNIX`.
- `date[:date_format]` - тоже что и `datetime`, но только дата без времени.
- `immutable_datetime[:date_format]`, `immutable_custom_datetime[:date_format]` - неизменяемое значение времени.
- `immutable_date[:date_format]` - неизменяемое значение даты.
- `bool`, `boolean` - логический тип.
- `EnumType::class` - перечисления `enum`, указывается полное имя класса.
``` php 
// Пример использования EnumType
enum PostType: int {
	case BUY = 1;
	case SELL = 2;
}

class Post extends Model {
	// Применение типа в casts
	protected $casts = ['post_type' => PostType::class];
}
```
- `array`, `json` - массив, преобразовываются `JSON` данные.
- `Illuminte\Database\Eloquent\Casts\AsArrayObject` - объект класса `ArrayObject`.
- `object` - объект класса `stdClass`.
- `collection`, `Illuminte\Database\Eloquent\Casts\AsCollection` - объект класса `Collection`.
- `encrypted` - зашифрованное элементарное значение. 
- `encrypted:array` - зашифрованный объект `ArrayObject`.
- `encrypted:object` - зашифрованный объект `stdClass`.
- `encrypted:collection` - зашифрованная коллекция `Collection`.
***
## Creating Relations
### 1. One To Many
Для создания между моделями связи "один ко многим", в классах объявляются два публичных метода:
1. `hasMany(string foreign_class_name, string foreign_field_name = null, string primary_field_name = null)` - метод, создающий связь с со вторичной моделью. _**(Обычно имя метода совпадает с имененем связанной таблицы)**_. _Если второй аргумент не передан методу, Laravel предполагает, что зависимая таблица содержит поле внешнего ключа в формате - `[primary_table_name]_id`. Если не задан третий агрумент методу - будет использовано поле `id` или заданное в свойстве `protected $primaryKey`.
2. `belongsTo(string primary_class_name, string foreign_key_name = null, string primary_key_field = null)` - метод, создающий связь с первичной моделью. _**(Обычно имя метода совпадает с имененем связанной таблицы)**_ 
###### Пример создания связи "Один ко многим" c названиями полей по умолчанию.
``` php
// Миграция для первой таблицы.
Schema::create('categories', function (Blueprint $table){
	$table->id(); // Primary Key
	...
});

// Миграция для второй таблицы
Schema::create('posts', function (Blueprint $table){
	...
	$table->foreignId('category_id')->constrained()->cascadeOnDelete();
	...
});

class Category extends Model {
	public function posts() : Illuminate\Database\Eloquent\Relations\HasMany
	{
		return $this->hasMany(App\Models\Post::class);
	}
}

class Post extends Model {
	public function category(): Illuminate\Database\Eloquent\Relations\BelongsTo
	{
		return $this->belongsTo(App\Models\Category::class);
	}
}
```
###### Пример создания связи "Один ко многим" c названиями полей _**не**_ по умолчанию.
``` php
// Миграция для первой таблицы.
Schema::create('categories', function (Blueprint $table){
	$table->string('title')->primary();
	...
});

// Миграция для второй таблицы
Schema::create('posts', function (Blueprint $table){
	...
	$table->string('category', 50);
	$table->foreign('category')->references('title')->on('category')
		->cascadeOnDelete();
	...
});

class Category extends Model {
	public function posts() : Illuminate\Database\Eloquent\Relations\HasMany
	{
		return $this->hasMany(App\Models\Post::class, 'category', 'title');
	}
}

class Post extends Model {
	protected $primaryKey = 'title';
	protected $keyType = 'string';
	public $incrementing = false;
	
	public function category(): Illuminate\Database\Eloquent\Relations\BelongsTo
	{
		return $this->belongsTo(App\Models\Category::class, 'category', 'title');
	}
}
```
### 2.  One To One from Many
Связь "один с одним из многих" при обращении со стороный первичной модели из всех имеющихся связанных записей вторичной модели выдаст лишь одну, соотвествующую указанному критерию.
Такая связь создается так же, как и прямая связь "один со многими". Но с двумя отличиями:
1. В методе, формирующим связь со вторичной моделью - связь создается не методом  `hasMany()`. Используется метод `hasOne()`. _**(Сигнатуры метода аналогичны)**_.
2. В методе,  формирующим связь с первичной моделью - в зависимости от логики используются методы:
	- `latestOfMany(string field_name = 'id')` - возвращает запись, в котором поле с указанным в агрументе метода полем, имеет максимальное значение. _(По умолчанию поле `id`)_
	- `oldestOfMany(string field_name = 'id')` - обратна методу выше,  возвращает запись, в котором поле с указанным в агрументе метода полем, имеет минимальное значение. _(По умолчанию поле `id`)_
	- `ofMany(string field_name = 'id', string aggregation_function = 'MAX')`, `ofMany(array fields_names, callback aggregation_function)` - метод позволяет указать более сложные условия для агрегирования записей. 
###### Пример создания связи "Один с одним из многих".
``` php
use Illuminate\Database\Eloquent\Relations\HasOne;

class Category extends Model
{
	...
	// Метод выдает последний добавленный пост.
	public function latestPost(): HasOne
	{
		return $this->hasOne(Post::class)->latestOfMany();
	}
	...
	// Второй способ по дате создания поста.
	public function latestDatePost(): HasOne
	{
		return $this->hasOne(Post::class)->ofMany('created_at', 'MAX');
	}
	...
	// Третий способ - агрегирование по двум полям.
	public function latestDateAuthorPost($author_id): HasOne
	{
		return $this->hasOne(Post::class)->ofMany(
			['created_at' => 'MAX', 'author' => $author_id],
			function ($querry) {
				$querry->whereMonth('created_at', now()->month);
			}
		);
	}
} 
```
## 3. One To One
Для создания связи "Один к одному" нужно объявить два публичных метода. 
1. `hasOne(string model_class_name)` - метод для связывания со вторичной таблицей. _(Обычно имя метода совпадает с названием вторичной таблицы)_. 
2. `belongsTo(string model_class_name)` - аналогичен методу для связи "Один ко многим".
###### Пример создания связи "Один к одному".
``` php
// Миграция
Schema::create('accounts', function (Blueprint $table) {
	...
	$table->foreign_id('user_id')->constained()
		->cascadeOnDelete();
	...
})

// Модель User
use App\Models\Account;
use Illuminate\Database\Eloquent\Relations\HasOne;

class User extends Model
{
	...
	public function account(): HasOne
	{
		return $this->hasOne(Account::class);
	}
	...
}

// Модель Account
use App\Models\Account;
use Illuminate\Database\Eloquent\Relations\BelongsTo;

class Account extends Model
{
	...
	public function user(): BelongsTo
	{
		return $this->hasOne(User::class);
	}
	...
}
```
## 4. Many to Many
Связь "Многие ко многим" реализуется в три этапа:
1. Создать связующую таблицу, содержащую два поля внешнего ключа. (По соглашению имена этих таблиц должны соответсвовать формату `[model_name]_id`. Сама связующая таблица по соглашению должна иметь имя `[model_name_a]_[model_name_b]` расположенные в алфавитном порядке). _(Создавать модель для связующей таблицы необязательно)._
2. Обявить в связываемых моделях метод `belongsToMany()`. (Как правило его имя совпадает с именем второй связываемой таблицы).
	- `belongsToMany(string second_model_class_name, string relation_table_name = null, string relation_field_name = null, string second_relation_field_name = null, string key_field_name = null, string secondary_key_field_name = null): BelongsToMany`  - метод формирует связь между двумя связующими таблицами.
3. Объявить метод `belongsToMany()` во второй связующей таблице.
###### Пример создания связи "Многие к многим" с соответсвием полей.
``` php
// Миграции
Schema::create('machines', function (Blueprint $table) {
	$table->id();
	$table->string('title', 30);
	$table->timestamps();
});

Schema::create('spares', function (Blueprint $table) {
	$table->id();
	$table->string('title', 30);
	$table->timestamps();
});
// Миграция связующей таблицы
Schema::create('machine_spare', function (Blueprint $table) {
	$table->foreignId('machine_id')->constrained()->cascadeOnDelete();
	$table->foreignId('spare_id')->constrained()->cascadeOnDelete();
});

// Модель Машины
use App\Models\Spare;
use Illuminate\Database\Eloquent\Relations\BelongsToMany;

class Machine extends Model
{
	...
	public function spares(): BelonsToMany
	{
		return $this->belongsToMany(Spare::class);
	}
}

// Модель Детали
use App\Models\Machine;
use Illuminate\Database\Eloquent\Relations\BelongsToMany;

class Spare extends Model
{
	...
	public function machines(): BelonsToMany
	{
		return $this->belongsToMany(Machine::class);
	}
}
```
###### Пример создания связи "Многие к многим" без соответсвия полей.
``` php
// Миграции
Schema::create('machines', function (Blueprint $table) {
	$table->id('michine_id');
	$table->string('title', 30);
	$table->timestamps();
});

Schema::create('spares', function (Blueprint $table) {
	$table->id('spare_id');
	$table->string('title', 30);
	$table->timestamps();
});
// Миграция связующей таблицы
Schema::create('ms', function (Blueprint $table) {
	$table->foreignId('machine')->constrained('michines', 'michine_id')
		->cascadeOnDelete();
	$table->foreignId('spare')->constrained('spares', 'spare_id')
		->cascadeOnDelete();
	$table->text('some_very_important_info');
});

// Модель Машины
use App\Models\Spare;
use Illuminate\Database\Eloquent\Relations\BelongsToMany;

class Machine extends Model
{
	protected $primaryKey = 'michine_id';
	...
	public function spares(): BelonsToMany
	{
		return $this->belongsToMany(Spare::class, 'ms', 'machine', 'spare'
			'michine_id', 'spare_id');
	}
}

// Модель Детали
use App\Models\Machine;
use Illuminate\Database\Eloquent\Relations\BelongsToMany;

class Spare extends Model
{
	protected $primaryKey = 'spare_id';
	...
	public function machines(): BelonsToMany
	{
		return $this->belongsToMany(Machine::class, 'ms', 'spare', 'machine'
			'spare_id', 'michine_id');
	}
}
```
Каждый объект связанной модели поддерживает свойство `pivot`, хранящее объект с записью связующей таблицы. 
Объект генерируется фреймворком Laravel и хранит поля с ключами связанных записей. (Если связущая модель имеет дополнительные поля, они в объект `pivot` по умолчанию не заносятся).
- `withPivot(array add_pivot_fields)` - метод указывает дополнительные поля связующей таблицы, которые должны содержаться в проекте из свойства `pivot`.
- `withTimestamps(string created_at_name = null, string updated_at_null)` - метод указывает обрабатывать поля отметок создания и правки записи таблицы, и добавляет их в объект свойства `pivot`.
- `as(string field_name)` - метод псевдоним, задает другое имя для свойства, хранящего объект с записью таблицы (вместо `pivot`).
###### Пример создания связи "Многие ко многим" со свойством `pivot`.
``` php
public function manyWithPivots(): BelongsToMany
{
	return $this->belongsToMany(Class:class)->as('connector')
		->withPivot('some_very_important_info')->withTimestams();
}
```
## Using Relation Models
**Pivot** - связующая модель, используемая при связи **"Многие ко многим"**.
Имеет следущие отличия от обычной:
- Наследуется от класса `Illuminate\Database\Eloquent\Relations\Pivot`.
- Не содержит втоикрементный ключ. `public $incrementing = false,`
- Все поля модели доступны для массового присваивания. `protected $guarded = []`.
Если связующая таблица не содержит полей создания и обновления записи, рекомендуется обозначить свойство `public $timestams = false`.
Связи с двумя моделями создаются с помощью метода `belongsTo`
###### Пример применения метода `belongsTo()`
``` php
// Пример использования метода belongsTo()
namespace App;

use Illuminate\Database\Eloquent\Relations\BelongsTo;
use Illuminate\Database\Eloquent\Relations\Pivot;
use App\Models\Machine;
use App\Models\Spare;

class MachineSpare extends Pivot 
{
	public $timestamps = false;

	public function machine(): BelongsTo
	{
		return $this->belongsTo(Machine::class);
	}

	public function spare(): BelongsTo
	{
		return $this->belongsTo(Spare::class);
	}
}
```
При создании сввязи в связываемых моделях следует явно указать, что для обработки связующей таблицы должна использоваться связующая модель, и задать имена полей, который должны присутствовать в модели pivot.
Связующая модель задается в  методе `using(string relation_model_class_name)`. Извлекаемые из связующей модели поля задаются в методе `withPivot(string field_name)`.
###### Пример применения метода `using()`
``` php
use App\Models\MachineSpare;

class Machine extends Model
{
	...
	public function spares(): BelongsToMany
	{
		return $this->belongsToMany(Spare::class)->using(MachineSpare::class)
			->withPivot('some_very_important_info');
	}
}
```
## Signing Models
По умолчанию при правке или удалении записей вторичной модели Laravel не помечает связанную с ними запись первичной моедли как исправленную, обновляя в не значение поля отметки правки. Чтобы указать фреймворку помечать в таких случаях запись первичной модели как исправленную следуюет:
- Объявить во вторичной модели `protected $touches`
- Присвоить этому свойству массив с именами обратных связей.
###### Пример применения свойства `touches`
``` php
class Post extends Model
{
	protected $touches = ['category'];
}
```
## One To Many Through
Фреймворк Laravel позволяет создать сквозную связь "Один со многими" между моделями через помежуточную.
Для этого в начальной модели объявляется метод, создающий такую связь. (Обычно его имя совпадает с именем связанной конечной таблицы).
- `hasManyThrough(string final_model_class_name, middle_model_class_name, string middle_model_foreing_key_field_name = null, string final_model_foreing_key_field_name = null, primary_model_field_name = null, string middle_model_key_field_name = null)` - метод для создания сквозной связи "Один ко многим".
###### Пример создания сковзной связи "Один ко многим".
``` php 
// Пример сощдания "One To Many Through"
use App\Models\Post;
use App\Models\Offer;

class Category extends Model
{
	...
	public function offer(): HasManyThrough
	{
		return $this->hasManyThrough(Offer::class, Post::class);
	}
}
```
## One To One Through
Сквозная связь "Один к одному" создается аналогично сквозной связи "Один со многими".
- `hasOneThrough(string final_model_class_name, middle_model_class_name, string middle_model_foreing_key_field_name = null, string final_model_foreing_key_field_name = null, primary_model_field_name = null, string middle_model_key_field_name = null)` - метод для создания сквозной связи "Один к одному".
###### Пример создания сквозной связи "Один к одному".
``` php
// Пример сощдания "One To One Through"
use App\Models\Post;
use App\Models\Offer;

class Category extends Model
{
	...
	public function offer(): HasOneThrough
	{
		return $this->hasOneThrough(Offer::class, Post::class);
	}
}
```
## Plug Models
Записи заглушки - нужны для закрытия `null` при отсутствии связанной записи в таблице. 
Можно настроить фреймворк Laravel выдавать запись-заглушку, представляющую собой объект связанной модели. 
- `withDefault(array fields_values = null | callback function = null)` - метод для создания записи  заглушки.
###### Пример создания записи-заглушки.
``` php
use Illuminate\Database\Eloquent\Relations\BelongsTo;
use App\Models\User;

class Post extends Model
{
	// Пример создания записи по умолчанию.
	public function user(): BelongsTo
	{
		return $this->belongsTo(User::class)->withDefault();
	}

	// Пример создания записи массивом.
	public function user(): BelongsTo
	{
		return $this->belongsTo(User::class)
			->withDefault(['name' => 'admin']);
	}

	public function user(): BelongsTo
	{
		return $this->belongsTo(User::class)
			->withDefault(function ($user, $bb) {
				$user->name = 'admin' . config('app.name');
			});
	}
}
```
## Closed Relation
Фреймворк Laravel позволяет создать закнутые связи - в которых таблица ссылается сама на себя.
Пример - использование вложенности с помощью поля `parent_id`.
###### Пример создания замкнутой связи.
``` php
// Пример создания поля 'parent_id' в таблице
Schema::create('categories', function (Blueprint $table) {
	$table->bigInteger('parent_id');  
	$table->foreign('parent_id')->references('id')  
	    ->on('categories')->restrictOnDelete();
});

use Illuminate\Database\Eloquent\Relations\BelongsTo;  
use Illuminate\Database\Eloquent\Relations\HasMany;

// Пример создания замкнутой связи в модеди.
class Category extends Model
{
	public function categories(): HasMany  
	{  
	    return $this->hasMany(self::class, 'parent_id');  
	}  
  
	public function parent(): BelongsTo  
	{  
	    return $this->belongsTo(self::class, 'parent_id');  
	}
}
```
***
## Model Methods
В классе модели можно объявлять методы, реализующие какой-либо функционал. 
- `find()` - статический метод для поиска объекта по заданным параметрам.
- `save()` - статический метод для сохранения записи в БД.
- `delete()` - статический метод, удаляет запись из БД.
- `create()` - статический метод, создает объект модели.
***
## Accessors & Mutator
Преобразование типов полей между моделями `Eloquent` и базой данных.
Реализовать _**"преобразователь"**_ можно в классе модели, с помощью защищенного метода. (Имя преобразователя должно созвпадать с именем поля в таблице) Метод возвращает объект класса `Illuminate\Database\Eloquent\Casts\Attribute`. Создавать его следует с помощью статического метода `make()`.
Метод `make()` принимает два параметра:
- `get` - **(accessor)** - анонимная функция, преобразующая значение перед выдачей коду. Принимает в качестве параметра значение, извлеченное из БД.
- `set` - **(mutator)** - анонимная функция, преобразующая значение перед занесением в базу данных. Принимает в параметрах, значение из фреймворка.
###### Пример создания "преобразователя" в модели.
``` php
use Illuminate\Database\Eloquent\Casts\Attribute;

// Пример создания "преобразователя".
class User extends Authenticatable  
{
	protected function name(): Attribute  
	{  
	    return Attribute::make(  
	        get: fn ($value) => ucfirst($value),  
	        set: fn ($value) => strtolower($value)  
	    );
	}
}
```
Значения "преобразователя" можно отправить с кэш с помощью метода `shouldCache()` **(сокращает количество высичлений)**
***
## Virtual Fields
