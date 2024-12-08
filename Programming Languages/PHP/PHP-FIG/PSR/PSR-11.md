# PSR-11
***
## Container interface
В этом документе описывается общий интерфейс для контейнеров внедрения зависимостей.
Цель `ContainerInterface`— стандартизировать то, как фреймворки и библиотеки используют контейнер для получения объектов и параметров ( в остальной части документа называемых _записями )._
Ключевые слова `«ОБЯЗАН», «НЕ ОБЯЗАН», «НЕОБХОДИМ», «ЖЕЛАТЕЛЕН», «НЕ ЖЕЛАТЕЛЕН», «ДОЛЖЕН», «НЕ ДОЛЖЕН», «РЕКОМЕНДОВАН», «ВЕРОЯТЕН», «ОПЦИОНАЛЕН»` в этом документе `СЛЕДУЕТ` интерпретировать в соответствии с [RFC 2119](http://www.ietf.org/rfc/rfc2119.txt).
В этом документе это слово `implementor` следует толковать как то, что кто-то реализует `ContainerInterface`в библиотеке или фреймворке, связанных с внедрением зависимостей. Пользователи контейнеров внедрения зависимостей (`DIC`) называются `user`.
***
## Specification
#### Entry identifiers 
**Entry identifiers (Идентификатор записи)** — это любая допустимая PHP строка, состоящая как минимум из одного символа, которая однозначно идентифицирует элемент внутри контейнера. 
**Идентификатор записи** — это непрозрачная строка, поэтому вызывающие `НЕ ДОЛЖНЫ` предполагать, что структура строки несет какое-либо семантическое значение.
#### Reading from a container
- Предоставляет `Psr\Container\ContainerInterface`два метода: `get`и `has`.
- `get`принимает один обязательный параметр: идентификатор записи, который `ДОЛЖЕН` быть строкой. `get`может возвращать что угодно ( _смешанное_ значение) или выдавать исключение, `NotFoundExceptionInterface`если идентификатор неизвестен контейнеру. Два последовательных вызова `get`с одним и тем же идентификатором `ДОЛЖНЫ` возвращать одно и то же значение. Однако в зависимости от `implementor` конструкции и/или `user`конфигурации могут возвращаться разные значения, поэтому `user` `НЕ СЛЕДУЕТ` полагаться на получение одного и того же значения при двух последовательных вызовах.
- `has`принимает один уникальный параметр: идентификатор записи, который ДОЛЖЕН быть строкой. `has` `ДОЛЖЕН` возвращать, `true` если идентификатор записи известен контейнеру, и `false`если нет. Если `has($id)`возвращает false, `get($id)` `ДОЛЖЕН` выдать `NotFoundExceptionInterface`.
#### Exceptions
Исключения, напрямую выбрасываемые контейнером, `ДОЛЖНЫ` реализовывать `Psr\Container\ContainerExceptionInterface`.
Вызов метода getс несуществующим идентификатором `ДОЛЖЕН` вызвать исключение `Psr\Container\NotFoundExceptionInterface`.
#### Recommended usage
Пользователи `НЕ ДОЛЖНЫ` передавать контейнер в объект, чтобы объект мог получить _свои собственные зависимости_ . Это означает, что контейнер используется как `Service Locator`, что является шаблоном, который обычно не рекомендуется.
***
## Package
Описанные интерфейсы и классы, а также соответствующие исключения предоставляются как часть пакета `psr/container`.
Пакеты, предоставляющие реализацию контейнера PSR, должны декларировать, что они предоставляют `psr/container-implementation` `1.0.0`.
Проекты, требующие реализации, должны потребовать `psr/container-implementation` `1.0.0`.
***
## Interfaces
### `Psr\Container\ContainerInterface`
```php
namespace Psr\Container;

/**
 * Describes the interface of a container that exposes methods to read its entries.
 */
interface ContainerInterface
{
    /**
     * Finds an entry of the container by its identifier and returns it.
     *
     * @param string $id Identifier of the entry to look for.
     *
     * @throws NotFoundExceptionInterface  No entry was found for **this** identifier.
     * @throws ContainerExceptionInterface Error while retrieving the entry.
     *
     * @return mixed Entry.
     */
    public function get($id);

    /**
     * Returns true if the container can return an entry for the given identifier.
     * Returns false otherwise.
     *
     * `has($id)` returning true does not mean that `get($id)` will not throw an exception.
     * It does however mean that `get($id)` will not throw a `NotFoundExceptionInterface`.
     *
     * @param string $id Identifier of the entry to look for.
     *
     * @return bool
     */
    public function has($id);
}
```
### `Psr\Container\ContainerExceptionInterface`
```php
namespace Psr\Container;

/**
 * Base interface representing a generic exception in a container.
 */
interface ContainerExceptionInterface
{
}
```
### `Psr\Container\NotFoundExceptionInterface`
```php
namespace Psr\Container;

/**
 * No entry was found in the container.
 */
interface NotFoundExceptionInterface extends ContainerExceptionInterface
{
}
```
***
