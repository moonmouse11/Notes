# Seeders
***
**Сидер** - модуль фреймворка Laravel, заполняющий таблицы базы данных указанным содержимым.
**Сидеры** - реализуются в виде подклассов класса `Illuminate\Database\Seeder`.
Объявляются в пространстве имен - `Database\Seeders`.
Скрипты хранятся в директории - `database/seeders`.
``` php
// Пример кода корневого сидера.
namespace Database\Seeders;

use Illuminate\Database\Console\Seeds\WithoutModelEvents;
use Illuminate\Database\Seeder;
use Illuminate\Facades\Hash;
use App\Models\User;

class DatabaseSeeder extends Seeder
{
	use WithoutModelEvents;
	
	public function run(): void
	{
		// Создание пользователя с помощью сидера.
		User::create([
			'name' => 'admin', 
			'email' => 'admin@admin.com',
			'password' => Hash::make('admin'),
			// Another model fields
		]);
	}
}
```
#### Root seeder (Корневой сидер)
**Корневой сидер** - основной сидер, запускаемый утилитой `artisan`. Генерируется при создании нового проекта и носит имя `DatabaseSeeder`.
Дополнительно можно создать произвольное количество подчиненных сидеров, запускаемых из боевого.