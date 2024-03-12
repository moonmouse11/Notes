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
- `firstOrNew()` -
- `firstOrCreate()`