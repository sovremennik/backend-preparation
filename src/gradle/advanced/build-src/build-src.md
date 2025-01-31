#### [Введение](#Введение-1)
#### [Основные концепции](#Основные-концепции-1)
#### [Примеры кода](#Примеры-кода-1)
#### [Ошибки и решения](#Ошибки-и-решения-1)
#### [Best Practices](#best-practices-1)

---

## Введение
Переиспользование кода в Gradle — важный аспект поддерживаемости и масштабируемости сборочного процесса. Оно позволяет минимизировать дублирование, улучшить читаемость и упростить управление зависимостями в крупных проектах. В сложных проектах, содержащих множество модулей, правильное структурирование кода сборки помогает избежать ошибок и улучшает его переиспользуемость.

## Основные концепции
- **Почему важно переиспользование кода?**
    - Уменьшает дублирование.
    - Повышает читаемость и упрощает поддержку.
    - Делает код сборки более декларативным.
    - Уменьшает вероятность расхождения настроек между модулями.
    - Позволяет централизованно управлять зависимостями и плагинами.
- **Подходы к переиспользованию в Gradle:**
    - **Использование `buildSrc`** — позволяет хранить общие скрипты и логику в отдельном модуле внутри проекта.
    - **Создание библиотекных модулей** — вынесение повторно используемых зависимостей и логики в отдельные модули.
    - **Конфигурационные плагины** — удобный способ инкапсуляции конфигурации для различных частей проекта.
    - **Использование `version catalog` (Gradle 7.0+)** — управление зависимостями в одном месте с `libs.versions.toml`.
    - **Общий `settings.gradle.kts`** — возможность централизованного управления настройками проекта.

## Примеры кода
### 1. Вынесение общих задач в `buildSrc`
```kotlin
// buildSrc/src/main/kotlin/CustomTask.kt
import org.gradle.api.DefaultTask
import org.gradle.api.tasks.TaskAction

abstract class CustomTask : DefaultTask() {
    @TaskAction
    fun execute() {
        println("Executing shared task logic")
    }
}
```
Использование в `build.gradle.kts`:
```kotlin
tasks.register<CustomTask>("runCustomTask")
```

### 2. Создание конфигурационных объектов
```kotlin
open class BuildConfigExtension {
    var debugMode: Boolean = false
}
```
```kotlin
class ConfigPlugin : Plugin<Project> {
    override fun apply(project: Project) {
        val extension = project.extensions.create("buildConfig", BuildConfigExtension::class.java)
        project.tasks.register("printConfig") {
            doLast {
                println("Debug mode: ${extension.debugMode}")
            }
        }
    }
}
```
Применение плагина в `build.gradle.kts`:
```kotlin
plugins {
    id("com.example.config-plugin")
}

buildConfig {
    debugMode = true
}
```

### 3. Использование Version Catalog для управления зависимостями
Файл `gradle/libs.versions.toml`:
```toml
[versions]
kotlin = "1.9.0"
retrofit = "2.9.0"

[libraries]
kotlin-stdlib = { module = "org.jetbrains.kotlin:kotlin-stdlib", version.ref = "kotlin" }
retrofit = { module = "com.squareup.retrofit2:retrofit", version.ref = "retrofit" }
```
Использование в `build.gradle.kts`:
```kotlin
dependencies {
    implementation(libs.kotlin.stdlib)
    implementation(libs.retrofit)
}
```

## Ошибки и решения
| Ошибка | Причина | Решение |
|--------|--------|---------|
| `Task already exists` | Попытка зарегистрировать дублирующееся имя задачи | Используйте `tasks.findByName` перед регистрацией |
| `Plugin not found` | Плагин не опубликован или не указан в `settings.gradle.kts` | Проверьте путь к плагину и используйте `includeBuild` |
| `Dependency resolution failed` | Несовместимые версии зависимостей | Используйте `version catalog` для централизованного управления версиями |
| `Configuration on demand is not supported` | Использование конфигурации "on demand" для многомодульных проектов | Отключите `org.gradle.configureondemand` в `gradle.properties` |

## Best Practices
1. **Используйте `buildSrc` для локальных утилит** — это упрощает поддержку и не требует публикации.
2. **Минимизируйте зависимости** — чем меньше зависимостей у плагина, тем стабильнее сборка.
3. **Разделяйте конфигурацию и логику** — используйте плагины вместо дублирования настроек в `build.gradle.kts`.
4. **Используйте `version catalog`** — централизованное управление зависимостями снижает риск версионных конфликтов.
5. **Оптимизируйте кэширование сборки** — правильно используйте `inputs` и `outputs` в задачах, чтобы минимизировать повторные сборки.
6. **Используйте включаемые билды (`includeBuild`)** — если необходимо переиспользовать код между проектами, но нет возможности опубликовать артефакты.

