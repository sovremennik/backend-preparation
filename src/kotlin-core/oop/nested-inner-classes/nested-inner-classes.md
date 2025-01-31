#### [Введение](#Введение-1)
#### [Основные концепции](#Основные-концепции-1)
#### [Примеры кода на Kotlin](#Примеры-кода-на-kotlin-1)
#### [Ошибки и подводные камни](#Ошибки-и-подводные-камни-1)
#### [Сравнение с альтернативами](#Сравнение-с-альтернативами-1)
#### [Best practices и антипаттерны](#best-practices-и-антипаттерны-1)

---
# Вложенные и внутренние классы в Kotlin

## Введение
Вложенные и внутренние классы в Kotlin позволяют организовывать код, инкапсулируя логику, которая тесно связана с внешним классом. Они используются в реальных проектах для реализации паттернов проектирования (например, Builder, Factory), обработчиков событий, кастомных итераторов и других сценариев, где требуется логическая группировка или доступ к данным внешнего класса.

---

## Основные концепции

### Вложенные классы (Nested Classes)
- **Определение**: Класс, объявленный внутри другого класса. По умолчанию является статическим (не имеет доступа к членам внешнего класса).
- **Практическая польза**: Используется для логической группировки классов, которые связаны с внешним классом, но не требуют доступа к его данным.

### Внутренние классы (Inner Classes)
- **Определение**: Класс, объявленный с ключевым словом `inner`. Имеет доступ к членам внешнего класса.
- **Практическая польза**: Используется, когда требуется доступ к данным внешнего класса, например, для реализации обработчиков событий или кастомных итераторов.

---

## Примеры кода на Kotlin

### Вложенный класс: Реализация паттерна Builder
```kotlin
class HttpClient {
    private var baseUrl: String = ""
    private var timeout: Int = 1000

    // Вложенный класс для построения конфигурации
    class Builder {
        private val httpClient = HttpClient()

        fun setBaseUrl(baseUrl: String): Builder {
            httpClient.baseUrl = baseUrl
            return this
        }

        fun setTimeout(timeout: Int): Builder {
            httpClient.timeout = timeout
            return this
        }

        fun build(): HttpClient {
            return httpClient
        }
    }

    fun sendRequest() {
        println("Sending request to $baseUrl with timeout $timeout ms")
    }
}

// Использование
val client = HttpClient.Builder()
    .setBaseUrl("https://api.example.com")
    .setTimeout(5000)
    .build()
client.sendRequest()
```

### Внутренний класс: Обработчик событий в UI
```kotlin
class Button {
    private var onClickListener: OnClickListener? = null

    // Внутренний класс для обработки событий
    inner class OnClickListener {
        fun onClick() {
            println("Button clicked!")
        }
    }

    fun setOnClickListener(listener: OnClickListener) {
        this.onClickListener = listener
    }

    fun click() {
        onClickListener?.onClick()
    }
}

// Использование
val button = Button()
val listener = button.OnClickListener()
button.setOnClickListener(listener)
button.click()
```

### Вложенный класс: Логическая группировка
```kotlin
class Database {
    private val connectionString = "jdbc:postgresql://localhost:5432/mydb"

    // Вложенный класс для работы с запросами
    class QueryBuilder {
        fun buildQuery(table: String, columns: List<String>): String {
            return "SELECT ${columns.joinToString(", ")} FROM $table"
        }
    }
}

// Использование
val query = Database.QueryBuilder().buildQuery("users", listOf("id", "name"))
println(query) // SELECT id, name FROM users
```

### Внутренний класс: Кастомный итератор
```kotlin
class TaskList {
    private val tasks = mutableListOf("Task 1", "Task 2", "Task 3")

    // Внутренний класс для итерации по задачам
    inner class TaskIterator {
        private var index = 0

        fun hasNext(): Boolean {
            return index < tasks.size
        }

        fun next(): String {
            return tasks[index++]
        }
    }

    fun getIterator(): TaskIterator {
        return TaskIterator()
    }
}

// Использование
val taskList = TaskList()
val iterator = taskList.getIterator()
while (iterator.hasNext()) {
    println(iterator.next())
}
```

---

## Ошибки и подводные камни

1. **Путаница между вложенными и внутренними классами**:
    - Ошибка: Использование вложенного класса, когда требуется доступ к данным внешнего класса.
    - Решение: Используйте `inner` для доступа к данным внешнего класса.

2. **Утечки памяти в внутренних классах**:
    - Ошибка: Внутренний класс сохраняет ссылку на внешний класс, что может привести к утечкам памяти.
    - Решение: Используйте вложенные классы, если доступ к данным внешнего класса не требуется.

---

## Сравнение с альтернативами

| Критерий          | Вложенные классы               | Внутренние классы              |
|-------------------|--------------------------------|--------------------------------|
| **Доступ к данным внешнего класса** | Нет                          | Да                             |
| **Память**        | Не хранит ссылку на внешний класс | Хранит ссылку на внешний класс |
| **Использование** | Логическая группировка         | Требуется доступ к данным внешнего класса |
| **Плюсы**         | Легковесные, изолированные     | Прямой доступ к данным         |
| **Минусы**        | Ограниченная функциональность  | Риск утечек памяти             |

---

## Best practices и антипаттерны

### Best practices
1. **Используйте вложенные классы для логической группировки**:
   ```kotlin
   class ApiClient {
       class RequestBuilder {
           fun buildRequest(url: String): String {
               return "GET $url"
           }
       }
   }
   ```

2. **Используйте внутренние классы для доступа к данным внешнего класса**:
   ```kotlin
   class TaskManager {
       private val tasks = mutableListOf<String>()

       inner class TaskHandler {
           fun addTask(task: String) {
               tasks.add(task)
               println("Task added: $task")
           }
       }
   }
   ```

3. **Избегайте утечек памяти с помощью вложенных классов**:
   ```kotlin
   class DataProcessor {
       class ProcessorConfig {
           fun configure() {
               println("Configuring processor")
           }
       }
   }
   ```

### Антипаттерны
1. **Избыточное использование внутренних классов**:
    - Проблема: Утечки памяти и усложнение кода.
    - Пример:
      ```kotlin
      class Outer {
          private val data = "Sensitive data"
 
          inner class Inner {
              fun leakData() {
                  println(data) // Утечка данных
              }
          }
      }
      ```

2. **Использование вложенных классов, когда требуется доступ к данным внешнего класса**:
    - Проблема: Недоступность данных внешнего класса.
    - Пример:
      ```kotlin
      class Outer {
          private val data = "Sensitive data"
 
          class Nested {
              fun printData() {
                  // Ошибка: Нет доступа к `data`
                  // println(data)
              }
          }
      }
      ```

---
