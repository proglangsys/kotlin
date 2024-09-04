# 1. Инструментальные средства разработки

## Методические указания

### Платформа разработки JDK

[Java Development Kit](https://ru.wikipedia.org/wiki/Java_Development_Kit) (JDK) — комплект инструментальных средств для разработки приложений на языке [Java](https://ru.wikipedia.org/wiki/Java). Включает компилятор [javac](https://ru.wikipedia.org/wiki/Javac), утилиты, библиотеки классов Java API, документацию и примеры, среду исполнения приложений — [Java Runtime Environment](https://ru.wikipedia.org/wiki/Java_Runtime_Environment) (JRE).

JRE включает виртуальную машину [Java Virtual Machine](https://ru.wikipedia.org/wiki/Java_Virtual_Machine) (VM) и откомпилированные библиотеки классов Java API. Позволяет запускать Java приложения на любых устройствах или операционных системах.

Существует три редакции JDK:
- SE (Standard Edition) — стандартная редакция JDK, которая используется для разработки консольных и настольных приложений.
- EE (Enterprise Edition) — редакция для разработки распределенных приложений масштаба предприятий.
- ME (Micro Edition) — редакция для разработки приложений мобильных устройств и встраиваемых систем.

> Для выполнение заданий рекомендуется установить:
> - [Oracle JDK SE](https://www.oracle.com/java/technologies/downloads/) или [OpenJDK](https://openjdk.org/install/) (версию 8 или выше).

### Интегрированная среда разработки

Integrated Development Environment (IDE) — комплекс программных средств, используемый программистами для разработки программного обеспечения (ПО), в том числе, текстовый редактор, средства автоматизации сборки, отладчик.

Существует несколько популярных IDE для разработки на Kotlin:
- [IntelliJ IDEA](https://www.jetbrains.com/idea)
- [Eclipse](https://eclipseide.org/release)
- [Fleet](https://www.jetbrains.com/fleet)

> Для выполнение заданий рекомендуется установить:
> - [IntelliJ IDEA Community Edition](https://www.jetbrains.com/ru-ru/idea/download/#section=windows)

### Создание проекта

#### Создать новый проект

Запустить «IntelliJ IDEA». Если открыт приветственный экран «Welcome to IntelliJ IDEA», нажать кнопку «New Project». Если открыто основное окно, выбрать пункт меню «File | New | Project».

![Снимок экрана: создание нового проекта](https://github.com/proglangsys/kotlin/blob/main/images/lab1_1.png)

Выбрать пункт «New Project» из списка слева. Задать имя нового проекта (например, «Lab1») в поле «Name» и изменить его месторасположение в поле «Location», если необходимо. Выбрать язык программирования «Kotlin» (в списке «Languages»). Выбрать систему сборки «Gradle» (в списке «Build system») и предметно-ориентированный язык  системы сборки «Groovy» (в списке «Gradle DSL). Проверить, что выбрана JDK версии 8 или выше (в списке «JDK»).

> Если вместо приветственного экрана «Welcome to IntelliJ IDEA» уже
> открыт некоторый проект, то закрыть его можно, выбрав пункт меню 
> «File | Close Project», после чего вы автоматически перейдете к
> приветственному экрану.

#### Создать Kotlin-файл 

В окне «Project» щелкнуть правой кнопкой мыши папку «src/main/kotlin», выбрать пункт «New | Kotlin Class/File». Задать имя нового Kotlin-файла как «Main», выбрав пункт «File» в списке снизу.

![Снимок экрана: создание нового файла](https://github.com/proglangsys/kotlin/blob/main/images/lab1_2.png)

Внутри созданного Kotlin-файла можем писать код.

#### Main-функция

Main-функция — «точка входа», запускает поток исполнения программы:
```kotlin
fun main() {
    // здесь начинается поток исполнения программы
    println("Hello, Kotlin!") // Вывод текста "Hello, Kotlin!" в командную строку
}
```

#### Собрать и запустить приложение

Выбрать пункт меню «Run | Run…» или нажать кнопку «Run» на панели инструментов сверху — «зеленый треугольник» (Alt+Shift+F10). В раскрывшемся списке выбрать «MainKt». Запустится компиляция исходного кода программы в байт-код и затем исполнение приложения в Java VM. Результат выполнения программы, сообщение «Hello, Kotlin!», будет выводиться во встроенной командной строке.

![Снимок экрана: проект (слева) и редактируемый файл (справа)](https://github.com/proglangsys/kotlin/blob/main/images/lab1_3.png)

### Выполнение Kotlin-кода в REPL

REPL (Read, Evaluate, Print, Loop) --- командная строка внутри IDE для быстрого выполнения Kotlin-кода без создания Kotlin-файла и компиляции. REPL можно запустить командой меню «Tools | Kotlin | Kotlin REPL». Код вводится в командной строке REPL. Для того чтобы запустить его следует нажать кнопку запуска слева (Ctrl+Enter). Результат выводится также в командной строке REPL.

![Снимок экрана: выполнение Kotlin-кода в REPL](https://github.com/proglangsys/kotlin/blob/main/images/lab1_4.png)

### Переменные, константы и типы

#### Типы данных
_Тип данных_ (_Тип_) --- множество значений и операций над этими значениями (IEEE Std 1320.2-1998). 
Тип ограничивает данные, хранимые в переменных и константах, а также вычисляемые выражениями. В программах, представленных на Kotlin, любая переменная, константа или выражение имеет некоторый тип данных.

Встроенные типы данных: `String` (строка), `Char` (символ), `Boolean` (логический), `Int` (целое число), `Double` (число с плавающей запятой), `List` (список), `Set` (множество), `Map` (ассоциативный массив)

#### Переменные с доступом на чтение и редактирование

Значения таких переменных можно изменить после инициализации.

Объявление и инициализация переменных:
```kotlin
fun main() {
    var str: String = "My text"
    var ch: Char = 'a'
    var flag : Boolean = false
    var i : Int = 5
    var d : Double = 0.5
}
```

Далее можно изменить значения таких переменных:
```kotlin
fun main() {
    var i : Int = 5
    // ...
    i = i + 1
    i += 1
    i ++
}
```

#### Переменные с доступом только на чтение

Значения таких переменных нельзя изменить после инициализации:
```kotlin
fun main() {
    val str: String = "My text"
    val ch: Char = 'a'
    val flag : Boolean = false
    val i : Int = 5
    val d : Double = 0.5
    val numbers = listOf(1, 2, 3, 4, 5)
    val planets = setOf("меркурий", "венера", "земля", "марс")
    val sizes = mapOf("small" to 500, "medium" to 1500, "large" to 3000)
}
```

При попытке изменить значения таких переменных будет сгенерирована ошибка:
```kotlin
fun main() {
    val i : Int = 5
    // ...
    i = i + 1 // Это ошибка
}
```

#### Автоматический вывод типов

Можно не указывать тип данных переменной, поскольку он выводится автоматически:
```kotlin
fun main() {
    var str = "My text"
    var ch = 'a'
    val flag = false
    val i = 5
    val d = 0.5
}
```
#### Глобальные константы

Глобальная константа объявляется вне функций и может иметь значения только одного из базовых типов данных: `String`, `Char`, `Byte`, `Short`, `Int`, `Long`, `Float`, `Double`, `Boolean`.
```kotlin
const val MIN_VALUE = 0.0
const val MAX_VALUE = 1.0
fun main() {
	// Инструкции
}
// Другие функции
```

### Декомпиляция байт-кода

Байт-код, полученный в результате компиляции Kotlin-файла, можно открыть командой меню «Tools | Kotlin | Show Kotlin Bytecode». Для того чтобы декомпилировать его в Java, можно нажать кнопку «Decompile» слева сверху окна с байт-кодом. Результат декомпиляции байт-кода выводится в Java-класс.

![Снимок экрана: окно с байт-кодом](https://github.com/proglangsys/kotlin/blob/main/images/lab1_5.png)

### Сборка и запуск проекта Gradle

#### Добавление зависимостей

Зависимости упрощают использование стороннего кода (библиотек, фреймворков и пр.) в собственных проектах. Зависимость представляет собой тройку координат: `groupId` (идентификатор группы), `artifactId` (идентификатор артефакта) и `version` (версия). Пример зависимости для системы сборки Maven:
```xml
<dependency>
    <groupId>org.apache.commons</groupId>
    <artifactId>commons-math</artifactId>
    <version>2.2</version>
</dependency>
```
Пример зависимости для системы сборки Gradle:
```groovy
implementation group: 'org.apache.commons', name: 'commons-math', version: '2.2'
```
Включение зависимости в проект означает, что соответствующий ей сторонний код (пакеты, классы, функции) становится доступным для использования в коде самого проекта.

Основной репозиторий зависимостей --- [Maven Repository](https://mvnrepository.com).


#### Сборка и запуск проекта

Сборка и запуск проекта осуществляется командами «build | build» и «application | run» соответственно панели Gradle (расположена справа, по умолчанию скрыта, ее можно открыть кнопкой Gradle). 

## Задание

**Часть I**

 - Разработать программу для вычисления факториал числа _n_ с помощью рекурсии (без циклов)
 - Создать, собрать и запустить проект с данной программой в IDE
 - Реализовать ввод числа _n_ из параметров командной строки
 - Декомпилировать байт-код, полученный в результате компиляции Kotlin-файла, в Java-класс  
 - Сопоставить (умозрительно) исходный Kotlin-файл и полученный Java-класс 

**Часть II**

- Выполнить программу в REPL для вычисления евклидова расстояния между двух точек 
- Использовать функции из пакета kotlin.math
- Создать проект с данной программой в IDE
- Добавить зависимость [commons-math](https://mvnrepository.com/artifact/org.apache.commons/commons-math) в файл build.gradle  
- Использовать данную зависимость при вычислении евклидова расстояния (воспользоваться готовым методом, поставляемом в составе библиотеки [Apache Commons Math](https://commons.apache.org/proper/commons-math))
- Собрать и запустить проект с данной программой с помощью системы сборки Gradle

**Часть III**
 - Разработать программу для вычисления косинусного сходства двух векторов, представленный набором _n_ элементов --- чисел
 - Реализовать ввод числа _n_ из параметров командной строки
 - Реализовать наполнение векторов случайными числами
 - Создать, собрать и запустить проект с данной программой в IDE
****

## Вопросы

1. Платформа разработки JDK
2. Платформа исполнения JRE
3. Байт-код
4. Main-функция
5. Типы данных
6. Литералы
7. Идентификаторы
8. Локальные переменные и глобальные константы
9. Блоки кода
10. Сборка мусора

## Ресурсы

- [Kotlin. Программирование для профессионалов. 2-е изд. Скин Д., Гринхол Д., Бэйли Э.](https://www.piter.com/collection/yazyki-programmirovaniya/product/kotlin-programmirovanie-dlya-professionalov-2-e-izd) Главы 1, 2
- [Создание проекта в IntelliJIDEA](https://www.jetbrains.com/help/idea/new-project-wizard.html#new-project-no-frameworks)
- [Main-функция](https://docs.oracle.com/javase/tutorial/getStarted/application/index.html)
- [Библиотека «Apache Commons Math»](https://commons.apache.org/proper/commons-math/)
- [Класс Class EuclideanDistance из «Apache Commons Math»](https://commons.apache.org/proper/commons-math/javadocs/api-3.6.1/org/apache/commons/math3/ml/distance/EuclideanDistance.html)
- [Maven репозиторий](https://mvnrepository.com)
- [IntelliJIDEA на русском](https://www.jetbrains.com/ru-ru/idea/resources)
- [Gradle в IntelliJ IDEA](https://www.jetbrains.com/help/idea/gradle.html)
