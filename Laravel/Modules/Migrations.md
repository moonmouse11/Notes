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
