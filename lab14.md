
# 14. Строители

_Строители_ (builders) — это механизм для создания сложных объектов или структур данных с использованием предметно-ориентированных языков — DSL (Domain-Specific Languages). Строители позволяют писать код в виде декларативного описания, что делает его более читаемым и удобным для использования.

Основные концепции строителей:
-  _Лямбда-выражение с объектом-приёмником_ позволяет вызывать методы на объекте-приёмнике внутри лямбда-выражения.    
-  _Функция-строитель_ принимает лямбда-выражение с объектом-приёмником и использует его для построения объекта.    
-  _Контекст_ — объект-приёмник, на котором вызываются методы внутри лямбда-выражения.

## Лямбда-выражение с объектом-приёмником

_Лямбда-выражение с объектом-приёмником (Lambda with Receiver)_ позволяют использовать объект-приемник внутри лямбда-выражения.

Синтаксис объявления _функционального типа с объектом-приёмником_:
```kotlin
ReceiverType.(p1, p2, ..., pn) -> ReturnType
```
где
-   `ReceiverType`  — тип объекта-приёмника.    
-   `p1, p2, ..., pn`  — параметры функции.  
-   `ReturnType`  — возвращаемый тип функции.

Лямбда-выражение с объектом-приёмником является реализацией функционального типа с объектом-приёмником.
```kotlin
val lambdaWithReceiver: ReceiverType.(p1, p2, ..., pn) -> ReturnType = {
    // Тело лямбда-выражения
    // Доступ к приемнику через this
}
```

Пример использования:
```kotlin
fun main() {
    val greet: String.(Int) -> String = { times -> this.repeat(times) }

    val result = "Hello".greet(3)
    println(result) // Вывод: HelloHelloHello
}
```
В этом примере `String.(Int) -> String` — это функциональный тип с объектом-приёмником типа `String`, который принимает один параметр типа `Int` и возвращает значение типа `String`. Внутри лямбда-выражения `this` ссылается на объект-приёмник лямбда-выражения (в данном случае строку `"Hello"`).

## Создание DSL с помощью функций-строителей

_Функция-строитель (builder function)_ — это функция, которая принимает лямбда-выражение с приёмником и использует его для построения объекта.

Простой пример DSL для генерации HTML-документа:
```kotlin
// Функция-строитель с единственным параметром init  
// функционального типа с приемником  
fun html(init: HTML.() -> Unit): HTML {  
	// Создается новый экземпляр HTML,  
	// который будет использоваться в качестве объекта-приемника  
	val html = HTML()  
	// Вызов функции, которая была передана в аргументе  
	html.init()  
	return html  
}  
  
// Тип объектов-приемников  
class HTML {  
	val body = StringBuilder()  
	  
	fun body(content: String) {  
		body.append(content)  
	}  
  
	override fun toString(): String {  
		return "<html><body>$body</body></html>"  
	}  
}  
  
fun main() {  
	// Вызов функции-строителя с лямбда-выражением в качестве аргумента,  
	// в котором this ссылается на объект-приемник (в данном случае объект типа HTML)  
	val htmlContent = html {  
		this.body("Hello, World!")  
	}
	  
	println(htmlContent) // Вывод: <html><body>Hello, World!</body></html>  
}
```
В этом примере `html` — это функция-строитель, которая принимает лямбда-выражение с приёмником типа `HTML`. Внутри лямбда-выражения вызывается метод `body`, который добавляет содержимое в объект `HTML`.

Как правило, ключевое слово this не используют явным образом, поэтому вызов функции строителя можно переписать в следующем виде:
```kotlin
html {
	body("Hello, World!")  
}
```

Расширенный пример DSL для генерации HTML-документа:
```kotlin
// Функция-строитель объектов типа HTML  
fun html(init: HTML.() -> Unit): HTML {  
	return HTML().apply(init)  
}  
  
// Класс объектов-приемников  
class HTML {  
	private val children = mutableListOf<HTMLTag>()  
	  
	// Функция-строитель объектов типа Head  
	fun head(init: Head.() -> Unit) {  
		children.add(Head().apply(init))  
	}  
	  
	// Функция-строитель объектов типа Body  
	fun body(init: Body.() -> Unit) {  
		children.add(Body().apply(init))  
	}  
	  
	override fun toString(): String {  
		return "<html>${children.joinToString("")}</html>"  
	}  
}  
  
interface HTMLTag  
  
// Класс объектов-приемников для функции head  
class Head : HTMLTag {  
	private val children = mutableListOf<HTMLTag>()  
	  
	fun title(text: String) {  
		children.add(Title(text))  
	}  
  
	override fun toString(): String {  
		return "<head>${children.joinToString("")}</head>"  
	}  
}  
  
// Класс объектов-приемников для функции body  
class Body : HTMLTag {  
	private val children = mutableListOf<HTMLTag>()  
	  
	fun p(text: String) {  
		children.add(P(text))  
	}  
	  
	override fun toString(): String {  
		return "<body>${children.joinToString("")}</body>"  
	}  
}  
  
class Title(private val text: String) : HTMLTag {  
	override fun toString(): String {  
		return "<title>$text</title>"  
	}  
}  
  
class P(private val text: String) : HTMLTag {  
	override fun toString(): String {  
		return "<p>$text</p>"  
	}  
}  
  
fun main() {  
	val htmlDocument = html {  
		head {  
		title("My Page")  
	}  
	body {  
		p("Hello, World!")  
	}  
}  
  
println(htmlDocument)  
// Вывод: <html><head><title>My Page</title></head><body><p>Hello, World!</p></body></html>  
}
```
В этом примере функция-строитель `html` принимает лямбда-выражение с объектом-приемником типа `HTML`. Внутри этого лямбда-выражения можно вызывать методы `head` и `body`, которые сами по себе также являются функциями-строителями и также принимают лямбда-выражения с объектами-приемниками типов `Head` и `Body` соответственно.

## Задание

**Часть I** 

Создайте DSL для генерации форматированного текста, в котором могут быть установлены шрифт, размер и цвет текста в виде HTML-разметки.

Пример использования DSL:
```kotlin
formatText {
	text("Hello, World!")
    font("Arial")
    size(14)
    color("#FF0000")
}
```
Вывод:
```html
<span style="font-family: Arial; font-size: 14px; color: #FF0000;">Hello, World!</span>
```

**Часть II**

Создайте DSL для генерации HTML-таблиц, который позволяет указывать заголовки столбцов и строки данных. 

Пример использования DSL:
```kotlin
table {
    headers("Name", "Age", "Email")
    row("John Doe", 30, "john.doe@example.com")
    row("Jane Smith", 25, "jane.smith@example.com")
}
```
Вывод:
```html
<table>
    <thead>
        <tr>
            <th>Name</th>
            <th>Age</th>
            <th>Email</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>John Doe</td>
            <td>30</td>
            <td>john.doe@example.com</td>
        </tr>
        <tr>
            <td>Jane Smith</td>
            <td>25</td>
            <td>jane.smith@example.com</td>
        </tr>
    </tbody>
</table>
```
   
 **Часть III**

Создайте DSL для генерации SQL-запросов, который позволяет указывать таблицу, столбцы, условия и сортировку. 

Пример использования DSL:
```kotlin
sql {
	select("name", "age")
    from("users")
    where {
	    "age" greaterThan 18
        "name" equalTo "John"
    }
    orderBy("age", descending = true)
}
```
Вывод:
```sql
SELECT name, age FROM users WHERE age > 18 AND name = 'John' ORDER BY age DESC
```

## Вопросы


## Ресурсы
- [Kotlin docs: Type-safe builders](https://kotlinlang.org/docs/type-safe-builders.html)
- [Руководство по языку Kotlin: Типобезопасные строители](https://kotlinlang.ru/docs/type-safe-builders.html)