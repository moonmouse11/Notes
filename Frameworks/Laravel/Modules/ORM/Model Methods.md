# Model Methods
***
## Create Record
Добавить в таблицу новую запись с помощью модели можно тремя способами:
1. Создание объекта модели, представляющего пустую запись. Внесение значений в отдельные поля объекта.
``` php
// Пример создания записи первым способом.
use App\Models\Category;

// Создаем объект класса модели с помощью конструктора.
$category = new Category();

// Заполняем свойства объекта.
$category->title = 'Category title';
$category->parent_id = null;

// Сохраняем запись с помощью метода Save
$category->save();
```
- `save(array settings): bool` - метод для сохранения объекта в базу данных. Возвращает `true` при успешном сохранении записи.
	- `array settings = ['touch' => bool]` - добавляет запись в базу данных без изменения отметок `created_at` и `updated_at`.
2. Создание записи и заполнение свойств объекта с помощью контруктора.
``` php
// Пример создания записи вторым способом.
use App\Models\Category;

// Создаем объект класса модели с помощью конструктора.
$category = new Category([
	'title' => 'Category title',
	'parent_id' => null,
]);

// Сохраняем запись с помощью метода Save
$category->save();
```
3. Создание и заполнение записи с помощью метода `create()`
- `create(array model_fields): model_object` - создает объект и добавляет его в базу данных. _**(Метод использует построитель запросов под капотом)**_
``` php
// Пример создания записи третьим способом.
use App\Models\Category;

$category = Category::create([
	'title' => 'Category title',
	'parent_id' => null
]);
```
- `firstOrNew(array search_model_fields, array new_model_fields)` - ищет запись, чьи поля соответсвует значениям массива. Если запись не найдена - создает новый объект.
- `firstOrCreate(array search_model_fields, array new_model_fields)` - ищет запись, чьи поля соответсвуют значениям массива. Если запись не найдена - создает новую в базы данных.
***
## Update Record
- `fill(array model_fields)` - метод для массового присваивания значений объекту модели.
- `update(array modles)_fields)` - метод для массового присваивания значений объекту модели и сохранения изменений с базе данных. `fill()` + `save()`.
- `updateOrCreate(array search_model_fields, array update_model_fields)` - ищет запись, чьи поля соответвуют значениям массива, и обновляет ее с сохранением значений в базе данных. Если запись не найдена - создает и сохраняет новую. 
``` php
// Пример использования метода updateOrCreate

Category::updateOrCreate(['title' => 'Category title', 'parent_id' => 1],
						['title' => 'Category new title', 'parent_id' => 1]);
```
### Update record fields
- `increment(string filed_name, int new_value = 1, array another_values = null)` - метод увеличивает значение указанного числового поля. 
- `decrement(string filed_name, int new_value = 1, array another_values = null)` - уменьшает значение указанного числового поля.
- `touch(string update_time_field_name)` - заносит текущую временную метку в поле обновления `updated_at` и сохраняет запись.
- `isDirty(string filed_name = null | ... strings field_names = null | array field_names = null): bool` - метод проверяет запись на изменения полей перед сохранением. Возвращает `true` если были изменения.
- `isClean(string filed_name = null | ... strings field_names = null | array field_names = null): bool` - противоположен методу выше.
- `wasChanged(string filed_name = null | ... strings field_names = null | array field_names = null): bool` - метод определяет изменились ли значения в полях записи после ее сохранения во время обработки текущего запроса.
- `getOriginal(string field_name = null)` - метод возвращает изначальное значение всего объекта или указанного поля.
***
## Delete Record
- `delete(): bool` - метод удаляет запись из базы данных.
- `destroy(... string record_key | array record_keys)` - метод для массового удаления записей. 
- `restore()` - метод для восстановления "мягко" удаленной записи.
- `trashed(): bool` - возвращает `true` если запись была "мягко" удалена.
- `forceDelete()` - полностью удаляет запись из базы данных.
***
## Work with relations
- `associate(object relation_model)` - метод для создания обратной связи между объектами.
- `saveMany(array relation_objects)` - методя связывает все записи, содержащиеся в переданном массиве.
- `dissociate()` - метод для удаления связи  между объектами.
- `createMany(array relation_objects)` - метод создает новые записи, заносит в поля каждой их них значения из переданного массива и связывает их.
- `push()` - метод для сохранения текущей и связанных записей в базу данных.
- `attach(string record_key, array fields_value = null | array keys)` -  метод для связывания объектов "Один ко многим". 
- `sync(array records_keys, bool disconnect_records = true)` -  связывает с текущей записью объекты по указанным ключам. Второй параметр для сохранения старых связей с записью.
- `syncWithoutDetaching(array records_keys)` - аналогично методу выше, без отсоединения старых связанных записей.
- `syncWithPivotWalues(array keys, array values, bool disconnect_records = true)` - метод связывает с текущей записью переданные в аргумент записи, и заносит данные в дополнительные поля.
- `toggle(array records_keys)` - перебирает заданный массив с ключами записей, и проверяет связанна ли текущая запись с переданными. Если запись связана - то ее отсоединяют. Если нет - наоборот.
- `updateExistingPivot(string record_key, array record_fields)` - обновляет значения указанной связующей записи.
***
## Copy Records
- `replicate(array ignoring_fields)` - метод копирует текущую запись, игнорируя заполнение переданных в параметрах полей.
``` php
$category = Category::firstOrNew(['name' => 'Грузовой']);

// Создает первую запись
$post1 = Post::create([
	'title' => 'ЗИЛ', 
	'content' => 'Старый, ржавый', 
	'address' => 'Писать мне', 
	'price' => 1000000, 
	'rubric_id' => $rubric->id, 
	'user_id' => $user->id
]);

// Создаем сторую запись на основе первой 
$post2 = $post1->replicate(['content', 'price']);
// Заносим новые значения в поля скопированной записи.
$bb2->fill(['content' => 'Новый', 'price' => 10000000]);
$bb2->save();
```
***
## Mass Work With Records
Для добавления, правки и удаления большого количества данных рекомендуется использовать простроитель запросов `DB`, для повышения быстродействия. 
Его методы можно вызвать у класса модели как статические, либо у объекта модели как обычные.
При использовании `DB` 
- Поля `created_at` и `updated_at` - не заполняются и не изменяются.
- Методы `save()` и `delete()` - не выполняются.
- События моделей не генерируются.
### Mass Creating Records
Для массового добавления записией применяются методы:
- `insert(array fields_value): bool` - метод для массового добавления записей в таблицу. Методв выдаст ошибку при добавлении записей с индентичным уникальным ключом.
``` php
// Пример использования метода insert
$category1 = Category::firstOrNew(['name' => 'Легковой']);
Post::insert([
	'title' => 'Запорожец', 
	'content' => 'Старый, ржавый, сильно битый', 
	'address' => 'На помойке', 
	'price' => 10000, 
	'rubric_id' => $category1->id, 
	'user_id' => $user->id
]); 

$category2 = Category::firstOrNew(['name' => 'Грузовой']); 
// Добавляем сразу две записи 
Post::insert([
	[
		'title' => 'МАЗ', 
		'content' => 'Старый, заслуженный', 
		'address' => 'На стоянке', 
		'price' => 4000000, 
		'rubric_id' => $category2->id, 
		'user_id' => $user->id
	], 
	[
		'title' => 'ГАЗ', 
		'content' => 'Совсем новый', 
		'address' => 'На стоянке', 
		'price' => 70000000, 
		'rubric_id' => $category2->id, 
		'user_id' => $user->id
	]
]);
```
- `insertOrIgnore(array fields_value)` - аналогичен методу выше, но при добавлении записей с одинаковыми ключами - ошибка будет проигнорировна.
- `insertUsing(array fields_value, inner_querry)` - метод добавляет записи, извлеченные переданным в аргументы "вложенным запросом".
``` php
// Пример работы метода insertUsing()
OldPost::insertUsing(
	['title', 'content', 'address', 'price', 'rubric_id', 'user_id'], 
	Post::select('title', 'content', 'address', 'price', 'rubric_id', 'user_id') 
		->where('created_at', '<', now()->subYears(10)) 
);
```
- `insertGetId(array record_fields, string counter_name)` - добавляет запись и возвращает ее сегенерированный идентификатор. Второй параметр для PostgreSQL - указывается имя счетчика для генерации идентификатора.
### Mass Updating Records
- `update(array record_fields)` - метод "построителя запросов" для массового обновления записей.
- `where(string field_name, mixed field_value)` - метод для поиска записей по указанным в аргументах параметрам.
- `updateOrInsert(array search_model_fields, array update_model_fields)` - метод аналогичен методу `updateOrCreate`. Только в качестве результата возвращает объект `DB`.
``` php
// Пример использования метода updateOrCreate
use Illuminate\Support\Facades\Hash; 
User::updateOrInsert([
	'email' => 'editor@bboard.ru', 
	'name' => 'editor'], 
	['password' => Hash::make('editor')
]);
```
- `upsert(array need_fields, array search_fields, array updateing_fields)` - метод исправляет записи моделей, удовлетворяющие заданным условиям.
``` php
// Пример использования метода upsert
User::upsert(
[ 
	 [
		 'email' => 'editor@bboard.ru', 
		 'name' => 'editor', 
		 'password' => Hash::make('supereditor')
	],
	[
		'email' => 'trainee@bboard.ru', 
		'name' => 'trainee', 
		'password' => Hash::make('12345')
	] 
], 
['email'], 
['password']
);
```
### Mass Deleting Records
- `delete(): int` - метод для массовго удаления записей. В качестве результата возвращает количество удаленных записей. _**(Если вызвать на коллекции без фильтрации - удалит все записи модели).**_
- `truncate()` - метод удаляет все записи таблицы и сбрасывает счетчик автоинкремента.
***
## DB Facade
`Illuminate\Support\Facades\DB`
Объект построителя запросов. Этот фасад пригодится, если необходимо добавить в таблицу данные, но связанная модель еще не создана. 
Методы `DB`, необходиые для работы:
- `connection(string database_name)` - метод для указания базы данных, с которой предстоит работать.
- `table(string table_name)` - метод для указания таблицы, с которой предстоит работать.
``` php
use Illuminate\Support\Facades\DB; 
// Добавляем запись в таблицу rubrics базы данных по умолчанию 
DB::table('categories')->insert(['title' => 'Техника']);  
// Добавляем запись в таблицу offers базы данных mysql 
DB::connection('mysql')->table('offers')->insert(['many_data_values']);
```
***
