#### [Основы объявления функций](#Основы-объявления-функций-1)
#### [Именованные и стандартные аргументы](#Именованные-и-стандартные-аргументы-1)
#### [Функции с параметрами по умолчанию](#Функции-с-параметрами-по-умолчанию-1)
#### [Локальные функции](#Локальные-функции-1)
#### [Функции высшего порядка](#Функции-высшего-порядка-1)
#### [Inline-функции](#inline-функции-1)
#### [Расширяющие функции](#Расширяющие-функции-1)
#### [Пример использования в реальном проекте](#Пример-использования-в-реальном-проекте-1)

---
# Функции в Kotlin

Функции в Kotlin — это основной строительный блок, позволяющий структурировать, повторно использовать и улучшать читаемость кода. Kotlin предоставляет мощные возможности для работы с функциями, включая лаконичный синтаксис, поддержку лямбда-выражений и функций высшего порядка, а также встроенные инструменты для обработки коллекций и потоков данных.

## Основы объявления функций

Функции в Kotlin могут быть объявлены как на уровне файла, так и внутри классов или объектов. Ключевое слово `fun` используется для определения функции.

Пример:

```kotlin
fun greetUser(user: String): String {
    return "Hello, $user!"
}

println(greetUser("Vlad")) // Output: Hello, Vlad!
```

Особенности:
- Тип возвращаемого значения указывается после двоеточия `:`.
- Если функция ничего не возвращает, можно указать `Unit`, но это не обязательно.

Пример функции без возвращаемого значения:

```kotlin
fun printGreeting() {
    println("Welcome to Kotlin!")
}

printGreeting() // Output: Welcome to Kotlin!
```

---

## Именованные и стандартные аргументы

Kotlin поддерживает именованные аргументы, что делает вызовы функций более читаемыми и уменьшает вероятность ошибок.

Пример:

```kotlin
fun formatMessage(user: String, message: String, timestamp: String) {
    println("[$timestamp] $user: $message")
}

formatMessage(user = "Admin", message = "System rebooted", timestamp = "2025-01-13 14:00")
// Output: [2025-01-13 14:00] Admin: System rebooted
```

---

## Функции с параметрами по умолчанию

В Kotlin можно задавать значения по умолчанию для параметров, что упрощает вызов функций.

Пример:

```kotlin
fun greet(user: String = "Guest") {
    println("Hello, $user!")
}

greet() // Output: Hello, Guest!
greet("Kotlin Dev") // Output: Hello, Kotlin Dev!
```

---

## Локальные функции

Kotlin позволяет объявлять функции внутри других функций. Это удобно, если вспомогательная функция используется только в одном месте.

Пример:

```kotlin
fun processInput(input: String) {
    fun validate(input: String): Boolean {
        return input.isNotBlank()
    }

    if (validate(input)) {
        println("Processing input: $input")
    } else {
        println("Invalid input")
    }
}

processInput("Data") // Output: Processing input: Data
```

---

## Функции высшего порядка

Функции высшего порядка принимают другие функции в качестве аргументов или возвращают их. Это важный аспект функционального программирования.

Пример функции высшего порядка:

```kotlin
fun performOperation(a: Int, b: Int, operation: (Int, Int) -> Int): Int {
    return operation(a, b)
}

val result = performOperation(5, 3) { x, y -> x + y }
println(result) // Output: 8
```

Здесь в функцию `performOperation` передаётся лямбда-выражение, которое определяет операцию.

---

## Inline-функции

Для уменьшения накладных расходов при использовании функций высшего порядка Kotlin поддерживает inline-функции. Ключевое слово `inline` используется для указания, что функция должна быть встроена на месте вызова.

Пример:

```kotlin
inline fun measureExecutionTime(block: () -> Unit) {
    val start = System.nanoTime()
    block()
    val end = System.nanoTime()
    println("Execution time: ${end - start} ns")
}

measureExecutionTime {
    println("Running a task...")
} // Output: Running a task...\nExecution time: <time> ns
```

---

## Расширяющие функции

Расширяющие функции позволяют добавлять новые методы к существующим классам без их изменения.

Пример:

```kotlin
fun String.capitalizeWords(): String {
    return this.split(" ").joinToString(" ") { it.capitalize() }
}

val text = "welcome to kotlin"
println(text.capitalizeWords()) // Output: Welcome To Kotlin
```

---

## Пример использования в реальном проекте

Предположим, вы разрабатываете систему для обработки данных о заказах:

```kotlin
data class Order(val id: Int, val amount: Double, val customer: String)
val orders = listOf(
    Order(1, 250.0, "Corporate Client"),
    Order(2, 99.99, "Individual Buyer"),
    Order(3, 499.0, "Enterprise Partner")
)
// Для наглядного форматирования заказов используем следующую функцию:
fun List<Order>.printSummary() {
    forEach {
        println("Order #${it.id} for ${it.customer}: $${it.amount}")
    }
}
fun main() {
    orders.printSummary()
}

// Output:
// Order #1 for Corporate Client: $250.0
// Order #2 for Individual Buyer: $99.99
// Order #3 for Enterprise Partner: $499.0
```

