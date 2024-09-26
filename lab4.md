# 4. Массивы. Null-безопасность. Исключения. 

## Массивы

> Массив — это коллекция элементов одного типа, расположенных в непрерывной области памяти.
В Kotlin массивы представлены объектами типа `Array`, имеют фиксированный размер и индексируются с нуля.

### Типы массивов

| Тип массива          | Функция создания       | Описание                                                                 |
|----------------------|------------------------|-------------------------------------------------------------------------|
| `Array<Int>`         | `arrayOf`              | Массив целых чисел.                                                     |
| `Array<String>`      | `arrayOf`              | Массив строк.                                                           |
| `IntArray`           | `intArrayOf`           | Массив примитивных целых чисел (`Int`).                                 |
| `DoubleArray`        | `doubleArrayOf`        | Массив примитивных чисел с плавающей запятой (`Double`).                |
| `FloatArray`         | `floatArrayOf`         | Массив примитивных чисел с плавающей запятой (`Float`).                 |
| `LongArray`          | `longArrayOf`          | Массив примитивных длинных целых чисел (`Long`).                        |
| `ShortArray`         | `shortArrayOf`         | Массив примитивных коротких целых чисел (`Short`).                      |
| `ByteArray`          | `byteArrayOf`          | Массив примитивных байтов (`Byte`).                                     |
| `BooleanArray`       | `booleanArrayOf`       | Массив примитивных булевых значений (`Boolean`).                        |
| `CharArray`          | `charArrayOf`          | Массив примитивных символов (`Char`).                                   |                                   |

### Создание массивов

```kotlin
val array = arrayOf(1, 2, 3, 4, 5) // Создает массив [1, 2, 3, 4, 5]
val array = Array(5) { i -> i * 2 } // Создает массив [0, 2, 4, 6, 8]
val intArray = intArrayOf(1, 2, 3, 4, 5) // Создает массив [1, 2, 3, 4, 5]
val doubleArray = doubleArrayOf(1.0, 2.0, 3.0) // Создает массив [1.0, 2.0, 3.0]
```

### Доступ к элементам массива
```kotlin
val array = arrayOf(1, 2, 3, 4, 5)
println(array[0]) // Вывод: 1
println(array[2]) // Вывод: 3
array[2] = 10 // Изменение элемента массива
println(array[2]) // Вывод: 10
```
### Основные операции с массивами
#### Получение размера массива:
```kotlin
val array = arrayOf(1, 2, 3, 4, 5)
println(array.size) // Вывод: 5
```
#### Проверка на пустоту:
```kotlin
val array = arrayOf(1, 2, 3, 4, 5)
println(array.isEmpty()) // Вывод: false
println(array.isNotEmpty()) // Вывод: true
```
#### Поиск элемента:
```kotlin
val array = arrayOf(1, 2, 3, 4, 5)
println(array.indexOf(3)) // Вывод: 2
```
#### Сортировка массива:
```kotlin
val array = arrayOf(5, 3, 2, 4, 1)
array.sort()
println(array.joinToString()) // Вывод: 1, 2, 3, 4, 5
```

### Многомерные массивы

> Многомерные массивы — это массивы, элементами которых являются другие массивы. Наиболее распространенным видом многомерных массивов являются двумерные массивы, которые можно представить в виде таблицы или матрицы. Можно создавать массивы любой размерности.

#### Создание многомерных массивов
```kotlin
val matrix = Array(3) { Array(3) { 0 } } // Создание двумерного массива
val cube = Array(3) { Array(3) { Array(3) { 0 } } } // Создание трехмерного массива
```

#### Доступ к элементам многомерных массивов
```kotlin
val matrix = arrayOf(
    arrayOf(1, 2, 3),
    arrayOf(4, 5, 6),
    arrayOf(7, 8, 9)
)

println(matrix[0][0]) // Вывод: 1
println(matrix[1][2]) // Вывод: 6

matrix[1][1] = 10
println(matrix[1][1]) // Вывод: 10
```

## Null-безопасность

> Null-безопасность — это механизм, который помогает избежать ошибок, связанных с `null` значениями.

### Допустимость null-значения
В Kotlin типы данных по умолчанию не допускают `null` значений, что позволяет избежать многих ошибок, связанных с обращением к `null`:
```kotlin
val a: Int = 10
val b: String = "Hello"
```
Типы данных, допускающие null-значения:
```kotlin
val a: Int? = null
val b: String? = null
```
При обращение значениям таких типов можно получить исключения типа NPE (NullPointerException). 

###  Проверка `null` в операторе `if`
Для того чтобы избежать исключения типа NPE, можно проверять значение выражения на равенство `null` с помощью if/else-выражения:
```kotlin
fun main() {  
	val str: String? = "Hello"  
	val length = if (str != null) str.length else null  
	println(length) // Вывод: 5
  
	val str2: String? = null  
	val length2 = if (str2 != null) str2.length else null  
	println(length2) // Вывод: null  
}
```

### Оператор безопасного вызова `?.`

Оператор безопасного вызова позволяет вызывать методы или обращаться к свойствам объекта, если он не равен `null`. Если объект равен `null`, выражение возвращает `null`.
```kotlin
val str: String? = "Hello"
val length = str?.length // length будет равно 5

val str2: String? = null
val length2 = str2?.length // length2 будет равно null
```

### Функция области видимости `let`
Функция `let` позволяет выполнить блок кода с объектом в качестве аргумента. Она возвращает результат выполнения блока кода. Внутри блока кода сам объект доступен через  `it`  (по умолчанию) или через явно указанное имя.
```kotlin
fun main() {  
	val line: String? = "0.5"  
	val result = line?.let {  
		it.toIntOrNull() // Вернуть null, если строка line не представляет целое число 
	} ?: 0 // result будет равно "0", если вызов line?.let вернет null
  
	println(result) // Вывод: 0
}
```

### Элвис-оператор `?:`
Оператор Элвис позволяет указать значение по умолчанию, которое будет использовано, если выражение равно `null`.
```kotlin
val str: String? = null
val length = str?.length ?: 0 // length будет равно 0
```

### Оператор not-null утверждения `!!`
Оператор  `!!`  принудительно приводит объект к non-nullable типу. Если объект равен  `null`, выбрасывается исключение  `NullPointerException`.
```kotlin
val str: String? = "Hello"
val length = str!!.length // length будет равно 5

val str2: String? = null
val length2 = str2!!.length // Выбросит NullPointerException
```

## Исключения

> Исключения — это события, которые возникают во время выполнения программы и нарушают нормальный ход её работы. В Kotlin исключения представлены классами, которые наследуются от класса `Throwable`.

В Kotlin все исключения являются непроверяемыми. Это означает, что разработчики не обязаны явно явно обрабатывать потенциальные исключения или объявлять исключения в сигнатуре метода.

### Обработка исключений
#### Конструкция `try-catch`
Конструкция `try-catch` позволяет обрабатывать исключения, возникающие во время выполнения программы. Она состоит из двух основных частей:
1.  Блок  `try` содержит код, который может вызвать исключение.
2. Блок  `catch` содержит код, который будет выполнен, если исключение возникло в блоке  `try`.
 
```kotlin
fun divide(a: Int, b: Int): Int {
    return try {
        a / b
    } catch (e: ArithmeticException) {
        println("Division by zero is not allowed")
        0
    }
}

fun main() {
    println(divide(10, 2)) // Вывод: 5
    println(divide(10, 0)) // Вывод: Division by zero is not allowed, 0
}
```

#### Конструкция `try-catch-finally`
Блок `finally` выполняется всегда, независимо от того, было ли выброшено исключение:
```kotlin
fun readFile(fileName: String): String {
    return try {
        // Код для чтения файла
        "File content"
    } catch (e: FileNotFoundException) {
        println("File not found")
        ""
    } finally {
        println("File operation completed")
    }
}

fun main() {
    println(readFile("example.txt")) // Вывод: File content, File operation completed
    println(readFile("nonexistent.txt")) // Вывод: File not found, File operation completed
}
```

### Выбрасывание исключений 
Выбрасывание исключения — это процесс, при котором программа прерывает нормальное выполнение и передает управление обработчику исключений. В Kotlin для выбрасывания исключений используется ключевое слово `throw`.
```kotlin
fun divide(a: Int, b: Int): Int {
    if (b == 0) {
        throw ArithmeticException("Division by zero is not allowed")
    }
    return a / b
}

fun main() {
    try {
        println(divide(10, 2)) // Вывод: 5
        println(divide(10, 0)) // Вывод: ArithmeticException: Division by zero is not allowed
    } catch (e: ArithmeticException) {
        println("Caught exception: ${e.message}")
    }
}
```

### Проверка предусловий
_Проверка предусловий_ — это механизм, который позволяет убедиться, что определенные условия выполняются перед выполнением кода. В Kotlin для проверки предусловий используются специальные функции, которые могут выбрасывать исключения, если условия не выполняются.

| Функция              | Описание                                                                 | Выбрасываемое исключение       |
|----------------------|-------------------------------------------------------------------------|--------------------------------|
| `require`            | Проверяет условие и выбрасывает `IllegalArgumentException`, если условие не выполняется. | `IllegalArgumentException`     |
| `requireNotNull`     | Проверяет, что значение не равно `null`, и выбрасывает `IllegalArgumentException`, если значение равно `null`. | `IllegalArgumentException`     |
| `check`              | Проверяет условие и выбрасывает `IllegalStateException`, если условие не выполняется. | `IllegalStateException`       |
| `checkNotNull`       | Проверяет, что значение не равно `null`, и выбрасывает `IllegalStateException`, если значение равно `null`. | `IllegalStateException`       |
| `assert`             | Проверяет условие и выбрасывает `AssertionError`, если условие не выполняется. Используется в отладочных целях. | `AssertionError`              |

Пример использования функции `require`
```kotlin
fun divide(a: Int, b: Int): Int {
    require(b != 0) { "Division by zero is not allowed" }
    return a / b
}

fun main() {
    println(divide(10, 2)) // Вывод: 5
    println(divide(10, 0)) // Вывод: IllegalArgumentException: Division by zero is not allowed
}
```

Пример использования функции `requireNotNull`
```kotlin
fun processString(str: String?) {
    requireNotNull(str) { "String cannot be null" }
    println(str.length)
}

fun main() {
    processString("Hello") // Вывод: 5
    processString(null) // Вывод: IllegalArgumentException: String cannot be null
}
```

Пример использования функции `check`
```kotlin
fun validateState(isReady: Boolean) {
    check(isReady) { "State is not ready" }
    println("State is ready")
}

fun main() {
    validateState(true) // Вывод: State is ready
    validateState(false) // Вывод: IllegalStateException: State is not ready
}
```

Пример использования функции `checkNotNull`
```kotlin
fun processData(data: Any?) {
    checkNotNull(data) { "Data cannot be null" }
    println(data.toString())
}

fun main() {
    processData("Some data") // Вывод: Some data
    processData(null) // Вывод: IllegalStateException: Data cannot be null
}
```

Пример использования функции `assert`
```kotlin
fun validateInput(input: String) {
    assert(input.isNotEmpty()) { "Input cannot be empty" }
    println("Input is valid")
}

fun main() {
    validateInput("Hello") // Вывод: Input is valid
    validateInput("") // Вывод: AssertionError: Input cannot be empty (только в режиме отладки)
}
```

## Задание


**Часть I**
- **Сумма чисел с null-безопасностью.** Напишите функцию `safeSum`, которая принимает два числовых параметра и возвращает их сумму. Если хотя бы один из параметров равен `null`, функция должна подставить вместо него `0` и вернуть сумму. Используйте Элвис-оператор (`?:`).
- **Обработка строк с null-безопасностью**
Напишите функцию  `safeSubstring`, которая принимает строку и два индекса (начальный и конечный). Функция должна вернуть подстроку, начиная с начального индекса и заканчивая конечным индексом. Если строка равна  `null`  или индексы выходят за пределы строки, функция должна вернуть  `null`. Используйте оператор безопасного вызова (`?.`) и функцию области видимости `let` для обработки возможных  `null`  значений и проверки индексов.
- **Безопасное деление с обработкой исключений.** Напишите функцию `safeDivide`, которая принимает два числа (делимое и делитель) и возвращает результат деления. Если делитель равен нулю, функция должна выбросить исключение `ArithmeticException` с соответствующим сообщением. Если входные данные не являются числами, функция должна выбросить исключение `NumberFormatException` с соответствующим сообщением.

**Часть II**
- **Безопасное преобразование строки в массив чисел.** Напишите функцию `safeStringToNumberArray`, которая принимает строку, содержащую числа, разделенные запятыми, и возвращает массив целых чисел. Если строка содержит некорректные данные (например, нечисловые символы), функция должна выбросить исключение `NumberFormatException` с соответствующим сообщением. Если строка пуста, функция должна вернуть пустой массив.
- **Безопасное добавление элемента в массив.** Напишите функцию `safeAddElement`, которая принимает массив целых чисел и новый элемент. Функция должна добавить элемент в массив, если это возможно. Если массив уже заполнен, функция должна выбросить исключение `ArrayIndexOutOfBoundsException` с соответствующим сообщением. Используйте одну из функций проверки предусловий для проверки размера массива.
- **Удаление дубликатов из массива чисел с проверкой пустоты массива**
Напишите функцию  `removeDuplicates`, которая принимает массив целых чисел и возвращает новый массив, содержащий только уникальные элементы. Если массив пуст, функция должна выбросить исключение  `IllegalStateException`  с соответствующим сообщением. Используйте одну из функций проверки предусловий для проверки пустоты массива.

**Часть III**
Реализуйте игру "Сапер", в которой игровое поле представлено с помощью двумерных массивов. 

_Краткое описание правил:_ Игровое поле представляет собой матрицу, где каждая ячейка может быть либо пустой, либо содержать мину. Игрок открывает клетки на поле, стараясь не наткнуться на мину. Если игрок открывает клетку с миной, игра заканчивается. Если игрок открывает пустую клетку, он видит количество мин вокруг этой клетки. Цель игры — открыть все клетки, не содержащие мин. 
[Подробное описание правил доступно здесь](https://ru.wikipedia.org/wiki/%D0%A1%D0%B0%D0%BF%D1%91%D1%80_(%D0%B8%D0%B3%D1%80%D0%B0))

_Требования:_
-   Использовать null-безопасность для обработки возможных  `null`  значений.
-   Применить оператор безопасного вызова (`?.`), Элвис-оператор (`?:`) и др.   
-   Обработать исключения, которые могут возникнуть при вводе данных пользователем
-   Использовать функции  проверки предусловий
-   Предоставьте пользователю возможность задавать размер игрового поля и количеством мин.
-   Предоставьте пользователю возможность вводить координаты клеток для открытия.

## Вопросы

1. Массивы
2. Массивы значений примитивных типов
3. Многомерные массивы
4. Nullable и Non-nullable типы данных
5. Оператор безопасного вызова
6. Элвис-оператор
7. Функция области видимости let
8. Оператор not-null утверждения !!
9. Обработка исключений
10. Выбрасывание исключений
11. Проверка предусловий

## Ресурсы

- [Kotlin. Программирование для профессионалов. 2-е изд. Скин Д., Гринхол Д., Бэйли Э.](https://www.piter.com/collection/yazyki-programmirovaniya/product/kotlin-programmirovanie-dlya-professionalov-2-e-izd) Глава 7
- [Kotlin docs: Arrays](https://kotlinlang.org/docs/arrays.html)
- [Kotlin docs: Null safety](https://kotlinlang.org/docs/null-safety.html)
- [Kotlin docs: Exceptions](https://kotlinlang.org/docs/exceptions.html)
- [Руководство по языку Kotlin: Основные типы](https://kotlinlang.ru/docs/basic-types.html)
- [Руководство по языку Kotlin: Null безопасность](https://kotlinlang.ru/docs/null-safety.html)
- [Руководство по языку Kotlin: Исключения](https://kotlinlang.ru/docs/exceptions.html)