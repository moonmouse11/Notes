# Laravel Convention
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
- Единственное число
- Имена моделей в алфавитном порядке
- snake_case
``` php
/* Правильно*/
post_user
/* Не правильно*/
user_post, posts_users
```
***
## Column
- snake_case
- Без префикса таблицы
``` php
/* Правильно*/
id, full_description, created_at
/* Не правильно*/
ID, fulldescr, createdAt
```
***
## Primary Key
- Без префиксов и постфиксов
``` php
/* Правильно*/
id, full_description, created_at
/* Не правильно*/
ID, fulldescr, createdAt
```
***
## Foreign Key
- Имя таблицы в единственном числе
- Постфикс `_id`
``` php
/* Правильно*/
user_id, post_id
/* Не правильно*/
user, users_id, id_post
```
***
## Relationship hasOne
- camelCase
- Единственное число
``` php
/* Правильно*/
user()
/* Не правильно*/
users()
```
***
## Relationship hasMany
- camelCase
- Множественное число
``` php
/* Правильно*/
users()
/* Не правильно*/
user()
```
***
## Migration
- snake_case
- Название описывает действие
``` php
/* Правильно*/
create_posts_table, add_user_id_to_posts_table
/* Не правильно*/
posts, fix_table, update_column
```
***
## Seeder
- Единственное число
- PascalCase
- Постфикс `Seeder`
- `Database\Seeders`
``` php
/* Правильно*/
PostSeeder
/* Не правильно*/
Post, PostsSeeder
```
***
## Route URI
- Множественное число
- kebab-case
[REST Naming Guide](https://restfulapi.net/resource-naming/) - более подробная информация.
``` php
/* Правильно*/
/posts/1, /about-us
/* Не правильно*/
/post/1, /aboutUs, /about_us
```
***
## Route Name
- snake_case
- dot-нотация
``` php
/* Правильно*/
posts.index, posts.show, about_us
/* Не правильно*/
posts, postsShow, about.us, about-us
```
***
## Controller
- Единственное число
- PascalCase
- Постфикс «Controller»
- App\Http\Controllers
``` php
/* Правильно*/
PostController, PostCommentController
/* Не правильно*/
PostsController, Post, posts_controller
```
***
## Resource Controller / CRUD
Стандартные CRUD методы: 
- index
- create
- store
- show
- edit
- update
- destroy.
[Документация](https://laravel.com/docs/10.x/controllers#actions-handled-by-resource-controller) - более подробная информация.
Также, если необходимо, то ты можешь добавлять свои методы, но чтобы избежать конфликтов, их роуты должны быть определены **до** ресурсного контроллера.
***
## View
- kebab-case
- snake_case
- Без точек в имени
Нет чёткого стандарта использовать **«kebab-case»** или **«snake_case»**. Главное используй один вариант во всем проекте.
Точки используются для разделения директорий, поэтому в имени их быть не должно, кроме расширения `.blade.php`
Данное соглашение касается как файлов, так и директорий.
``` php
/* Правильно*/
index, post_comments, post-comments
/* Не правильно*/
Index, postComments
```
***
## Config & Language
- snake_case
***
## Contract / Interface
- PascalCase
- Существительное или прилагательное
- Без префикса и постфикса
- App\Contracts
``` php
/* Правильно*/
Authenticatable, Dispatcher, ShouldQueue
/* Не правильно*/
Authentication, DispatcherInterface, IShouldQueue
```
***
## Trait
- PascalCase
- Прилагательное
- Без префикса и постфикса
- App\Traits
``` php
/* Правильно*/
Notifiable, Dispatchable
/* Не правильно*/
Notification, NotifiableTrait, Dispatcher
```
***
