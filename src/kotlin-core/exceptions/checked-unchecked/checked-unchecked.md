#### [Введение](#Введение-1)
#### [Основные концепции](#Основные-концепции-1)
#### [Ошибки и подводные камни](#Ошибки-и-подводные-камни-1)
#### [Сравнение с альтернативами](#Сравнение-с-альтернативами-1)
#### [Best practices и антипаттерны](#best-practices-и-антипаттерны-1)

---
# Проверяемые и непроверяемые исключения

## Введение
Исключения в Kotlin, как и в других языках программирования, используются для обработки ошибок и неожиданных ситуаций в коде. Понимание разницы между проверяемыми и непроверяемыми исключениями важно для написания надежного и поддерживаемого кода, особенно в системах с высокой нагрузкой, таких как микросервисы или распределенные системы.

## Основные концепции

### Проверяемые исключения
Проверяемые исключения (checked exceptions) — это исключения, которые компилятор требует явно обработать или пробросить дальше. Они обычно используются для ошибок, которые можно предвидеть и обработать, например, проблемы с вводом-выводом (IO) или сетевыми запросами.

Пример из реального кейса:
- В микросервисной архитектуре, при работе с внешними API, проверяемые исключения могут использоваться для обработки ошибок соединения или некорректных ответов от сервера.

### Непроверяемые исключения
Непроверяемые исключения (unchecked exceptions) — это исключения, которые не требуют явной обработки компилятором. Они обычно возникают из-за ошибок в логике программы, таких как `NullPointerException`, `IllegalArgumentException` и т.д.

Пример из реального кейса:
- В системе обработки платежей непроверяемые исключения могут возникать при попытке выполнить операцию с некорректными данными, например, при передаче отрицательной суммы платежа.

## Примеры кода на Kotlin

### Проверяемые исключения
```kotlin
fun readFile(filePath: String): String {
    return try {
        File(filePath).readText()
    } catch (e: IOException) {
        // Логируем ошибку и возвращаем пустую строку
        println("Ошибка при чтении файла: ${e.message}")
        ""
    }
}
```
В этом примере `IOException` — это проверяемое исключение, которое мы обрабатываем в блоке `catch`.

### Непроверяемые исключения
```kotlin
fun divide(a: Int, b: Int): Int {
    if (b == 0) {
        throw IllegalArgumentException("Делитель не может быть равен нулю")
    }
    return a / b
}
```
Здесь `IllegalArgumentException` — это непроверяемое исключение, которое выбрасывается при попытке деления на ноль.

## Ошибки и подводные камни

### Проверяемые исключения
- **Избыточность кода**: Часто приводит к большому количеству boilerplate кода, так как каждое проверяемое исключение нужно обрабатывать.
- **Утечка абстракций**: Проверяемые исключения могут раскрывать детали реализации, что нарушает принцип инкапсуляции.

### Непроверяемые исключения
- **Сложность отладки**: Непроверяемые исключения могут быть пропущены на этапе разработки, что приводит к ошибкам в runtime.
- **Неожиданное завершение программы**: Если непроверяемое исключение не обработано, программа может завершиться аварийно.

## Сравнение с альтернативами

| Характеристика          | Проверяемые исключения          | Непроверяемые исключения          |
|-------------------------|----------------------------------|------------------------------------|
| **Обязательность обработки** | Да                              | Нет                                |
| **Использование**        | Для предсказуемых ошибок         | Для ошибок в логике программы      |
| **Boilerplate код**      | Много                           | Минимум                            |
| **Риск утечки абстракций** | Высокий                         | Низкий                             |

## Best practices и антипаттерны

### Best practices
1. **Используйте проверяемые исключения для предсказуемых ошибок**:
    - Например, при работе с файловой системой или сетевыми запросами.
   ```kotlin
   fun loadConfig(filePath: String): Config {
       return try {
           ConfigLoader.load(filePath)
       } catch (e: IOException) {
           throw ConfigLoadException("Не удалось загрузить конфигурацию", e)
       }
   }
   ```

2. **Логируйте непроверяемые исключения**:
    - Это поможет быстрее находить и исправлять ошибки в production.
   ```kotlin
   fun processPayment(amount: Double) {
       try {
           PaymentProcessor.process(amount)
       } catch (e: IllegalArgumentException) {
           logger.error("Ошибка при обработке платежа: ${e.message}")
           throw e
       }
   }
   ```

3. **Создавайте пользовательские исключения для бизнес-логики**:
    - Это улучшает читаемость кода и упрощает отладку.
   ```kotlin
   class InsufficientFundsException(message: String) : RuntimeException(message)

   fun withdraw(amount: Double) {
       if (amount > balance) {
           throw InsufficientFundsException("Недостаточно средств на счете")
       }
       balance -= amount
   }
   ```

### Антипаттерны
1. **Игнорирование проверяемых исключений**:
    - Это может привести к неожиданным ошибкам и сложностям в отладке.
   ```kotlin
   fun readFileUnsafe(filePath: String): String {
       return File(filePath).readText() // IOException может быть проигнорирован
   }
   ```

2. **Злоупотребление непроверяемыми исключениями**:
    - Использование непроверяемых исключений для всех типов ошибок может привести к нечитаемому коду и сложностям в поддержке.
   ```kotlin
   fun validateUser(user: User) {
       if (user.name.isBlank()) {
           throw RuntimeException("Имя пользователя не может быть пустым") // Не информативно
       }
   }
   ```
