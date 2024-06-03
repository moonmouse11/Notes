# Factory Method
***
**Фабричный метод (англ. Factory Method)** — порождающий паттерн (шаблон) проектирования, предоставляющий подклассаминтерфейс для создания экземпляров некоторого класса. В момент создания наследники могут определить, какойкласс создавать. Иными словами, Фабрика делегирует создание объектов наследникам родительского класса. Этопозволяет использовать в коде программы не специфические классы, а манипулировать абстрактными объектами наболее высоком уровне. Также известен под названием виртуальный конструктор (англ. Virtual Constructor).
Определяет интерфейс для создания объекта, но оставляет подклассам решение о том, какой классинстанциировать. Фабричный метод позволяет классу делегировать создание подклассов. Используется, когда:
- классу заранее неизвестно, объекты каких подклассов ему нужно создавать.
- класс спроектирован так, чтобы объекты, которые он создаёт, специфицировались подклассами.
- класс делегирует свои обязанности одному из нескольких вспомогательных подклассов, и планируетсялокализовать знание о том, какой класс принимает эти обязанности на себя.
###### Пример:
``` php
<?php
interface Interviewer
{
    public function askQuestions();
}

class Developer implements Interviewer
{
    public function askQuestions()
    {
        echo 'Asking about design patterns!';
    }
}

class CommunityExecutive implements Interviewer
{
    public function askQuestions()
    {
        echo 'Asking about community building';
    }
}

abstract class HiringManager
{

    // Фабричный метод
    abstract public function makeInterviewer(): Interviewer;

    public function takeInterview()
    {
        $interviewer = $this->makeInterviewer();
        $interviewer->askQuestions();
    }
}

class DevelopmentManager extends HiringManager
{
    public function makeInterviewer(): Interviewer
    {
        return new Developer();
    }
}

class MarketingManager extends HiringManager
{
    public function makeInterviewer(): Interviewer
    {
        return new CommunityExecutive();
    }
}

$devManager = new DevelopmentManager();
$devManager->takeInterview(); // Output: Спрашивает о шаблонах проектирования.

$marketingManager = new MarketingManager();
$marketingManager->takeInterview(); // Output: Спрашивает о создании сообщества.
```
Этот шаблон полезен для каких-то общих обработок в классе, но требуемые подклассы динамически определяются в ходе выполнения (runtime). То есть когда клиент не знает, какой именно подкласс может ему понадобиться.
***
## Плюсы
- позволяет сделать код создания объектов более универсальным, не привязываясь к конкретным классам(`ConcreteProduct`), а оперируя лишь общим интерфейсом (`Product`);
- позволяет установить связь между параллельными иерархиями классов.
## Минусы
- необходимость создавать наследника `Creator` для каждого нового типа продукта (`ConcreteProduct`).
- `Product` — продукт
    - определяет интерфейс объектов, создаваемых абстрактным методом;
- `ProductA`, `ProductB` — конкретный продукт
    - реализует интерфейс `Product`;
- `FactoryAbstract` — создатель
    - объявляет фабричный метод, который возвращает объект типа `Product`. Может также содержатьреализацию этого метода «по умолчанию»;
    - может вызывать фабричный метод для создания объекта типа `Product`;
- `Factory` — конкретный создатель
    - переопределяет фабричный метод таким образом, чтобы он создавал и возвращал объект класса `ConcreteProduct`.