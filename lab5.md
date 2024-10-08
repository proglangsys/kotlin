# 5. Лямбда-выражения 

## Анонимные функции

> Анонимная функция — это функция, которая не имеет имени и может быть использована непосредственно в месте ее объявления.

Объявление и реализация анонимной функции с блочным телом:
```kotlin
val sum = fun(a: Int, b: Int): Int {
    return a + b
}
```
Здесь переменная `sum` хранит ссылку на функцию.

Вызов анонимной функции:
```kotlin
println(sum(3, 5))  // Вывод: 8
```

В Kotlin анонимные функции часто реализуются с помощью лямбда-выражений, которые являются более компактным и удобным способом записи анонимных функций.

## Лямбда-выражения
> Лямбда-выражения — это способ создания анонимных функций.

Синтаксис лямбда-выражений:
```
{ параметры -> тело }
```
-  Левая часть (`параметры`):  Перечисление параметров функции, разделенных запятыми. 
-  Правая часть (`тело`):  блок кода, который выполняется при вызове лямбда-выражения.

Пример лямбда-выражения с двумя параметрами:
```kotlin
{ a: Int, b: Int -> a + b }
```

Объявление и реализация анонимной функции с помощью лямбда-выражения:
```kotlin
val sum = { a: Int, b: Int -> a + b }

println(sum(3, 5))  // Вывод: 8
```

## Тип функции
> _Тип функции_ — это тип данных, который описывает сигнатуру функции, включая типы её параметров и возвращаемое значение. Тип функции может быть использован для объявления переменных, параметров функций и возвращаемых значений.

Синтаксис типа функции
```
(параметры) -> возвращаемый_тип
```

Объявление переменной с типом функции
```kotlin
val sum: (Int, Int) -> Int = { a, b -> a + b }
println(sum(3, 5))  // Вывод: 8
```

## Автоматический вывод типов

Автоматический вывод типов позволяет компилятору самостоятельно определять типы переменных, параметров и возвращаемых значений на основе контекста. Kotlin может автоматически выводить типы функций, если они могут быть определены на основе контекста.

Лямбда-выражение без явного указания типа:
```kotlin
val sum = { a: Int, b: Int -> a + b }
println(sum(3, 5))  // Вывод: 8
```

## Лямбда-выражения с единственным параметром

Когда лямбда-выражение имеет только один параметр, можно использовать специальное ключевое слово `it` для ссылки на этот параметр.

```kotlin
val square: (Int) -> Int = { it * it }
println(square(4))  // Вывод: 16
```

## Захват переменных
> _Захват переменных_ — это возможность лямбда-выражений использовать переменные из окружающей области видимости.

Доступ к переменной из окружающей области видимости
```kotlin
fun main() {
    val multiplier = 3 // Переменная из окружающей области видимости
    val multiplyByMultiplier: (Int) -> Int = { it * multiplier }
    println(multiplyByMultiplier(5))  // Вывод: 15
}
```

Изменение переменной из окружающей области видимости
```kotlin
fun main() {
    var counter = 0
    val incrementCounter: () -> Unit = { counter++ }

    incrementCounter()
    incrementCounter()
    println(counter)  // Вывод: 2
}
```

## Замыкание 

_Замыкание_ — это функция, которая запоминает состояние окружающей области видимости, даже если эта область видимости уже завершила свою работу.

```kotlin
fun createCounter(): () -> Int {
    var count = 0
    return { count++ }
}

fun main() {
    val counter = createCounter()
    println(counter())  // Вывод: 0
    println(counter())  // Вывод: 1
    println(counter())  // Вывод: 2
}
```

## Функции высшего порядка
> _Функция высшего порядка_ — это функция, которая принимает другие функции в качестве аргументов или возвращает функцию в качестве результата.

Прием функции в качестве аргумента:
```kotlin
fun applyOperation(a: Int, b: Int, operation: (Int, Int) -> Int): Int {
    return operation(a, b)
}

fun main() {
	val sum = fun(a: Int, b: Int): Int {
	    return a + b
	}
	// Вызов функции высшего порядка
    val sum = applyOperation(3, 5, sum) 
    println(sum)  // Вывод: 8
}
```

Возврат функции:
```kotlin
fun createMultiplier(factor: Int): (Int) -> Int {
    return { x -> x * factor }
}

fun main() {
	// Вызов функции высшего порядка
    val double = createMultiplier(2)
    println(double(5))  // Вывод: 10
}
```

## Передача лямбда-выражений в функцию

_Передача лямбда-выражений в функцию_ — это механизм, который позволяет передавать функции в качестве аргументов функциям высшего порядка:

```kotlin
fun applyOperation(a: Int, b: Int, operation: (Int, Int) -> Int): Int {
    return operation(a, b)
}

fun main() {
    val result = applyOperation(3, 5, { a, b -> a + b })
    println(result)  // Вывод: 8
}
```

Когда лямбда-выражение является последним аргументом функции, его можно вынести за скобки вызова функции (т.е. использовать _внешние лямбда-выражение_): 
```kotlin
fun main() {
    val sum = applyOperation(3, 5) { a, b -> a + b }
    println(sum)  // Вывод: 8

    val multiply = applyOperation(3, 5) { a, b -> a * b }
    println(multiply)  // Вывод: 15
}
```

## Ссылки на функции

Ссылка на функцию позволяет ссылаться на функцию как на объект.   

Синтаксис ссылок на функции:
```
::имя_функции
```

Ссылка на функцию верхнего уровня:
```kotlin
fun greet() {
    println("Hello, World!")
}

fun main() {
    val greetFunction = ::greet
    greetFunction()  // Вывод: Hello, World!
}
```

Ссылка на функцию с параметрами:
```kotlin
fun add(a: Int, b: Int): Int {
    return a + b
}

fun main() {
    val addFunction = ::add
    println(addFunction(3, 5))  // Вывод: 8
}
```

Передача ссылки на функцию в качестве аргумента:
```kotlin
fun applyOperation(a: Int, b: Int, operation: (Int, Int) -> Int): Int {
    return operation(a, b)
}

fun add(a: Int, b: Int): Int {
    return a + b
}

fun main() {
    val result = applyOperation(3, 5, ::add)
    println(result)  // Вывод: 8
}
```

Ссылки на методы классов пакета kotlin:
```kotlin
fun main() {
    val toDouble = Int::toDouble
    val d = toDouble(5)
    println(d)  // Вывод: 5.0
}
```

## Композиция функций

> Композиция функций — это процесс объединения двух или более функций для создания новой функции. Новая функция применяет первую функцию к своему аргументу, а затем применяет вторую функцию к результату первой функции.

В Kotlin композицию функций можно реализовать с помощью функции высшего порядка `compose`, которая принимает две функции и возвращает новую функцию, являющуюся их композицией.

Композиция двух функций:
```kotlin
fun addOne(x: Int): Int = x + 1
fun multiplyByTwo(x: Int): Int = x * 2

val addOneAndMultiplyByTwo = compose(::multiplyByTwo, ::addOne)

fun main() {
    println(addOneAndMultiplyByTwo(5))  // Вывод: 12
}
```

Композиция с лямбда-выражениями:
```kotlin
val addOne: (Int) -> Int = { x -> x + 1 }
val multiplyByTwo: (Int) -> Int = { x -> x * 2 }

val addOneAndMultiplyByTwo = compose(multiplyByTwo, addOne)

fun main() {
    println(addOneAndMultiplyByTwo(5))  // Вывод: 12
}
```

Композиция с использованием оператора  `andThen`:
```kotlin
val addOne: (Int) -> Int = { x -> x + 1 }
val multiplyByTwo: (Int) -> Int = { x -> x * 2 }

val addOneAndMultiplyByTwo = addOne andThen multiplyByTwo

fun main() {
    println(addOneAndMultiplyByTwo(5))  // Вывод: 12
}
```


## Рекурсия

Лямбда-выражения можно использовать как рекурсивные функции:
```kotlin
fun main() {
    val factorial: (Int) -> Int = { n ->
        val factorial: (Int) -> Int = { m ->
            if (m == 0) 1 else m * factorial(m - 1)
        }
        factorial(n)
    }

    println(factorial(5))  // Вывод: 120
}
```

## Задание


**Часть I**
- **Анонимные функции:** Напишите четыре лямбда-выражение, которые принимают два числа с плавающей точкой и возвращают соответственно результат их сложения, вычитания, умножения и деления. Присвойте эти лямбда-выражения переменным и вызовите их. Результат вызова выведите в командную строку.
- **Передача лямбда-выражений в функцию:** Напишите функцию  `applyOperation`, которая принимает два целых числа и лямбда-выражение, которое принимает два целых числа и возвращает целое число. Напишите четыре вызова  функции `applyOperation` с двумя целыми числами и лямбда-выражениями, которые возвращают сумму, разность, произведение и остаток от деления двух целых чисел. Результат вызова выведите в командную строку.
- **Лямбда-выражения с захватом переменных:** Создайте переменную  `multiplier` типа `Double` и присвойте ей некоторое значение. Напишите лямбда-выражение, которое принимает целое число и возвращает его произведение на `multiplier`. Вызовите это лямбда-выражение с некоторым аргументом. Результат вызова выведите в командную строку.
- **Лямбда-выражения как параметры по умолчанию:** Напишите функцию  `repeatString`, которая принимает строку и целое число (количество повторений) и возвращает строку, состоящую из указанного количества повторений исходной строки. Добавьте параметр по умолчанию в функцию  `repeatString`, который будет использовать лямбда-выражение для добавления пробела между повторениями строки. Результат вызова выведите в командную строку.

**Часть II**
- **Ссылка на стандартную функцию:** Используйте ссылку на стандартную функцию  `String::length`  для вычисления длины строки. Создайте переменную  `myString`  и присвойте ей значение "Hello, World!". Используйте ссылку на функцию  `String::length`, чтобы вывести длину строки.
- **Ссылка на функцию с параметрами:** Используйте ссылку на функцию  `String::substring`  для извлечения подстроки из строки. Создайте переменную  `myString`  и присвойте ей значение "Hello, World!". Используйте ссылку на функцию  `String::substring`, чтобы извлечь подстроку "World" и вывести её.
- **Композиция функций:** Реализуйте функцию  `compose`, которая принимает две лямбда-функции и возвращает новую функцию, которая является композицией двух исходных лямбда-функций.
- **Рекурсивное применение функции:** Реализуйте функцию  `applyRecursively`, которая принимает лямбда-функцию и целое число  `n`, и возвращает новую функцию, которая применяет исходную функцию  `n`  раз.
- **Функция высшего порядка с захватом переменной:** Реализуйте функцию  `createMultiplier`, которая принимает целое число  `n`  и возвращает лямбда-функцию, которая принимает целое число и возвращает его произведение на  `n`.
- **Функция с замыканием:** Реализуйте функцию  `createCounter`, которая возвращает лямбда-функцию, которая при каждом вызове увеличивает счетчик на 1 и возвращает его значение.

**Часть III**
- **Использование лямбда-выражений для реализации различных алгоритмов сортировки:** Реализовать четыре различных алгоритма сортировки массива целых чисел, используя лямбда-выражения для определения порядка сортировки (по возрастанию, по убыванию).

## Вопросы
1. Анонимные функции
2. Передача лямбда-выражений в функцию
3. Лямбда-выражения с захватом переменных
4. Лямбда-выражения как параметры по умолчанию
5. Ссылки на функции
6. Композиция функций
7. Рекурсивное применение функции
8. Функция высшего порядка
9. Функция высшего порядка с захватом переменной
10. Функция с замыканием

## Ресурсы

- [Kotlin. Программирование для профессионалов. 2-е изд. Скин Д., Гринхол Д., Бэйли Э.](https://www.piter.com/collection/yazyki-programmirovaniya/product/kotlin-programmirovanie-dlya-professionalov-2-e-izd) Глава 8
- [Kotlin docs: Higher-order functions and lambdas](https://kotlinlang.org/docs/lambdas.html)
- [Руководство по языку Kotlin: Высокоуровневые функции и лямбды](https://kotlinlang.ru/docs/lambdas.html)