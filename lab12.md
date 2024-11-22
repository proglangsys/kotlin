
# 12. Делегирование. Псевдонимы

## Делегирование

_Делегирование (Delegation)_ — это механизм, который позволяет классу делегировать частично или полностью обязанность по реализации поведения своих объектов некоторому объекту другого класса (такой объект принято называть _объектом-делегатом_ или просто _делегатом_, а класс таких объектов --- _классом-делегатом_).

Класс **_C_** делегирует реализацию членов **_M1,...,Mn_**, унаследованных от некоторого интерфейса **_C_** , классу-делегату **_D_**. Делегирование осуществляется за счет того, что создается объект-делегат (экземпляр класса-делегата, реализующего данный интерфейс) **_d_**, который является частью любого объекта  **_c_** делегирующего класса  **_C_**. При обращении к членам **_M1,...,Mn_** класса **_C_**, объект-делегат **_d_** выполняет соответствующие обязанности по реализации поведения объекта **_c_**.

Делегирование позволяет избежать использования наследования и вместо наследования использовать композицию для повторного использования кода. 

**Преимущества делегирования:**
-   _Гибкость:_  Можете изменять поведение объекта, просто заменяя объект-делегат, не изменяя код самого объекта.
-   _Повторное использование кода:_  класс-делегат может быть использован в нескольких классах, что уменьшает дублирование кода.
-   _Множественное делегирование:_ класс может делегировать реализацию нескольких интерфейсов разным объектам-делегатам.

**Пример делегирования:**

1. Создается интерфейс `BaseInterface`, включающий объявление одного или нескольких абстрактных методов.
```kotlin
interface BaseInterface {
    fun doSomething()
}
```
2. Создается класс-делегат `BaseImpl`, реализующий интерфейс `BaseInterface`.
```kotlin
class BaseImpl: BaseInterface {
    override fun doSomething() {
        println("Делаю что-то в делегате")
    }
}
```
3. Создается класс `Derived`, делегирующий выполнение методов, унаследованных от интерфейса `BaseInterface`, делегату `delegate` (объекту класса `BaseImpl`). >
>_Применение делегирования указывается ключевым словом  `by`. Включение параметра `delegate` в первичный конструктор является обязательным._
```kotlin
class Derived(private val delegate: BaseImpl) : BaseInterface by delegate {
    /* 
	   Класс MyClass не реализует абстрактные методы, 
       унаследованные от интерфейса MyInterface, 
       поскольку он делегирует реализацию MyInterface объекту delegate
    */
}
```
5. При создании объекта класса `Derived` необходимо передать делегат `delegate` в соответствующий конструктор.
```kotlin
fun main() {
    val delegate = BaseImpl() // Создать делегат
    val myObj = Derived(delegate) // Передать делегат в конструктор делегирующего класса
    myObj.doSomething()  // Вывод: Делаю что-то в делегате
}
```

### Переопределение члена интерфейса, реализованного делегированием

При делегировании реализации некоторого интерфейса, члены этого интерфейса можно переопределить в классе, который делегирует эту реализацию.

**Пример переопределения члена интерфейса, реализованного делегированием:**

1. Создается интерфейс `BaseInterface`  и реализующий его класс-делегат `BaseImpl`
```kotlin
interface BaseInterface {
    val message: String
    fun doSomething()
    fun doSomethingElse()
}

class BaseImpl : BaseInterface {
    override val message = "I am from BaseImpl"
    override fun doSomething() { println("Doing something in BaseImpl") }
    override fun doSomethingElse() { println("Doing something else in BaseImpl") }
}
```

2. Создается класс `Derived`, делегирующий выполнение методов, унаследованных от интерфейса `BaseInterface`, делегату `delegate` (объекту класса `BaseImpl`). При этом  унаследованный метод  `doSomething`  переопределяется в классе  `MyClass`.

```kotlin
class Derived(delegate: BaseInterface ) : BaseInterface by delegate {
    override val message = "I am from Derived"
    override fun doSomething() { println("Doing something in Derived") }
}

fun main() {
    val delegate = BaseImpl()
    val derived = Derived(delegate)
    derived.doSomething()
    print(derived.message)
    derived.doSomething()
    derived.doSomethingElse()
}
```

### Множественное делегирование

_Множественное делегирование (multiple delegation)_ позволяет классу делегировать реализацию нескольких интерфейсов разным объектам-делегатам.

```kotlin
interface InterfaceA {
    fun doSomethingA()
}

interface InterfaceB {
    fun doSomethingB()
}

class DelegateA : InterfaceA {
    override fun doSomethingA() {
        println("Делаю что-то в делегате A")
    }
}

class DelegateB : InterfaceB {
    override fun doSomethingB() {
        println("Делаю что-то в делегате B")
    }
}

class MyClass(val delegateA: DelegateA, val delegateB: DelegateB) : 
	InterfaceA by delegateA, InterfaceB by delegateB {
    // MyClass делегирует выполение методов InterfaceA объекту delegateA,
    // а выполение методов InterfaceB объекту delegateB
}

fun main() {
    val delegateA = DelegateA()
    val delegateB = DelegateB()
    val myClass = MyClass(delegateA, delegateB)

    myClass.doSomethingA()  // Вывод: Делаю что-то в делегате A
    myClass.doSomethingB()  // Вывод: Делаю что-то в делегате B
}
```

### Делегирование свойств

_Делегирование свойств (Delegated Properties)_ — это механизм, который позволяет делегировать логику получения и установки значений свойств другому объекту.

**Пример делегирования свойства:**

Класс-делегат `Delegate`  реализует логику для получения и установки значения свойства. Он должен реализовать два оператора:  `getValue`  и  `setValue`.

```kotlin
import kotlin.reflect.KProperty  
  
class Delegate {  
	private var value: Int = 0  
	  
	operator fun getValue(thisRef: Any?, prop: KProperty<*>): Int {  
		return value  
	}  
	operator fun setValue(thisRef: Any?, prop: KProperty<*>, newValue: Int) {  
		value = newValue  
	} 
}
```

В классе  `MyClass`  свойство  `myProperty`  делегируется классу  `Delegate`  с помощью ключевого слова  `by`.
```kotlin
class MyClass {
    var myProperty: Int by Delegate()
}
```

При получении значения свойства  `myProperty`  вызывается метод  `getValue`  класса-делегата. При установке значения свойства  `myProperty`  вызывается метод  `setValue`  класса-делегата.
```kotlin
fun main() {
    val myObj = MyClass()
    print(myObj.myProperty)  // Вывод: 0
    myObj.myProperty = 42    
    print(myObj.myProperty)  // Вывод: 42
}
```

### Стандартные делегаты

Kotlin предоставляет несколько стандартных делегатов, которые можно использовать для делегирования свойств.

Делегат свойства с отложенной инициализацией (`lazy`):
```kotlin
val lazyValue: String by lazy {
    println("Вычисляю отложенное значение")
    "Hello"
}

fun main() {
    println(lazyValue)  // Вывод: Вычисляю отложенное значение
                        // Hello
    println(lazyValue)  // Вывод: Hello
}
```

Делегат свойства, которое уведомляет о своих изменениях (`observable`):
```kotlin
import kotlin.properties.Delegates  
  
class User {  
	var name: String by Delegates.observable("No name") {  
		prop, old, new ->  
		println("Имя изменено с $old на $new")  
	}  
}  
  
fun main() {  
	val user = User()  
	user.name = "Alice" // Вывод: Имя изменено с No name на Alice  
	user.name = "Bob" // Вывод: Имя изменено с Alice на Bob  
}
```

Делегат свойства, которое может отклонить изменения (`vetoable`):
```kotlin
import kotlin.properties.Delegates  
  
class User {  
	var age: Int by Delegates.vetoable(0) {  	
		prop, old, new ->  
		println("Попытка изменить возраст с $old на $new")  
		new > old  
	}  
}  
  
fun main() {  
	val user = User()  
	user.age = 42 // Вывод: Попытка изменить возраст с 0 на 42  
	println(user.age) // Вывод: 42  
	user.age = 41 // Вывод: Попытка изменить возраст с 42 на 41  
	println(user.age) // Вывод: 42  
}
```

## Псевдонимы типов

_Псевдонимы типов (Type Aliases)_ — это механизм, который позволяет создавать альтернативные имена для существующих типов.

Псевдонимы типов используются для улучшение читаемости кода: они могут сделать код более читаемым, особенно если используются длинные и сложные типы.    
    
-   **Создание более описательных имен:**  Псевдонимы типов могут помочь вам создать более описательные имена для общих типов, что улучшает понимание кода.

Создание псевдонима типа с помощью ключевого слова  `typealias`.
```kotlin
typealias Name = String
typealias Age = Int

fun main() {
    val name: Name = "Alice"
    val age: Age = 30

    println("Имя: $name, Возраст: $age")
}
```

### Использование псевдонимов для функциональных типов

Псевдонимы типов могут использоваться для обозначения функциональных типов.
```kotlin
typealias StringMapper = (String) -> String

fun main() {
    val upperCaseMapper: StringMapper = { it.toUpperCase() }
    val lowerCaseMapper: StringMapper = { it.toLowerCase() }

    println(upperCaseMapper("Hello"))  // Вывод: HELLO
    println(lowerCaseMapper("WORLD"))  // Вывод: world
}
```

### Использование псевдонимов для типов коллекций

Псевдонимы типов могут использоваться для создания более описательных имен для типов коллекций.
```kotlin
typealias UserList = List<User>

data class User(val name: String, val age: Int)

fun main() {
    val users: UserList = listOf(
        User("Alice", 30),
        User("Bob", 25)
    )

    users.forEach { println("Имя: ${it.name}, Возраст: ${it.age}") }
}
```

## Задание

**Часть I** 
**Делегирование в классе.** Реализуйте систему управления задачами, используя делегирование.

Требования:
-  Интерфейс  `Taskable`:
    -   Содержит метод  `addTask(task: String)`, который добавляет задачу в список.
    -   Содержит метод  `listTasks(): List<String>`, который возвращает список задач.
-  Класс  `TaskList`:
    -   Хранит список задач.
    -   Реализует интерфейс  `Taskable`.
-  Класс  `User`:
    -   Хранит информацию о пользователе: имя и список задач.  
    -   Реализует интерфейс  `Taskable`, делегируя вызовы методов классу  `TaskList`.

**Переопределение члена интерфейса, реализованного делегированием.** 
Реализуйте систему управления заметками, используя делегирование и переопределение методов.

Требования:
-  Интерфейс  `Notable`:
    -   Содержит метод  `addNote(note: String)`, который добавляет заметку в список. 
    -   Содержит метод  `listNotes(): List<String>`, который возвращает список заметок.   
-  Класс  `NoteList`:
    -   Хранит список заметок.
    -   Реализует интерфейс  `Notable`.
-  Класс  `User`:
    -   Хранит информацию о пользователе: имя и список заметок.
    -   Реализует интерфейс  `Notable`, делегируя вызовы методов классу  `NoteList`.
    -   Переопределите метод  `listNotes()`, чтобы он возвращал список заметок с префиксом "Заметка: ".

**Часть II**

**Множественное делегирование и делегирование свойств** Реализуйте систему управления профилями пользователей и их настройками, используя множественное делегирование и делегирование свойств.
Требования:
-  Интерфейс  `User`:
    -   Содержит метод  `getName(): String`, который возвращает имя пользователя.
-  Интерфейс  `Settings`:
    -   Содержит метод  `getSettings(): Map<String, Any>`, который возвращает настройки пользователя.
-  Класс  `UserProfile`:
    -   Хранит информацию о профиле пользователя: имя.
    -   Реализует интерфейс  `User`.  
-  Класс  `UserSettings`:
    -   Хранит информацию о настройках пользователя: язык, тему.
    -   Реализует интерфейс  `Settings`.
- Класс  `UserManager`:
    -   Хранит информацию о профиле пользователя и его настройках.
    -   Реализует интерфейсы  `User`  и  `Settings`, используя множественное делегирование.
    -   Реализуйте свойства  `name`,  `language`, и  `theme`, делегируя их классам  `UserProfile`  и  `UserSettings`.
 
**Стандартные делегаты.** Реализуйте систему управления пользовательскими настройками, используя стандартные делегаты Kotlin.

Требования:
-  Класс  `UserSettings`:
    -   Хранит информацию о настройках пользователя: язык, тема, уведомления.  
    -   Используйте стандартный делегат  `lazy`  для инициализации списка настроек.   
    -   Используйте стандартный делегат  `observable`  для отслеживания изменений в настройках.
    -   Используйте стандартный делегат  `vetoable`  для валидации значений настроек.   
-  Класс  `SettingsManager`:
    -   Хранит информацию о настройках пользователя.
    -   Реализуйте свойства  `language`,  `theme`, и  `notifications`, делегируя их классу  `UserSettings`.

**Применение псевдонимов.** Реализуйте систему управления списком задач, используя псевдонимы для упрощения работы с типами данных.
Требования:
1.  Класс  `Task`:
    -   Хранит информацию о задаче: название и статус (выполнена/не выполнена).
2.  Класс  `TaskList`:
    -   Хранит список задач.
    -   Добавьте методы для:
        -   Добавления задачи в список.
        -   Вывода списка задач.   
-  Используйте псевдонимы для упрощения работы с типами данных:
    -   Создайте псевдоним для типа  `List<Task>`. 
    -   Создайте псевдоним для типа  `MutableList<Task>`.
   
 **Часть III**

Реализуйте систему управления онлайн-курсами, используя делегирование для разделения ответственности между классами и упрощения доступа к данным. Используйте псевдонимы для упрощения работы с интерфейсами и классами.

Требования:
-  Интерфейс  `Course`:
    -   Содержит метод  `getTitle(): String`, который возвращает название курса.
    -   Содержит метод  `getDescription(): String`, который возвращает описание курса.
-  Интерфейс  `Enrollable`:
    -   Содержит метод  `enroll(student: Student)`, который добавляет студента в список участников курса.
    -   Содержит метод  `getStudents(): List<Student>`, который возвращает список участников курса.  
-  Класс  `OnlineCourse`:
    -   Хранит информацию о курсе: название и описание.  
    -   Реализует интерфейс  `Course`. 
    -   Реализует интерфейс  `Enrollable`, используя делегирование.   
-  Класс  `CourseEnrollment`:
    -   Хранит список участников курса.  
    -   Реализует интерфейс  `Enrollable`. 
-  Класс  `Student`:
    -   Хранит информацию о студенте: имя и список курсов, на которые он записан.
    -   Реализует интерфейс  `Enrollable`, делегируя вызовы методов классу  `CourseEnrollment`.
    -   Переопределите метод  `enroll(course: Course)`, чтобы он добавлял курс в список курсов студента. 
-  Класс  `CourseManager`:
    -   Хранит информацию о курсе и его участниках.  
    -   Реализует интерфейсы  `Course`  и  `Enrollable`, используя множественное делегирование.
    -   Реализуйте свойства  `title`,  `description`, и  `students`, делегируя их классам  `OnlineCourse`  и  `CourseEnrollment`.

## Вопросы

## Ресурсы
- [Kotlin docs: Delegation](https://kotlinlang.org/docs/delegation.html)
- [Kotlin docs: Delegated properties](https://kotlinlang.org/docs/delegated-properties.html)
- [Kotlin docs: Type aliases](https://kotlinlang.org/docs/type-aliases.html)
- [Руководство по языку Kotlin: Делегирование](https://kotlinlang.ru/docs/delegation.html)
- [Руководство по языку Kotlin: Анонимные объекты и объявление объектов](https://kotlinlang.ru/docs/object-declarations.html)
- [Руководство по языку Kotlin: Псевдонимы типов](https://kotlinlang.ru/docs/type-aliases.html)