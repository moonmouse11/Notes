# Easy Way
***
## Basic
**Регулярное выражение** - это шаблон, сопоставляемый с искомой строкой слева направо. Термин «Регулярное выражение» сложно произносить каждый раз, поэтому, обычно, вы будете сталкиваться с сокращениями "регэкспы" или "регулярки". Регулярные выражения используются для замен текста внутри строк, валидации форм, извлечений подстрок по определенным шаблонам и множества других вещей.
***
## Example
![[Pasted image 20241221114416.png]]
Регулярное выражения выше может принимать строки `john_doe`,`jo-hn_doe` и `john12_as`. Оно не валидирует `Jo`, поскольку эта строка содержит заглавные буквы, а также она слишком короткая.
***
## Matching
В сущности, регулярное выражение - это просто набор символов, который мы используем для поиска в тексте. Например, регулярное выражение `the` состоит из буквы `t`, за которой следует буква `h`, за которой следует буква `e`.
``` 
"the" => The fat cat sat on the mat.
```
Регулярное выражение `123` соответствует строке `123`. Регулярное выражение сопоставляется с входной строкой, сравнивая каждый символ в регулярном выражении с каждым символом во входной строке по одному символу за другим. Регулярные выражения обычно чувствительны к регистру, поэтому регулярное выражение `The` не будет соответствовать строке `the`.
```
"The" => The fat cat sat on the mat.
```
***
## Meta Characters
