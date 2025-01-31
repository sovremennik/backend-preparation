#### [Введение](#Введение-1)
#### [Основные концепции](#Основные-концепции-1)
#### [Примеры кода](#Примеры-кода-1)
#### [Ошибки и подводные камни](#Ошибки-и-подводные-камни-1)
#### [Сравнение с альтернативами](#Сравнение-с-альтернативами-1)
#### [Best practices и антипаттерны](#best-practices-и-антипаттерны-1)

---
# Ключевые слова try, catch, finally

## Введение
Обработка исключений через `try/catch/finally` — базовый механизм для управления ошибками в Kotlin, критичный для стабильности сервисов (например, при работе с внешними API или файловыми системами). Без корректной обработки падающих операций система теряет отказоустойчивость.

---

## Основные концепции
**Определения:**
- **`try`**: Блок для оборачивания потенциально опасного кода (например, HTTP-запросы к платежному шлюзу).
- **`catch`**: Перехват и обработка конкретных исключений (например, `IOException` при работе с S3-хранилищем).
- **`finally`**: Гарантированное выполнение кода для освобождения ресурсов (закрытие соединения с PostgreSQL даже при ошибке).

**Пример из реального кейса:**  
В микросервисе обработки транзакций `finally` используется для записи логов аудита в Kafka, даже если операция откатилась из-за `SQLException`.

---

## Примеры кода
### Сценарий 1: Работа с файлами (с гарантированным закрытием)
```kotlin
fun readConfigFile(path: String): String {
    val reader = FileReader(path)
    try {
        return reader.readText() // Может выбросить IOException
    } catch (e: IOException) {
        logger.error("Config read failed for $path", e)
        throw ApplicationConfigException("Invalid config")
    } finally {
        reader.close() // Всегда выполняется
    }
}
```

### Сценарий 2: Каскадные исключения в REST-контроллере
```kotlin
fun fetchUserData(userId: String): UserData {
    try {
        return userService.getUser(userId) // Может выбросить HttpClientErrorException
    } catch (e: HttpClientErrorException.NotFound) {
        throw UserNotFoundException("User $userId not found")
    } catch (e: HttpClientErrorException) {
        metrics.increment("api_errors")
        throw ApiIntegrationException("User service failed", e)
    } finally {
        // Очистка TLS-контекста или ThreadLocal
        SecurityContextHolder.clearContext()
    }
}
```

---

## Ошибки и подводные камни
1. **Порядок `catch`-блоков:**  
   Более специфичные исключения должны быть выше. Например, `FileNotFoundException` должен обрабатываться до общего `IOException`.

2. **Ресурсы в `finally`:**  
   Для автоматического закрытия используйте `.use{}` (реализует `AutoCloseable`):
   ```kotlin
   FileInputStream("data.bin").use { stream -> 
       // Работа с потоком
   } // Автоматически закрывается
   ```

3. **Игнорирование исключений:**  
   Пустой `catch`-блок маскирует ошибки:
   ```kotlin
   try { ... } catch (e: Exception) {} // Антипаттерн!
   ```

---

## Сравнение с альтернативами

| Подход               | Плюсы                          | Минусы                                   |
|----------------------|--------------------------------|------------------------------------------|
| `try/catch/finally`  | Полный контроль над потоком    | Много шаблонного кода                    |
| `Result`-обёртки     | Чистый FP-стиль                | Не работает с Java-библиотеками          |
| Глобальные обработчики | Централизованная логика        | Не подходит для точечного восстановления |

---

## Best practices и антипаттерны

### Best practices
1. **`finally` для критических операций**  
   Всегда закрывайте сетевые сокеты или JDBC-соединения:
   ```kotlin
   val socket = Socket()
   try { ... } finally {
       socket.close() // Даже при NetworkError
   }
   ```

2. **Специфичные исключения первыми**  
   Обрабатывайте узкие кейсы до общих:
   ```kotlin
   catch (e: PaymentDeclinedException) { ... }
   catch (e: PaymentException) { ... }
   ```

3. **Логгирование с контекстом**  
   Добавляйте идентификаторы для трассировки:
   ```kotlin
   catch (e: DatabaseException) {
       logger.error("DB fail for order ${orderId}", e)
   }
   ```

### Антипаттерны
1. **Тихий проглатывание ошибок**
   ```kotlin
   try { sendMetrics() } catch (e: Exception) {} // Метрики не отправляются, но никто не знает
   ```

2. **`catch (Throwable)` без реагирования**  
   Перехват `Error` (например, `OutOfMemoryError`) мешает корректному крашу приложения:
   ```kotlin
   try { ... } catch (e: Throwable) { /* JVM не может восстановиться */ }
   ```