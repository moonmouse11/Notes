# LaraWay
***
## Basic
В первую очередь следуй [PSR](https://www.php-fig.org/psr/psr-12/) и принятым соглашениям внутри команды/проекта.  
Важно поддерживать единый «code style».
***
## Functions & Variables
- camelCase
 ``` php
/* Правильно*/
getPost(), $isActive, $id
/* Не правильно*/
GetPost(), $is_active, $ID
```
***
## Model
- Единственное число
- Существительное
- PascalCase
- App\Models
``` php
/* Правильно*/
Post, PostComment
/* Не правильно*/
Posts, post_comment, Postcomment
```
***
## Table
- Множественное число
- snake_case
**Имя таблицы = имя модели во множественном числе.**
Если `$table` не указано явно в модели, то Laravel автоматически получит имя таблицы.
Даже если явно указывать имя таблицы, то всё равно рекомендуется придерживаться данного соглашения.
``` php
/* Правильно*/
posts, post_comments
/* Не правильно*/
post, postcomment, PostComments
```
***
## Pivot Table
