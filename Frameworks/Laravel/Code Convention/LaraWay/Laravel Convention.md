# Laravel Convention
***
## Basic
В первую очередь следуй [PSR](https://www.php-fig.org/psr/psr-12/) и принятым соглашениям внутри команды/проекта.  
Важно поддерживать единый «code style».
***
## Functions & Variables
- camelCase
 ``` php
/* Правильно */
getPost(), $isActive, $id
/* Не правильно */
GetPost(), $is_active, $ID
```
***
## Model
- Единственное число
- Существительное
- PascalCase
- App\Models
``` php
/* Правильно */
Post, PostComment
/* Не правильно */
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
/* Правильно */
posts, post_comments
/* Не правильно */
post, postcomment, PostComments
```
***
## Pivot Table
- Единственное число
- Имена моделей в алфавитном порядке
- snake_case
``` php
/* Правильно */
post_user
/* Не правильно */
user_post, posts_users
```
***
## Column
- snake_case
- Без префикса таблицы
``` php
/* Правильно */
id, full_description, created_at
/* Не правильно */
ID, fulldescr, createdAt
```
***
## Primary Key
- Без префиксов и постфиксов
``` php
/* Правильно */
id, full_description, created_at
/* Не правильно */
ID, fulldescr, createdAt
```
***
## Foreign Key
- Имя таблицы в единственном числе
- Постфикс `_id`
``` php
/* Правильно */
user_id, post_id
/* Не правильно */
user, users_id, id_post
```
***
## Relationship hasOne
- camelCase
- Единственное число
``` php
/* Правильно */
user()
/* Не правильно */
users()
```
***
## Relationship hasMany
- camelCase
- Множественное число
``` php
/* Правильно */
users()
/* Не правильно */
user()
```
***
## Migration
- snake_case
- Название описывает действие
``` php
/* Правильно */
create_posts_table, add_user_id_to_posts_table
/* Не правильно */
posts, fix_table, update_column
```
***
## Seeder
- Единственное число
- PascalCase
- Постфикс `Seeder`
- `Database\Seeders`
``` php
/* Правильно */
PostSeeder
/* Не правильно */
Post, PostsSeeder
```
***
## Route URI
- Множественное число
- kebab-case
[REST Naming Guide](https://restfulapi.net/resource-naming/) - более подробная информация.
``` php
/* Правильно */
/posts/1, /about-us
/* Не правильно */
/post/1, /aboutUs, /about_us
```
***
## Route Name
- snake_case
- dot-нотация
``` php
/* Правильно */
posts.index, posts.show, about_us
/* Не правильно */
posts, postsShow, about.us, about-us
```
***
## Controller
- Единственное число
- PascalCase
- Постфикс «Controller»
- App\Http\Controllers
``` php
/* Правильно */
PostController, PostCommentController
/* Не правильно */
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
/* Правильно */
index, post_comments, post-comments
/* Не правильно */
Index, postComments
```
***
## Config & Language
- snake_case
``` php
/* Example */
database.php
mail.php
```
***
## Contract / Interface
- PascalCase
- Существительное или прилагательное
- Без префикса и постфикса
- App\Contracts
``` php
/* Правильно */
Authenticatable, Dispatcher, ShouldQueue
/* Не правильно */
Authentication, DispatcherInterface, IShouldQueue
```
***
## Trait
- PascalCase
- Прилагательное
- Без префикса и постфикса
- App\Traits
``` php
/* Правильно */
Notifiable, Dispatchable
/* Не правильно */
Notification, NotifiableTrait, Dispatcher
```
***
## Class
- Studly Case так же известен как PascalCase.
``` php
/* Example */
UserController, PostController
```
***
## Method
- Camel Case
``` php
/* Example */
getUserById(), createNewPost()
```
***
## Variable
- Camel Case
``` php
/* Example */
$userName, $postContent
```
***
## Conventions accepted by Laravel community

| What                                                                  | How                                                                       | Good                                    | Bad                                                             |
| --------------------------------------------------------------------- | ------------------------------------------------------------------------- | --------------------------------------- | --------------------------------------------------------------- |
| Controller                                                            | singular                                                                  | ArticleController                       | ~~ArticlesController~~                                          |
| Route                                                                 | plural                                                                    | articles/1                              | ~~article/1~~                                                   |
| Route name                                                            | snake_case with dot notation                                              | users.show_active                       | ~~users.show-active, show-active-users~~                        |
| Model                                                                 | singular                                                                  | User                                    | ~~Users~~                                                       |
| hasOne or belongsTo relationship                                      | singular                                                                  | articleComment                          | ~~articleComments, article_comment~~                            |
| All other relationships                                               | plural                                                                    | articleComments                         | ~~articleComment, article_comments~~                            |
| Table                                                                 | plural                                                                    | article_comments                        | ~~article_comment, articleComments~~                            |
| Pivot table                                                           | singular model names in alphabetical order                                | article_user                            | ~~user_article, articles_users~~                                |
| Table column                                                          | snake_case without model name                                             | meta_title                              | ~~MetaTitle; article_meta_title~~                               |
| Model property                                                        | snake_case                                                                | $model->created_at                      | ~~$model->createdAt~~                                           |
| Foreign key                                                           | singular model name with _id suffix                                       | article_id                              | ~~ArticleId, id_article, articles_id~~                          |
| Primary key                                                           | -                                                                         | id                                      | ~~custom_id~~                                                   |
| Migration                                                             | -                                                                         | 2017_01_01_000000_create_articles_table | ~~2017_01_01_000000_articles~~                                  |
| Method                                                                | camelCase                                                                 | getAll                                  | ~~get_all~~                                                     |
| Method in resource controller                                         | [table](https://laravel.com/docs/master/controllers#resource-controllers) | store                                   | ~~saveArticle~~                                                 |
| Method in test class                                                  | camelCase                                                                 | testGuestCannotSeeArticle               | ~~test_guest_cannot_see_article~~                               |
| Variable                                                              | camelCase                                                                 | $articlesWithAuthor                     | ~~$articles_with_author~~                                       |
| Collection                                                            | descriptive, plural                                                       | $activeUsers = User::active()->get()    | ~~$active, $data~~                                              |
| Object                                                                | descriptive, singular                                                     | $activeUser = User::active()->first()   | ~~$users, $obj~~                                                |
| Config and language files index                                       | snake_case                                                                | articles_enabled                        | ~~ArticlesEnabled; articles-enabled~~                           |
| View                                                                  | kebab-case                                                                | show-filtered.blade.php                 | ~~showFiltered.blade.php, show_filtered.blade.php~~             |
| Config                                                                | snake_case                                                                | google_calendar.php                     | ~~googleCalendar.php, google-calendar.php~~                     |
| Contract (interface)                                                  | adjective or noun                                                         | AuthenticationInterface                 | ~~Authenticatable, IAuthentication~~                            |
| Trait                                                                 | adjective                                                                 | Notifiable                              | ~~NotificationTrait~~                                           |
| Trait [(PSR)](https://www.php-fig.org/bylaws/psr-naming-conventions/) | adjective                                                                 | NotifiableTrait                         | ~~Notification~~                                                |
| Enum                                                                  | singular                                                                  | UserType                                | ~~UserTypes~~, ~~UserTypeEnum~~                                 |
| FormRequest                                                           | singular                                                                  | UpdateUserRequest                       | ~~UpdateUserFormRequest~~, ~~UserFormRequest~~, ~~UserRequest~~ |
| Seeder                                                                | singular                                                                  | UserSeeder                              | ~~UsersSeeder~~                                                 |
***
## Standard Laravel tools
 Лучше использовать встроенную функциональность Laravel и пакеты сообщества вместо использования сторонних пакетов и инструментов. Любому разработчику, который будет работать с вашим приложением в будущем, потребуется изучить новые инструменты. Кроме того, шансы получить помощь от сообщества Laravel значительно ниже, если вы используете сторонний пакет или инструмент.

|Task|Standard tools|3rd party tools|
|---|---|---|
|Authorization|Policies|Entrust, Sentinel and other packages|
|Compiling assets|Laravel Mix, Vite|Grunt, Gulp, 3rd party packages|
|Development Environment|Laravel Sail, Homestead|Docker|
|Deployment|Laravel Forge|Deployer and other solutions|
|Unit testing|PHPUnit, Mockery|Phpspec, Pest|
|Browser testing|Laravel Dusk|Codeception|
|DB|Eloquent|SQL, Doctrine|
|Templates|Blade|Twig|
|Working with data|Laravel collections|Arrays|
|Form validation|Request classes|3rd party packages, validation in controller|
|Authentication|Built-in|3rd party packages, your own solution|
|API authentication|Laravel Passport, Laravel Sanctum|3rd party JWT and OAuth packages|
|Creating API|Built-in|Dingo API and similar packages|
|Working with DB structure|Migrations|Working with DB structure directly|
|Localization|Built-in|3rd party packages|
|Realtime user interfaces|Laravel Echo, Pusher|3rd party packages and working with WebSockets directly|
|Generating testing data|Seeder classes, Model Factories, Faker|Creating testing data manually|
|Task scheduling|Laravel Task Scheduler|Scripts and 3rd party packages|
|DB|MySQL, PostgreSQL, SQLite, SQL Server|MongoDB|
***
## Other good practice
- Избегайте использования шаблонов и инструментов, чуждых Laravel и аналогичным фреймворкам (например, RoR, Django). Если вам нравится подход Symfony (или Spring) для создания приложений, то лучше использовать эти фреймворки.
- Никогда не вносите никакой логики в файлы route.
- Минимизируйте использование чистого PHP в шаблонах Blade.
- Используйте базу данных в памяти для тестирования.
- Не переопределяйте стандартные функции фреймворка, чтобы избежать проблем, связанных с обновлением версии фреймворка, и многих других проблем.
- По возможности используйте современный синтаксис PHP, но не забывайте о читабельности.
- Избегайте использования View Composers и подобных инструментов, если вы не знаете, что делаете. В большинстве случаев есть лучший способ решить проблему.
***
