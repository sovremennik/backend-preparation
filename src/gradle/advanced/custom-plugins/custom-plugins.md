#### [Введение](#Введение-1)
#### [Основные концепции](#Основные-концепции-1)
#### [Примеры конфигурации](#Примеры-конфигурации-1)
#### [Ошибки и решения](#Ошибки-и-решения-1)
#### [Best Practices](#best-practices-1)

---

## Введение
Создание плагинов в Gradle — ключевой инструмент для автоматизации и стандартизации сборочного процесса. В крупных проектах собственные плагины позволяют уменьшить дублирование кода, централизовать настройку зависимостей и упрощать CI/CD.

## Основные концепции
- **Что такое Gradle-плагин:** это модуль, инкапсулирующий логику настройки сборки и переиспользуемый в нескольких проектах.
- **Зачем использовать:**
    - Централизованное управление зависимостями.
    - Расширяемость сборочного процесса.
    - Повторное использование общих задач и конфигураций.
- **Типы плагинов:**
    - **Скриптовые (`buildSrc`)** — простейший способ инкапсуляции логики.
    - **Стандартные (`precompiled script plugins`)** — создаются через `*.gradle.kts` в `buildSrc`.
    - **Бинарные (Standalone Plugins)** — полноценные плагины, публикуемые в локальные/глобальные репозитории.

## Примеры конфигурации
### 1. Создание простого плагина в `buildSrc`
```kotlin
// buildSrc/src/main/kotlin/MyCustomPlugin.kt
import org.gradle.api.Plugin
import org.gradle.api.Project

class MyCustomPlugin : Plugin<Project> {
    override fun apply(project: Project) {
        project.tasks.register("hello") {
            doLast {
                println("Hello from custom Gradle plugin!")
            }
        }
    }
}
```

Использование плагина в `build.gradle.kts`:
```kotlin
plugins {
    id("my.custom.plugin")
}
```

### 2. Создание плагина в отдельном модуле
1. Добавьте новый модуль `my-plugin`:
```shell
gradle init --type java-library --project-name my-plugin
```
2. Определите плагин:
```kotlin
// my-plugin/src/main/kotlin/MyPlugin.kt
import org.gradle.api.Plugin
import org.gradle.api.Project

class MyPlugin : Plugin<Project> {
    override fun apply(project: Project) {
        project.tasks.register("customTask") {
            doLast {
                println("Executing custom Gradle task")
            }
        }
    }
}
```
3. Опубликуйте в локальный репозиторий:
```shell
gradle publishToMavenLocal
```
4. Используйте в другом проекте:
```kotlin
plugins {
    id("com.example.myplugin") version "1.0.0"
}
```

## Ошибки и решения
| Ошибка | Причина | Решение |
|--------|--------|---------|
| `Plugin with id 'my.custom.plugin' not found` | Плагин не зарегистрирован или не опубликован | Убедитесь, что `buildSrc` корректно скомпилирован, либо плагин опубликован в репозиторий |
| `NoClassDefFoundError` | Ошибка в зависимостях плагина | Проверьте, что все зависимости указаны в `implementation` |
| `Task already exists` | Конфликт имен задач | Используйте уникальные имена или проверяйте наличие задачи перед созданием |

## Best Practices
1. **Используйте `buildSrc` для локальных плагинов** – это упрощает поддержку и не требует публикации.
2. **Пишите тесты для плагинов** – используйте `Gradle TestKit` для проверки корректности выполнения задач.
3. **Минимизируйте зависимости** – чем меньше зависимостей у плагина, тем стабильнее сборка.
4. **Делайте плагины конфигурируемыми** – предоставляйте расширения (`extension`) для настройки плагина в `build.gradle.kts`.

