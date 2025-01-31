#### [Условные выражения](#Условные-выражения-1)
#### [Циклы](#Циклы-1)
#### [Управляющие операторы](#Управляющие-операторы-1)

---
# Управляющие конструкции

Управляющие конструкции в Kotlin обеспечивают гибкость и выразительность при построении алгоритмов. Они включают условные выражения, циклы и управляющие операторы, позволяя эффективно управлять потоком выполнения программы. Управляющие конструкции Kotlin сочетают в себе гибкость, лаконичность и выразительность, позволяя писать чистый и понятный код. Их правильное использование помогает строить эффективные и читаемые алгоритмы.

## Условные выражения

### if-else

Конструкция `if` в Kotlin является выражением, что позволяет использовать её для вычисления значений.

```kotlin
val max = if (a > b) a else b
```

Для более сложных условий можно использовать блоки:

```kotlin
val result = if (score >= 90) {
    "Excellent"
} else if (score >= 75) {
    "Good"
} else {
    "Needs Improvement"
}
```

### when

Конструкция `when` заменяет цепочки `if-else if` и может использоваться как оператор или выражение.

```kotlin
val response = when (statusCode) {
    200 -> "OK"
    404 -> "Not Found"
    500 -> "Internal Server Error"
    else -> "Unknown Status"
}
```

`when` поддерживает проверки на диапазоны, типы и условия:

```kotlin
val message = when {
    input.isEmpty() -> "Input is empty"
    input.length > 100 -> "Input is too long"
    else -> "Valid input"
}
```

## Циклы

### Диапазоны

Диапазоны (`ranges`) позволяют работать с последовательностями чисел, символов или любых элементов, реализующих интерфейс `Comparable`. Основные функции:

- `..`: Создаёт включённый диапазон (от начального до конечного значения).

  ```kotlin
  for (i in 1..5) {
      println(i) // 1, 2, 3, 4, 5
  }
  ```

- `until`: Исключает конечное значение из диапазона.

  ```kotlin
  for (i in 1 until 5) {
      println(i) // 1, 2, 3, 4
  }
  ```

- `downTo`: Создаёт убывающий диапазон.

  ```kotlin
  for (i in 5 downTo 1) {
      println(i) // 5, 4, 3, 2, 1
  }
  ```

- `step`: Определяет шаг для итерации.

  ```kotlin
  for (i in 1..10 step 2) {
      println(i) // 1, 3, 5, 7, 9
  }
  ```

Диапазоны также применяются в условиях и функциях:

```kotlin
if (x in 1..10) {
    println("x входит в диапазон")
}
```

### while и do-while

Конструкция `while` выполняет блок кода, пока условие истинно:

```kotlin
while (counter > 0) {
    println("Counter: $counter")
    counter--
}
```

Цикл `do-while` гарантирует выполнение тела хотя бы один раз:

```kotlin
do {
    println("Processing item $item")
    item = getNextItem()
} while (item != null)
```

### for

Цикл `for` используется для итерации по коллекциям, диапазонам и последовательностям:

```kotlin
for (i in 1..10) {
    println("Index: $i")
}
```

Итерация с шагом и в обратном порядке:

```kotlin
for (i in 10 downTo 1 step 2) {
    println("Index: $i")
}
```

Итерация по коллекциям:

```kotlin
val employees = listOf("Manager", "Developer", "Designer")
for (role in employees) {
    println("Role: $role")
}
```

Итерация с индексом:

```kotlin
for ((index, role) in employees.withIndex()) {
    // Здесь `index` — это позиция элемента в коллекции (начиная с 0), а `role` — сам элемент коллекции
    println("Index: $index, Role: $role")
} {
    println("Role at index $index is $role")
}
```

## Управляющие операторы

### return

Оператор `return` используется для выхода из функции. В лямбда-выражениях его поведение отличается в зависимости от контекста:

```kotlin
fun findFirstEven(numbers: List<Int>): Int? {
    numbers.forEach {
        if (it % 2 == 0) return it // Выход из всей функции
    }
    return null
}
```

Для возврата только из лямбды используйте `return@label`:

```kotlin
numbers.forEach label@{
    if (it % 2 == 0) return@label
}
```

### break и continue

- `break` завершает выполнение текущего цикла:

  ```kotlin
  for (i in 1..10) {
      if (i == 5) break
      println(i)
  }
  ```

- `continue` пропускает текущую итерацию и переходит к следующей:

  ```kotlin
  for (i in 1..10) {
      if (i % 2 == 0) continue
      println(i)
  }
  ```

### Метки (Labels)

Метки (`labels`) — это механизм управления потоком выполнения программы, позволяющий влиять на вложенные циклы или лямбда-выражения. Они полезны для сложных конструкций, где требуется прерывать или продолжать выполнение не текущего, а внешнего цикла. Используются для управления вложенными циклами и лямбда-выражениями. Метка определяет идентификатор, перед которым ставится символ `@`:

- **Прерывание внешнего цикла с помощью break:**

  ```kotlin
  data class Employee(val name: String, val position: String)
  val employees = listOf(
      Employee("Viktor Lebedev", "Marketing Specialist"),
      Employee("Olga Mikhailova", "Senior Developer"),
      Employee("Dmitry Kuznetsov", "Sales Manager")
  )
  outer@ for (employee in employees) {
      if (employee.position == "Senior Developer") {
          println("Duty developer found: ${employee.name}, position: ${employee.position}")
          break@outer // Прерываем внешний цикл, если находим дежурного разработчика
      }
  }
  ```

  В этом примере, как только найден "Backend Developer", выполнение внешнего цикла завершается.

- **Продолжение внешнего цикла с помощью continue:**

  ```kotlin
  outer@ for (project in listOf("Project A", "Project B")) {
      for (task in listOf("Task 1", "Task 2")) {
          if (task == "Task 2") continue@outer
          println("Project: $project, Task: $task")
      }
  }
  ```

  При достижении условия цикл `task` завершается и сразу переходит к следующей итерации цикла `project`.

Метки также применяются для возврата из лямбда-выражений в контексте их вызова:

```kotlin
run loop@{
    listOf(1, 2, 3, 4).forEach {
        if (it == 3) return@loop
        println(it)
    }
}
```
