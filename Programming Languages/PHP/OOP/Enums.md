# Enums
***
## Theory
**Enum** - перечисляемый тип данных.
### Magic Constants
- `__CLASS__` — магическая константа, которая ссылается на имя enum из enum.
- `__FUNCTION__` — в контексте метода enum.
- `__METHOD__` — в контексте метода enum.
### Cancelled Magic Methods
- `__get()` - чтобы предотвратить сохранение состояния в объектах enum.
- `__set()` - чтобы предотвратить присвоение динамических свойств и поддержания состояния.
- `__construct()` - перечисления не поддерживают все конструкции `new Foo()`.
- `__destruct()` - перечисления не должны поддерживать состояние.
- `__clone()` - перечисления — это неклонируемые объекты.
- `__sleep()` - перечисления не поддерживают методы жизненного цикла.
- `__wakeup()` - перечисления не поддерживают методы жизненного цикла.
- `__set_state()` - чтобы предотвратить принуждения состояния к объектам enum.

_**Одно из наиболее важных различий между enum и классом заключается в том, что enums не могут иметь состояния. Объявление свойств для перечислений запрещены, также не допускаются статические свойства.**_
_**Перечисления, не поддерживаемые значением, автоматически реализуют интерфейс UnitEnum.
Перечисления не могут явно реализовать этот интерфейс, поскольку это делается внутри движка. Это делается только для помощи в определении типа данных enum. Метод UnitEnum :: cases возвращает массив всех случаев данного enum.
Перечисления поддерживают пространства имён, автозагрузку, методы, но не свойства, реализующие интерфейсы и другое поведение PHP-классов.**_

***
## Pure enum
``` php
// Pure enum
enum Color {
    case Red;
    case Black;
    case White;
}

// Enum со скалярными данными.
enum Color: string {
    case Red = "R";
    case Black = "B";
    case White = "W";
}

Color::Red->value // Получение свойства с enum.

Color::from("R"); // Получение объекта Enum через значение.
Color::tryFrom("NULL"); // Безопасное получение объекта Enum через значение.
Color::cases(); // Вывод всех значение Enum.
```
***
## Enum methods & implementations
``` php
// Пример использования Trait и Interface в Enum
interface Colorful {
  public function color(): string;
}

trait Rectangle {
  public function shape(): string {
    return "Rectangle";
  }
}

enum Suit implements Colorful {
  use Rectangle;

  case Hearts;
  case Diamonds;
  case Clubs;
  case Spades;

  public function color(): string {
    return match($this) {
      Suit::Hearts, Suit::Diamonds => 'Red',
      Suit::Clubs, Suit::Spaces => 'Black',
    };
  }
}
```
***
