# The Power of 10: Rules for Developing Safety-Critical Code
***
## Basic
1. Нужно сильно ограничивать ветвления и условия. Не использовать `goto`, `setjmp` или `longjmp`, не использовать прямую или косвенную рекурсию.    
2. У всех циклов должен быть предел. Проверяющая программа должна иметь возможность легко доказать, что определенное количество итераций не может быть превышено. Если предел невозможно доказать статически, то правило считается нарушенным.
3. Не использовать динамическое распределение памяти после инициализации.
4. Любая функция должна уместиться на одном стандартном листе бумаги, одно выражение на строку и одна строка на определение. Обычно это означает, что функция не должна быть длиннее 60 строк.
5. Должно быть не более двух ассертов на функцию. Ассерты используются для проверки аномальных условий, которые не могут произойти при реальном запуске. Ассерты не должны содержать сайд-эффектов, и по формату должны быть Boolean-тестами. Когда ассерт падает, должно запуститься специальное действие по восстановлению, например, возврат условия падения обратно в вызывающую функцию. Если проверяющая программа доказывает, что ассерт никогда не фейлится или никогда не удовлетворяется, то правило считается нарушенным. (Нельзя обойти это правило с помощью бессмысленных “assert(true)”).
6. Объекты с данными должны быть задекларированы на самом низком (из возможных) уровне области видимости.
7. Возвращаемое значение не-void функции должно проверяться вызывающей функцией. Валидность параметров должна проверяться внутри каждой функции.
8. Препроцессор можно использовать только для включения header-файлов и простых макро-определений. `Token pasting`, вариативные функции и рекурсивные макро вызовы запрещены. Использование условных директив компиляции нежелательно, но иногда неизбежно. Это означает, что только в редких случаях уместно использовать больше чем одно или два условия в директивах компиляции, даже в больших проектах.
9. Использование указателей должно быть ограничено. Допустимо не больше одного уровня разыменования. Операторы разыменования не должны быть скрыты в макро определениях или внутри `typedef`. Указатели на функции запрещены.
10. Весь код должен компилироваться при всех включенных warning'ах, на самых дотошных настройках компилятора с самого первого дня разработки. Весь код должен компилироваться с такими настройками без единого warning'а. Весь код должен проверяться каждый день (как минимум раз в день, но желательно чаще), с использованием лучшего из доступных на текущий день статического анализатора кода, и должен проходить анализ без единого warning'а.
***
## Link
- [10rules.pdf](https://web.eecs.umich.edu/~imarkov/10rules.pdf)
- 