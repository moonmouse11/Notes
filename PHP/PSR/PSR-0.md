# PSR-0
**Deprecated** - 2014-10-21 `PSR-0` помечен как deprecated. Стандарт `PSR-4` рекомендован на замену.
***
Ниже описаны обязательные требования, необходимые для совместимости с классами автозагрузки.
# Обязательные условия
- Полностью совместимые пространство имен и класс должны иметь следующую структуру `\<Наименование производителя>\(<Пространство имен>\)*<Название класса>`
- Каждое пространство имен должно иметь пространство верхнего уровня («Наименование производителя»).
- Каждое пространство имен может иметь столько подпространств, сколько необходимо.
- Каждый разделитель пространства имен, конвертируется в `DIRECTORY_SEPARATOR` при загрузке из файловой системы.
- Каждый символ `_` в Названии_Класса (`CLASS_NAME`) конвертируется в `DIRECTORY_SEPARATOR`. Символ `_` не имеет особого значения в пространстве имен.
- К полностью совместимому пространству имен и классу во время загрузки из файловой системы добавляется расширение `.php`.
- Буквы в наименование производителя, пространствах имен, классах могут быть любым сочетанием букв нижнего и верхнего регистра.
***
# Примеры
- `\Doctrine\Common\IsolatedClassLoader` => `/путь/к/проекту/lib/vendor/Doctrine/Common/IsolatedClassLoader.php`
- `\Symfony\Core\Request` => `/путь/к/проекту/lib/vendor/Symfony/Core/Request.php`
- `\Zend\Acl` => `/path/to/project/lib/vendor/Zend/Acl.php`
- `\Zend\Mail\Message` => `/путь/к/проекту/lib/vendor/Zend/Mail/Message.php`
***
# Подчеркивание в названиях пространств имен и классов
- `\namespace\package\Class_Name` => `/путь/к/проекту/lib/vendor/namespace/package/Class/Name.php`
- `\namespace\package_name\Class_Name` => `/путь/к/проекту/lib/vendor/namespace/package_name/Class/Name.php`
Стандарты, установленные здесь, должны быть минимальным общим знаменателем для безболезненной функциональной совместимости с классами автозагрузки. Вы можете проверить, поняли ли вы эти стандарты, используя пример этого класса автозагрузки (`SplClassLoader`), реализация которого может загрузить классы PHP 5.3.
***
# Пример реализации
Ниже представлена функция для демонстрации автоматической загрузки стандартов, представленных выше.
```php
<?php

function autoload($className)
{
    $className = ltrim($className, '\\');
    $fileName  = '';
    $namespace = '';
    if ($lastNsPos = strripos($className, '\\')) {
        $namespace = substr($className, 0, $lastNsPos);
        $className = substr($className, $lastNsPos + 1);
        $fileName  = str_replace('\\', DIRECTORY_SEPARATOR, $namespace) . DIRECTORY_SEPARATOR;
    }
    $fileName .= str_replace('_', DIRECTORY_SEPARATOR, $className) . '.php';

    require $fileName;
}
```
***
