#### [Введение](#Введение-1)
#### [Основные концепции](#Основные-концепции-1)
#### [Примеры кода](#Примеры-кода-1)
#### [Ошибки и решения](#Ошибки-и-решения-1)
#### [Best Practices](#best-practices-1)

---

## Введение
Стандартный Kotlin-плагин в Gradle предоставляет удобный способ компиляции, тестирования и упаковки Kotlin-кода. Он упрощает настройку проекта, добавляет интеграцию с JVM и позволяет использовать современные возможности Kotlin в сочетании с инструментами Gradle.

## Основные концепции
- **Что делает стандартный Kotlin-плагин?**
   - Добавляет поддержку компиляции Kotlin-кода (`compileKotlin`).
   - Автоматически создает задачи для тестирования (`test`) и упаковки (`jar`).
   - Управляет зависимостями через `implementation`, `testImplementation` и другие конфигурации.
   - Поддерживает интеграцию с Java-проектами и смешанный код.

- **Ключевые задачи, добавляемые плагином:**
   - `compileKotlin` — компиляция исходного кода на Kotlin.
   - `compileTestKotlin` — компиляция тестов на Kotlin.
   - `test` — выполнение юнит-тестов.
   - `jar` — создание JAR-файла.
   - `assemble` — сборка проекта.
   - `check` — выполнение тестов и проверок.
   - `build` — полная сборка проекта.

## Примеры кода
### 1. Подключение Kotlin-плагина в `build.gradle.kts`
```kotlin
plugins {
    kotlin("jvm") version "1.9.0"
}
```

### 2. Минимальная конфигурация проекта
```kotlin
plugins {
    kotlin("jvm") version "1.9.0"
}

group = "com.example"
version = "1.0.0"

repositories {
    mavenCentral()
}

dependencies {
    implementation(kotlin("stdlib"))
    testImplementation("org.jetbrains.kotlin:kotlin-test")
}
```

### 3. Запуск ключевых задач
```sh
gradle build      # Полная сборка проекта
gradle test       # Запуск тестов
gradle jar        # Создание JAR-файла
gradle clean      # Очистка каталога build/
```

### 4. Настройка JAR-архива
```kotlin
jar {
    manifest {
        attributes["Main-Class"] = "com.example.MainKt"
    }
}
```

## Ошибки и решения
| Ошибка | Причина | Решение |
|--------|--------|---------|
| `Main class not found` | Kotlin-файл не скомпилирован корректно | Убедитесь, что `MainKt` указан как `Main-Class` |
| `Could not resolve dependency` | Ошибка при загрузке зависимостей | Проверьте репозитории в `repositories {}` |
| `Unresolved reference` | Gradle не нашел стандартную библиотеку Kotlin | Добавьте `kotlin("stdlib")` в `dependencies` |

## Best Practices
1. **Используйте `kotlin("jvm")` вместо `id("org.jetbrains.kotlin.jvm")`** — это более удобный способ подключения плагина.
2. **Определяйте версии Kotlin явно** — фиксированные версии предотвращают неожиданные обновления.
3. **Добавляйте `mainClass` в JAR** — это позволит запускать JAR напрямую через `java -jar`.
4. **Оптимизируйте кеширование** — используйте `Gradle Build Cache` для ускорения повторных сборок.
5. **Используйте `kotlin-dsl` для Gradle** — он предоставляет лучшую поддержку автодополнения в IDE.

