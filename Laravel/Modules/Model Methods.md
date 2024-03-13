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
