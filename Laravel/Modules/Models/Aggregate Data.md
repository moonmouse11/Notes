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
``` php
// Посмотрим все объявления из рубрики "Дома" 
$category = Category::firstOrNew(['name' => 'Дома']); 
/** В модели Rubric "прямую" связь создает метод bbs(), поэтому для получения коллекции связанных объявлений обращаемся к свойству bbs */
foreach ($category->posts as $post) {
	echo $post->title;
}
```
Вызов метода, создающий прямую связь, возвращает объект "прямой" связи. (Логично). Возвращается один объект - не коллекция.