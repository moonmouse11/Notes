# Frontend
***
## Basic
Существует два основных способа разработки фронтенда при создании приложения на Laravel:
- на основе PHP.
- c помощью JavaScript-фреймворков.
***
## Using PHP
### PHP and Blade
**Blade** – это крайне легкий язык шаблонов, который предоставляет удобный и краткий синтаксис для отображения данных, итерации по данным и многого другого:
```php
/* Пример шаблона Blade*/

<div>
    @foreach ($users as $user)
        Привет, {{ $user->name }} <br />
    @endforeach
</div>
```
При построении приложений в таком стиле, отправка форм и другие взаимодействия со страницей обычно влекут получение полностью нового HTML-документа от сервера, и страница полностью перерисовывается браузером.
## Livewire
**Laravel Livewire** - это платформа для создания фронтенда на базе Laravel, который выглядит динамичными, современными и живыми, как фронтенд, созданные с использованием современных фреймворков JavaScript, таких как Vue и React.
При использовании Livewire создаются “компоненты” Livewire, которые отображают отдельные части пользовательского интерфейса и предоставляют методы и данные, которые можно вызывать и с которыми можно взаимодействовать из фронтенда приложения.
```php
/* Пример компонента Counter используя Livewire */

namespace App\Http\Livewire;

use Livewire\Component;

class Counter extends Component
{
    public $count = 0;

    public function increment()
    {
        $this->count++;
    }

    public function render()
    {
        return view('livewire.counter');
    }
}

/* Шаблон с вызовом компонента Counter */

<div>
    <button wire:click="increment">+</button>
    <h1>{{ $count }}</h1>
</div>
```
Livewire позволяет вам добавлять новые атрибуты HTML, которые соединяют фронтенд и бэкенд.
***
## Using Vue / React
### Inertia
**Inertia** - позволяет создавать полноценные, современные интерфейсы, используя Vue или React, при этом воспользовавшись маршрутами и контроллерами Laravel для решения задач по маршрутизации, гидратации данных и аутентификации.
Inertia предоставляет поддержку рендеринга на стороне сервера. (`SSR`)
```php
/* Пример использования Inertia */

namespace App\Http\Controllers;

use App\Http\Controllers\Controller;
use App\Models\User;
use Inertia\Inertia;
use Inertia\Response;

class UserController extends Controller
{
    /**
     * Показать профиль для указанного пользователя
     */
    public function show(string $id): Response
    {
        return Inertia::render('Users/Profile', [
            'user' => User::findOrFail($id)
        ]);
    }
}
```
Страница Inertia соответствует компоненту Vue или React, обычно сохраненному в директории `resources/js/Pages` . Данные, переданные странице с помощью метода `Inertia::render`, будут использоваться для гидратации “props” компонента страницы
```vue
/* Пример использования компонента страницы */

<script setup>
import Layout from '@/Layouts/Authenticated.vue';
import { Head } from '@inertiajs/vue3';

const props = defineProps(['user']);
</script>

<template>
    <Head title="User Profile" />

    <Layout>
        <template #header>
            <h2 class="font-semibold text-xl text-gray-800 leading-tight">
                Profile
            </h2>
        </template>

        <div class="py-12">
            Hello, {{ user.name }}
        </div>
    </Layout>
</template>
```
***
## Bundling Assets
По умолчанию Laravel использует `Vite` для сборки ресурсов. `Vite` обеспечивает моментальную сборку и практически мгновенную замену модулей (`HMR`) во время локальной разработки.
- `vite.config.js` - конфигурационный файл `Laravel Vite`.
