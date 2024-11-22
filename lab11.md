# 11. Интерфейсы. Объявления объектов

## Интерфейсы

Интерфейс позволяет определить контракт, который должен быть реализован классами. Классы могут реализовывать несколько интерфейсов.

Интерфейсы объявляются с помощью ключевого слова `interface`. 
Они могут содержать абстрактные методы и свойства, а также методы с реализацией по умолчанию.
```kotlin
interface MyInterface {
	// Абстрактный метод
    fun abstractMethod()

	// Метод с реализацией по умолчанию
    fun defaultMethod() {
        println("This is a default method")
    }
}
```

### Реализация интерфейса в классе
Класс реализует интерфейс с помощью ключевого слова `implements`. Класс должен предоставить реализацию всех абстрактных методов, определенных в интерфейсе.
```kotlin
class MyClass : MyInterface {
	// Реализация абстрактного метода, унаследованного от интерфейса
    override fun abstractMethod() {
        println("This is an implementation of abstractMethod")
    }
}

fun main() {
    val obj = MyClass()
    obj.abstractMethod()  // Вывод: This is an implementation of abstractMethod
    obj.defaultMethod()   // Вывод: This is a default method
}
```

### Свойства в интерфейсах

Интерфейсы могут содержать абстрактные свойства, которые должны быть реализованы в классе. Также можно определить свойства с геттерами и сеттерами по умолчанию.

```kotlin
interface MyInterface {
	// Абстрактное свойство
    val abstractProperty: String
	// Свойство с реализацией геттера по умолчанию
    val defaultProperty: String
        get() = "Default Property"
}

class MyClass : MyInterface {
	// Реализация абстрактного свойства
    override val abstractProperty: String = "Implemented Property"
}

fun main() {
    val obj = MyClass()
    println(obj.abstractProperty)  // Вывод: Implemented Property
    println(obj.defaultProperty)   // Вывод: Default Property
}
```

### Множественная реализация интерфейсов

Класс может реализовывать несколько интерфейсов.
```kotlin
interface InterfaceA {
    fun methodA()
}

interface InterfaceB {
    fun methodB()
}

class MyClass : InterfaceA, InterfaceB {
    override fun methodA() {
        println("Method A")
    }

    override fun methodB() {
        println("Method B")
    }
}

fun main() {
    val obj = MyClass()
    obj.methodA()  // Вывод: Method A
    obj.methodB()  // Вывод: Method B
}
```

### Наследование интерфейсов

Интерфейс может наследовать один или несколько других интерфейсов.
```kotlin
interface BaseInterface {
    fun baseMethod()
}

interface DerivedInterface : BaseInterface {
    fun derivedMethod()
}

class MyClass : DerivedInterface {
    override fun baseMethod() {
        println("Implementation of baseMethod")
    }

    override fun derivedMethod() {
        println("Implementation of derivedMethod")
    }
}

fun main() {
    val obj = MyClass()
    obj.baseMethod()     // Вывод: Implementation of baseMethod
    obj.derivedMethod()  // Вывод: Implementation of derivedMethod
}
```

## Вложенные интерфейсы

...

...

... 


## Объявления объектов

Объект создается с помощью ключевого слова `object`. Этот объект может быть использован как экземпляр класса (синглтона), но при этом не требует явного создания экземпляра.
```kotlin
object MySingleton {
    val name = "Singleton Object"

    fun sayHello() {
        println("Hello from $name")
    }
}

fun main() {
    MySingleton.sayHello()  // Вывод: Hello from Singleton Object
}
```

### Анонимные объекты

Анонимный объект может быть использован как выражение, чтобы создать экземпляр класса без явного создания класса.

```kotlin
fun main() {
    val myObject = object {
        val name = "Anonymous Object"
        fun sayHello() {
            println("Hello from $name")
        }
    }

    myObject.sayHello()  // Вывод: Hello from Anonymous Object
}
```

### Наследование анонимных объектов от супертипов

Анонимный объект реализует интерфейс или наследует класс, но не требует создания отдельного класса для этого.
```kotlin
interface MyInterface {
    fun doSomething()
}

fun main() {
    val myObject = object : MyInterface {
        override fun doSomething() {
            println("Doing something")
        }
    }

    myObject.doSomething()  // Вывод: Doing something
}
```

### Анонимные объекты как параметры функции
Объект может быть передан в качестве параметра функции.
```kotlin
fun processObject(obj: Any) {
    println("Processing object: $obj")
}

fun main() {
    processObject(object {
        val value = 42
    })
}
```

### Анонимные объекты как возвращаемые типы значений

Анонимные объекты могут использоваться в качестве возвращаемых типов значений.

```kotlin
interface SystemInfo {
    val systemName: String
    val version: String
    fun printInfo()
}

fun getSystemInfo(): SystemInfo {
    return object : SystemInfo {
        override val systemName: String = "MySystem"
        override val version: String = "1.0.0"

        override fun printInfo() {
            println("System Name: $systemName, Version: $version")
        }
    }
}

fun main() {
    val systemInfo = getSystemInfo()
    systemInfo.printInfo()  // Вывод: System Name: MySystem, Version: 1.0.0
}
```

### Объекты-компаньоны
Объект-компаньон объявляется внутри класса и позволяет добавить статические члены в класс. Он создается с помощью пары ключевых слов `companion object`.

```kotlin
class MyClass {
    companion object {
        val staticField = "Static Field"

        fun staticMethod() {
            println("This is a static method")
        }
    }
}

fun main() {
    println(MyClass.staticField) // Вывод: Static Field
    MyClass.staticMethod()       // Вывод: This is a static method
}
```

Объект-компаньон может иметь имя, что позволяет обращаться к его членам через это имя.
```kotlin
class MyClass {
    companion object Factory {
        fun create(): MyClass = MyClass()
    }
}

fun main() {
    val instance = MyClass.Factory.create()
}
```

## Задание

**Часть I** 
- **Создание объекта-синглтона.** Создайте объект-синглтон `Logger`, который будет хранить информацию о сообщениях, записанных в журнал. Объект должен иметь метод `log(message: String)`, который будет сохранять переданное сообщение, метод `getLastMessage(): String`, который будет возвращать последнее сохраненное сообщение и метод `getAllMessages(): List<String>`, который будет возвращать все сохраненные сообщения.
- **Использование анонимного объекта для реализации интерфейса.** Создайте интерфейс  `ClickListener`  с методом  `onClick()`. В основной функции программы создайте анонимный объект, реализующий этот интерфейс, и назначьте его обработчиком события нажатия кнопки. При нажатии кнопки должно выводиться сообщение "Кнопка нажата!".
- **Создание объекта-компаньона.** Создайте класс  `User`  с приватным конструктором и объектом-компаньоном. Объект-компаньон должен содержать содержать фабричный метод `createUser(name: String, age: Int): User`, который будет создавать и возвращать новый экземпляр класса  `User`.
- **Создание объекта с помощью объектного выражения и переопределение метода.** Создайте абстрактный класс  `Shape`  с абстрактным методом  `draw(Graphics2D g)`. В основной функции программы создайте объект класса  `Shape`  с помощью объектного выражения и переопределите метод  `draw(Graphics2D g)`  так, чтобы он выводил на графический контекст круг. Используйте графический контекст изображения типа `BufferedImage`. Сохраните изображение в файл формата PNG.
-  **Использование анонимного объекта для реализации обратного вызова.** Создайте интерфейс  `Callback`  с методом  `onResult(result: String)`. Создайте функцию  `getData(callback: Callback)`, которая вызывает метод  `onResult()`  с некоторым строковым значением. В основной функции программы создайте анонимный объект, реализующий интерфейс  `Callback`, и передайте его в функцию  `getData()`. Реализуйте метод  `onResult()`  так, чтобы он выводил на экран переданное ему значение.
- **Использование анонимного объекта в качестве возвращаемого значения.** Создайте интерфейс `Shape`  с абстрактным методом  `draw(g: Graphics2D)`. Создайте функцию  `createShape(type: String): Shape`, которая принимает строку  `type`  и возвращает анонимный объект, представляющий собой соответствующую фигуру. В зависимости от значения  `type`, функция должна возвращать объект, который содержит метод  `draw(g: Graphics2D)`, выводящий на экран заданную фигуру.
Возможные значения  `type`:
-   `"circle"` --- возвращает объект, представляющий круг.   
-   `"square"` --- возвращает объект, представляющий квадрат.
-   `"triangle"` --- возвращает объект, представляющий треугольник.  
В основной функции программы вызовите функцию  `createShape`  с разными значениями  `type`  и вызовите метод  `draw(g: Graphics2D)`  для каждого возвращенного объекта. Выведите фигуры на графический контекст изображения типа `BufferedImage`. Сохраните изображение в файл формата PNG.

**Часть II**
**Разработка информационной системы управления онлайн-курсами.**
Разработать информационную систему управления онлайн-курсами на основе принципов объектно-ориентированного программирования (ООП) с использованием наследования и реализации интерфейсов. Система должна обеспечивать хранение создаваемых объектов (курсов, студентов, преподавателей и пр.) в коллекциях и предоставлять функциональность для управления ими.

Требования:
- Создайте интерфейс  `Course`  со следующими методами:
    -   `fun getTitle(): String`  - возвращает название курса.        
    -   `fun getInstructor(): String`  - возвращает имя преподавателя курса.
    -   `fun getDuration(): Int`  - возвращает продолжительность курса в часах.
    -   `fun getDescription(): String`  - возвращает описание курса.
- Создайте абстрактный класс  `BaseCourse`, реализующий интерфейс  `Course`.
	-   Добавьте свойства: `title: String` (название курса); `instructor: String` (имя преподавателя курса); `duration: Int` (продолжительность курса в часах); `courseId: String` (уникальный идентификатор курса); `limit` (лимит количества студентов, которые могут быть записаны на курс).
    -   Реализуйте методы  `getTitle()`,  `getInstructor()`  и  `getDuration()` и `getLimit()`.
    -   Добавьте абстрактный метод  `getCategory(): String`  - возвращает категорию курса.
- Создайте класс  `ProgrammingCourse`, расширяющий класс `BaseCourse`.
    -   Добавьте свойство  `language: String`  - язык программирования, который изучается на курсе.
    -   Реализуйте метод  `getCategory()`, возвращающий строку "Программирование".
    -   Реализуйте метод  `getDescription()`, возвращающий описание курса с указанием языка программирования.
- Создайте класс  `DesignCourse`, наследующийся от  `BaseCourse`.
    -   Добавьте свойство  `software: String`  - программное обеспечение, которое используется на курсе.
    -   Реализуйте метод  `getCategory()`, возвращающий строку "Дизайн".
    -   Реализуйте метод  `getDescription()`, возвращающий описание курса с указанием используемого программного обеспечения.
- Создайте интерфейс  `CourseUser`  со следующими методами:
    -   `fun getName(): String`  - возвращает имя пользователя.   
    -   `fun getRole(): String`  - возвращает роль пользователя (студент, преподаватель и т.д.).
    -   `fun enrollInCourse(course: Course): Boolean`  - позволяет пользователю записаться на курс и возвращает `true`, если он был успешно записан (будем считать, что он был успешно записан, если лимит студентов на курсе не был превышен), и `false` в противном случае.
    -   `fun completeCourse(course: Course)`  - позволяет пользователю завершить курс.
- Создайте класс  `Student`, реализующий интерфейс  `CourseUser`.   
    -   Добавьте свойство  `studentId: String`  - уникальный идентификатор студента.  
    -   Реализуйте методы  `getName()`,  `getRole()`,  `enrollInCourse()`  и  `completeCourse()`.
- Создайте класс  `Instructor`, реализующий интерфейс  `CourseUser`.    
    -   Добавьте свойство  `specialization: String`  - специализация преподавателя.    
    -   Реализуйте методы  `getName()`,  `getRole()`,  `enrollInCourse()`  и  `completeCourse()`.
- Создайте класс  `CourseManagementSystem`, который будет управлять всеми объектами (курсами, студентами, преподавателями и пр.) и предоставлять методы для их добавления, удаления, поиска и других операций. Класс должен содержать следующие методы:
	-   `fun addCourse(course: Course)`  — добавляет курс в систему.
	-   `fun removeCourse(course: Course)`  — удаляет курс из системы.
	-   `fun getCourses(): List<Course>`  — возвращает список всех курсов.
	-   `fun addStudent(student: Student)`  — добавляет студента в систему.
	-   `fun removeStudent(student: Student)`  — удаляет студента из системы.
	-   `fun getStudents(): List<Student>`  — возвращает список всех студентов.
	-   `fun addInstructor(instructor: Instructor)`  — добавляет преподавателя в систему.
	-   `fun removeInstructor(instructor: Instructor)`  — удаляет преподавателя из системы.
	-   `fun getInstructors(): List<Instructor>`  — возвращает список всех преподавателей.
	-   `fun enrollStudentInCourse(student: Student, course: Course): Boolean`  — позволяет студенту записаться на курс.
	-   `fun completeCourseForStudent(student: Student, course: Course)`  — позволяет студенту завершить курс.
 
 **Часть III**
**Разработка многослойной иерархии интерфейсов для системы управления университетом.**
Создайте многослойную иерархию интерфейсов для системы управления университетом. Система должна включать в себя различные модули, такие как управление студентами, управление преподавателями, управление курсами и управление финансами. Каждый модуль должен иметь свои специфические функции, но также должен уметь взаимодействовать с другими модулями через общие интерфейсы. 

Требования:
-  Интерфейс  `Module`:   
    -   Базовый интерфейс для всех модулей.       
    -   Содержит метод  `getStatus(): String`, который возвращает текущий статус модуля.        
-  Интерфейс  `StudentManagementModule`:
    -   Расширяет интерфейс  `Module`.     
    -   Содержит методы  `enrollStudent(student: Student)`,  `expelStudent(student: Student)`  и  `getStudentList(): List<Student>`. 
-  Интерфейс  `TeacherManagementModule`:
    -   Расширяет интерфейс  `Module`.
    -   Содержит методы  `hireTeacher(teacher: Teacher)`,  `fireTeacher(teacher: Teacher)`  и  `getTeacherList(): List<Teacher>`.
-  Интерфейс  `CourseManagementModule`:
    -   Расширяет интерфейс  `Module`.
    -   Содержит методы  `createCourse(course: Course)`,  `deleteCourse(course: Course)`  и  `getCourseList(): List<Course>`.
-  Интерфейс  `FinancialManagementModule`:
    -   Расширяет интерфейс  `Module`.
    -   Содержит методы  `calculateTuitionFee(student: Student): Double`,  `calculateSalary(teacher: Teacher): Double`  и  `getTotalBudget(): Double`.
-  Классы  `Student`,  `Teacher`,  `Course`:
    -   Классы, представляющие студентов, преподавателей и курсы соответственно.      
    -   Каждый класс должен иметь свои атрибуты и методы для работы с ними.
-  Класс  `University`:
    -   Содержит экземпляры всех модулей.
    -   Предоставляет методы для управления университетом, такие как  `admitStudent(student: Student)`,  `hireTeacher(teacher: Teacher)`,  `createCourse(course: Course)`,  `calculateTotalBudget()`.

## Вопросы
1. Интерфейсы
2. Реализация интерфейсов
3. Свойства в интерфейсах
4. Функциональные интерфейсы
5. Наследование интерфейсов
6. Объявления объектов
7. Объекты-синглтоны
8. Объекты-компаньоны
9. Анонимные объекты
10. Наследование анонимных объектов от супертипов
11. Анонимные объекты как параметры функции
12. Анонимные объекты как возвращаемые типы значений

## Ресурсы
- [Kotlin. Программирование для профессионалов. 2-е изд. Скин Д., Гринхол Д., Бэйли Э.](https://www.piter.com/collection/yazyki-programmirovaniya/product/kotlin-programmirovanie-dlya-professionalov-2-e-izd) Главы 16, 17
- [Kotlin docs: Interfaces](https://kotlinlang.org/docs/interfaces.html)
- [Object declarations and expressions](https://kotlinlang.org/docs/object-declarations.html)
- [Руководство по языку Kotlin: Интерфейсы](https://kotlinlang.ru/docs/interfaces.html)
- [Руководство по языку Kotlin: Анонимные объекты и объявление объектов](https://kotlinlang.ru/docs/object-declarations.html)