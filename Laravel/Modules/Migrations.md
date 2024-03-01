# Migrations
***
## Basics
_**Миграция**_ - это скрипт, вносящий в структуру базы данных заданные изменения. Миграция может создать таблицу со всеми необходимыми полями, индексами и связями, добавить в уже существущую таблицу индекс или поле, изменить или удалить таблицу.
С любой миграцией можно сделать два действия:
- `up` - применение, при котором миграция вносит в БД заданные изменения. *(Применение миграций проходит в хронолигическом порядке)*
- `down` - откат, при котором миграция возвращает базу данных в изначальное состояние, существовашее перед применением миграции. *(Откат выполняется в обратном хронолигичком порядке)*
После применения миграции Laravel заносит данные в журнал миграций. Таблица миграций создается автоматически. Имя таблицы можно установить в конфигурационном файле. Свойство - `database.migrations`.
Формат именования файлов мигарции:
`<year>_<month>_<date>_<hours>_<minutes>_<seconds>_<migration_name>.php`
_**В имени миграции отдельные слова должны разделяться символом  `_`**_
По умолчанию все операции, изменяющие структуру базы данных, по возможности выполняются в транзакции. Что бы указать Laravel не выполнять их в транзакции нужно изменить свойство.
```php
public $withinTransaction = false; // По умолчанию true
```
`Illuminate\Support\Facades\Schema` - класс для работы со структурой базы данных.
***
## Creating Tables.
- `create(string table_name, callback Blueprint)` - метод для создания таблицы.
Анонимная функция принимает объект класса `Illuminate\Database\Schema\Blueprint`. 
### Creating fields.
Код, создающий поля новой таблицы, создаются в анонимной функции, переданной вторым параметром.
Поля создаются методами объекта структуры таблицы.
#### Schema methods.
- `string(string field_name, int field_length = null)` - строковое поле `VARCHAR`, хранящее строку ограниченной длины. Если не указан второй параметр - задается длинна статического свойства `Schema::defaultStringLength(int string_length)`.
- `char(string field_name, int field_length = null)` - строковое поле типа `CHAR`, хранящее строку фиксированной длины. 
- `text(string field_name)` - текстовое поле с предельным объемом 65536 символов. Формат `TEXT`.
- `mediumText(string field_name)` - текстовое поле с предельным объемом 16 777 216 сиволов. Формат `MEDIUMTEXT`.
- `longText(string field_name)` - текстовое поле с предельным объемом 4 294 967 296 символов.
- `integer(string field_name, boll autoincrement = false, bool unsigned = false)` - целочисленное поле размером 4 байта.
- `unsignedInteger(string field_name, bool autoincrement)` - беззнаковое целочисленное поле размером 4 байта. _**(Беззнаковые числовые поля поддерживаются только MySQL)**_
- `bigInteger(string field_name, boll autoincrement = false, bool unsigned = false)` - знаковое числовое поле размером 8 байт.
- `unsignedBigInteger(string field_name, bool autoincrement)` - беззнаковое целочисленное поле размером 8 байт.
- `mediumInteger(string field_name, boll autoincrement = false, bool unsigned = false)` - знаковое числовое поле размером 3 байта.
- `unsignedMediumInteger(string field_name, bool autoincrement)` - беззнаковое целочисленное поле размером 3 байта.
- `smallInteger(string field_name, boll autoincrement = false, bool unsigned = false)` - знаковое числовое поле размером 2 байта.
- `unsignedSmallInteger(string field_name, bool autoincrement)` - беззнаковое целочисленное поле размером 2 байта.
- `tinyInteger(string field_name, boll autoincrement = false, bool unsigned = false)` - знаковое числовое поле размером 1 байт.
- `unsignedTinyInteger(string field_name, bool autoincrement)` - беззнаковое целочисленное поле размером 1 байт.
- `float(string field_name, int digit_length = 8, int digit_after_point_length = 2, unsigned = false)` - знаковое дробное (вещественное) число обычной точности 4 байта. _**(Не все СУБД поддерживают указание общего количества цифр и количество цифр после запятой)**_
- `unsignedFloat(string field_name, int digit_length = 8, int digit_after_point_length = 2)` - беззнаковое дробное (вещественное) число обычной точности 4 байта.
- `double(stirng field_name, int digit_length = 8, int digit_after_point_length = 2, unsigned = false)` - знаковое дробное (вещественное) число двойной точности, размером 8 байт.
-  `unsignedDouble(string field_name, int digit_length = 8, int digit_after_point_length = 2)` - беззнаковое дробное (вещественное) число двойной точности, рамером 8 байт.
- `decimal(string field_name, int digit_length = 8, int digit_after_point_length = 2, unsigned = false)` - дробное (вещественное) число высокой точности. 
- `unsignedDecimal(string field_name, int digit_length = 8, int digit_after_point_length = 2)` - беззнаковое дробное (вещественное) число высокой точности.
- `dateTime(string field_name, int precision = 0)` - временная отметка. Формат `DATETIME`. Параметр точность (`precision`) указывает количество цифр после запятой, отводимых для хранения долей секунд.
- `dateTimeTz(string field_name, int precision = 0)` - то же, что и метод выше, но с учетом временной зоны.
- `timestamp(string field_name, int precision = 0)` - временная отметка TIMESTAMP. Параметр точность (`precision`) указывает количество цифр после запятой, отводимых для хранения долей секунд.
- `timestampTz(string field_name, int precision = 0)` - то же, что и метод выше, но с учетом временной зоны.
- `timestamps(int precision = 0)` - метод создает необязательные для заполнения поля типа TIMESTAMP для хранения отметок создания и правки с именами:
	- `created_at` - timestamp создания записи в таблице.
	- `updatetd_at` - timestamp узменения записи в таблице.
- `nullableTimestamps(int precision = 0)` - то же, что и метод выше.
- `timestampsTz(int precision = 0)` - то же, что и метод выше, с учетом временной зоны.
- `date(string field_name)` - метод создает поле для хранения даты, формат DATE.
- `time(string field_name, int precision = 0)` -  метод для создани поля формата TIME.
- `timeTz(string field_name, int precision = 0)` - аналогичен методу выше, с учетом временных зон.
- `year(string field_name)` - создает поле формата `YEAR`.
- `boolean(string field_name)` - создает поле формата `BOOLEAN`.
- `bigIncrements(string field_name)` - ключевое автоинкрементное беззнаковое целочисленное поле размером 8 байт. Формат `UNSIGNED BIGINT`. Так же автомитически создает индекс по этому полю.
- `id(string field_name = id)` - alias (псевдоним) метода выше. По умолчанию создает поле с именем `id`.
- `increments(string field_name)` - ключевое автоинкрементное беззнаковое целочисленное поле размером 4 байта. Формат `UNSIGNED INTEGER`.
- `mediumIncrements(string field_name)` - ключевое автоинкрементное беззнаковое целочисленное поле размером 3 байта. Формат `UNSIGNED MEDIUMINT`.
- `smallIncrements(string field_name)` - ключевое автоинкрементное беззнаковое целочисленное поле размером 2 байта. Формат `UNSIGNED SMALLINT`.
- `tinyIncrements(string field_name)` - ключевое автоинкрементное беззнаковое целочисленное поле размером 1 байта. Формат `UNSIGNED TINYINT`.
- `uuid(string firld_name)` - создает поля для  универсального уникального идентификатора. Формат `UUID`.
- `rememberToken()` - метод создает поле для хранения токена "remember me". Формат `VARCHAR(100)`.
- `enum(string field_name, array valid_values)` - перечисляемый тип. Любое строковое значение из заданного массива. _**(Поле перечисления).**_ Формат `ENUM`.
- `set(string field_name, array valid_values)` - перечисляемый тип. Произвольное количество любых строковых значений. _**(Поле набора).**_ Формат `SET`.
- `json(string field_name)` - метод создает поле в формате `JSON`.
- `jsonb(string field_name)` - метод создает поле в формате `JSONB`.
- `binary(string field_name)` - метод создает поле в формет двоичных данных. Формат `BLOB`.
- `ipAddress(string field_name)` - создает поле для хранения IP-адреса. Формат `VARCHAR`.
- `macAddress(string field_name)` - создает поле для хранения MAC-адреса. _**(У PostgreSQL свой формат для хранения этого значения. Остальные используют `VARCHAR`).**_
- `geometry(string field_name)` - создает поле, для ~~описания геометрической фигуры~~. Формат `GEOMETRY`.
- `point(string field_name)` - создает поле, для ~~описания геометрической точки~~. Формат `POINT`.
- `lineString(string field_name)` - создает поле, для ~~описания геометрической линии~~. Формат `LINESTRING`.
- `polygon(string field_name)` - создает поле, для ~~описания геометрического полигона~~. Формат `POLYGON`.
- `geometryCollection(string field_name)` - создает поле ~~набора описаний геометрических фигур~~. Формат `GEOMETRYCOLLECTION`.
- `multiPoint(string field_name)` - набор ~~описаний геометрических точек~~.Формат `MULTIPOINT`.
- `multiLineString(string field_name)` - набор ~~описаний геометрических линий~~. Формат `MULTILINESTRING`.
- `multiPolygon(string field_name)` - набор ~~~~описаний геометрических полигонов~~. Формат `MULTIPOLYGON`.
Все методы возвращают объект класса `\Illuminate\Database\Schema\ColumnDifinition`  представляющий описание данного поля.
***
## Soft Deletes
Создание в таблице поля отметки "мягкого" удаления выполняется вызовом метода класса _**`Blueprint`**_
- `softDeletes(string field_name = deleted_at, int precision = 0)` - отметка удаления. Формат `TIMESTAMP`. 
- `softDeletesTz(string field_name = deleted_at, int precision = 0)` - аналогично методу выше с утетом временных зон.t
***
## Add fields parameters.
Для указания дополнительных параметров полей используются методы:
- `default(mixed value)` - задает для текущего поля значение по умолчанию.
```php
// Пример кода с использование default()
$table->decimal('price', 10, 2)->default(0);
// Пример с выражением SQL
use Illuminate\Database\Qeury\Expression;

$table->float('random')->default(new Expression('RAND()'));
```
- `nullable(bool nullable = true)` - превращает текущее поле в необязательное _**(Может принимать значение `NULL`)**_ если предано `true`. Обратное поведение с `false`.
- `useCurrent()` - задает для текущего поля временной отметки в качестве значения по умолчанию текущее дату и время.
- `useCurrentOnUpdate()` - аналогична методу выше, записывает текущее значение времени при каждом обновлении строки.
- `autoincrement()` - превращает текущее целочисленное поле в автоинкремент.
- `from(started value` - указывает у автоинкрементного поля заданное "начальное значение" _**(Только MySQL & PostgreSQL)**_
- `storedAs(new Illuminate\Database\Qeury\Expression(expression))` - позволяет рассчитывать значение по умолчанию на основе заданного SQL выражения. _**(Только MySQL & PostgreSQL)**_
``` php
// Пример функции storedAs
$table->decimal('total', 10, 2)->storedAs('`price` * `count`');
```
- `comment(string comment)` - добавляет полю заданный строковый комментарий. _**(Только MySQL & PostgreSQL)**_
- `virtualAs(new Illuminate\Database\Qeury\Expression(expression))` - превращает поле в простое вычисляемое _**(Только MySQL)**_
- `unsigned()` - превращяет целочисленное поле в беззнаковое _**(Только MySQL)**_
- `invisible()` - превращает текущее поле в невидимое, и его значение не будет извлекаться при выполенении `SELECT`. _**(Только MySQL)**_
- `charset(string charset_code)` - указывает полю заданную в методе кодировку. _**(Только MySQL)**_
- `collation(string sorting_querry)` - указывает последовательность сортировки с заданным обозначением. _**(Только MySQL)**_
- `first()` - помещает поле в начало таблицы. _**(Только MySQL)**_
- `after(string field_name)` - помещает поле в тадлице после заданного. _**(Только MySQL)**_
- `generatedAs(string expression)` - переводит числовое поле в поле идентификации. Заполняется только при отсутвии явного заполнения. _**(Только PostgreSQL)**_
``` php
// Пример метода generatedAs
$table->unsignedBigInteger('id')->generatedAs();
// Добавляем дополнительные параметры.
$table->unsignedBigInteger('id')->generatedAs('start with 10 increment by 5');
// Пример с методом ниже.
$table->unsignedBigInteger('id')->generatedAs()->always();
```
- `always()` - превращяет поле идентификации в заполняемое принудительно. _**(Только PostgreSQL)**_
- `isGeometry()` - указывает полю тип `GEOMETRY` вместо `GEOGRAPHY` по умолчанию. _**(Только PostgreSQL)**_
***
## Creating indexes
Методы для создания индексов в таблице:
- `index()` - метод добавляет обычный инекс. Поддреживает три формата вызова
1. `index(string index_name = null, string algorythm = null)` - создает именованный индекс с указанным алгоритмом. _**(Создание индекса с разным алгоритмом поддерживают не все СУБД)**_
``` php
// Пример применения метода index
$table->unsignedTinyInteger('order')->index();
$table->string('name', 40)->index('idx_name', 'hash');
```
2. `index(string index_field_name, string index_name = null, string algorythm = null)` - метод вызываетя у объекта, представляющего структуру создаваемой таблицы.
```php
// Пример использования
$table->string('name', 40);
$table->index('name');
```
3. `index(array index_fileds_names, string index_name = null, string algorythm = null)` - создает составной индекс по нескольким полям.
``` php
// Пример создания составного индекса
$table->unsignedTinyInteger('order');
$table->string('name', 40);
$table->index(['name', 'order']);
```
- `unique(string index_name = null, string algorythm = null)` - создает уникальный индекс. Формат вызова аналогичен методоу `index()`.
- `primary(string index_name = null, string algorythm = null)` - создает первичный ключ. Формат вызова аналогичен методоу `index()`. _(Следует вызывать у не автоинкрементных полей)_
- `fullText(string index_name = null, string algorythm = null)` - создает полнотекстовый индекс. Формат вызова такой же как у метода `index()`. _**(Только MySQL & PostgreSQL)**_ 
- `spatialIndex(string index_name = null)` - создает пространственный индекс. _**(Только MySQL)**_
- `rawIndex(new Illuminate\Database\Qeury\Expression(expression), string index_name)` - создает индекс на основе SQL выражения. 
``` php
use \Illuminate\Database\Query\Expression;

$table->string('name', 40);
$table->rawIndex(new Expression('upper(name)'), 'index_name');
```
***
## Creating Foreign Key
