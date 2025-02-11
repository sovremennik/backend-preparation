#### [Введение](#Введение-1)
#### [Основные концепции](#Основные-концепции-1)
#### [Примеры кода](#Примеры-кода-1)
#### [Ошибки и решения](#Ошибки-и-решения-1)
#### [Best Practices](#best-practices-1)

---

## Введение
Стандартный Java-плагин в Gradle — это встроенный механизм, который позволяет управлять компиляцией, тестированием и упаковкой Java-кода в проекты. Он используется практически во всех Java-проектах, предоставляя базовые задачи для работы с кодом. Этот плагин необходим для автоматизации сборочного процесса и интеграции с инструментами экосистемы Java.

## Основные концепции
- **Что делает стандартный Java-плагин?**
   - Добавляет поддержку компиляции Java-кода (`compileJava`).
   - Автоматически создает задачи для тестирования (`test`) и упаковки (`jar`).
   - Управляет зависимостями через `implementation`, `testImplementation` и другие конфигурации.
   - Генерирует каталог `build/`, содержащий артефакты сборки.

- **Ключевые задачи, добавляемые плагином:**
   - `compileJava` — компиляция исходного кода.
   - `processResources` — обработка ресурсов.
   - `classes` — сборка скомпилированных классов.
   - `test` — выполнение юнит-тестов.
   - `jar` — создание JAR-файла.
   - `assemble` — сборка проекта.
   - `check` — выполнение тестов и проверок.
   - `build` — полная сборка проекта.

## Примеры кода
### 1. Подключение Java-плагина в `build.gradle`
```groovy
plugins {
    id 'java'
}
```

### 2. Минимальная конфигурация проекта
```groovy
plugins {
    id 'java'
}

group = 'com.example'
version = '1.0.0'

repositories {
    mavenCentral()
}

dependencies {
    implementation 'org.apache.commons:commons-lang3:3.12.0'
    testImplementation 'junit:junit:4.13.2'
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
```groovy
jar {
    manifest {
        attributes 'Main-Class': 'com.example.Main'
    }
}
```

## Ошибки и решения
| Ошибка | Причина | Решение |
|--------|--------|---------|
| `Could not find or load main class` | Не указан `Main-Class` в `manifest` | Добавьте `manifest` в `jar`-задачу |
| `No tests found for given includes` | Gradle не нашел тесты | Убедитесь, что тестовые классы находятся в `src/test/java/` и аннотированы `@Test` |
| `Could not resolve dependency` | Ошибка при загрузке зависимостей | Проверьте репозитории в `repositories {}` |

## Best Practices
1. **Определяйте версии зависимостей явно** — фиксированные версии предотвращают неожиданные обновления.
2. **Используйте `implementation` вместо `compile`** — `compile` устарел с Gradle 5+.
3. **Добавляйте `mainClass` в JAR** — это позволит запускать JAR напрямую через `java -jar`.
4. **Оптимизируйте кеширование** — используйте `Gradle Build Cache` для ускорения повторных сборок.
5. **Следите за версией Java** — используйте `toolchain` для совместимости с разными версиями JDK.

