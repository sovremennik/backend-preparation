#### [Введение](#Введение-1)
#### [Основные концепции](#Основные-концепции-1)
#### [Примеры конфигурации](#Примеры-конфигурации-1)
#### [Ошибки и решения](#Ошибки-и-решения-1)
#### [Best Practices](#best-practices-1)

---

## Введение
Spring Boot Plugin для Gradle — ключевой инструмент для автоматизированной сборки и развертывания Spring Boot приложений. Без него сложно настроить генерацию исполняемых JAR-файлов, корректную обработку зависимостей и интеграцию с облачными сервисами.

## Основные концепции
- **Практическое определение:** Spring Boot Plugin расширяет возможности Gradle, упрощая упаковку, запуск и развертывание приложений Spring Boot.
- **Контекст из индустрии:** В Netflix OSS Spring Boot Plugin применяется для автоматизации сборки микросервисов, обеспечивая консистентность артефактов и упрощая CI/CD.
- **Разъяснение терминов:**
   - *Fat JAR* — исполняемый JAR, включающий все зависимости, необходимые для работы приложения.
   - *Spring Boot Layered JAR* — оптимизированный формат JAR, разделяющий код, зависимости и ресурсы для ускорения деплоя.

## Примеры конфигурации

### Подключение плагина в `build.gradle.kts`
```kotlin
plugins {
    id("org.springframework.boot") version "3.2.0"
    id("io.spring.dependency-management") version "1.1.4"
    kotlin("jvm") version "1.9.22"
    kotlin("plugin.spring") version "1.9.22"
}

dependencies {
    implementation("org.springframework.boot:spring-boot-starter-web")
    implementation("com.fasterxml.jackson.module:jackson-module-kotlin")
    testImplementation("org.springframework.boot:spring-boot-starter-test")
}
```

### Генерация исполняемого JAR
```shell
gradle bootJar
```

### Запуск приложения
```shell
gradle bootRun
```

### Оптимизированный Layered JAR для Docker
```kotlin
springBoot {
    layered()
}
```

## Ошибки и решения

| Ошибка | Причина | Решение |
|---|---|---|
| `ClassNotFoundException` при запуске | Отсутствие зависимостей в fat JAR | Убедитесь, что `bootJar` используется вместо `jar` |
| Долгая сборка в CI | Неиспользование кэша слоёв JAR | Включите `layered()` и разделяйте зависимости |
| Конфликт версий библиотек | Отсутствует `dependency-management` | Добавьте `io.spring.dependency-management` в плагины |

## Best Practices
1. **Явное указание версий плагинов**: Всегда фиксируйте версии плагинов и зависимостей для предсказуемости.
2. **Использование Layered JAR**: Ускоряет доставку и деплой приложений в облаке.
3. **Интеграция с Docker**: Используйте `bootBuildImage` для создания OCI-совместимых образов.
4. **Минимизация зависимости от `bootRun` в продакшене**: Запускайте через `bootJar` и `java -jar`.
5. **Мониторинг и логирование**: Настройте `logback.xml` и `Prometheus` для метрик.

