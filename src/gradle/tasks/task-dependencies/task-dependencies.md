#### [Введение](#Введение-1)
#### [Основные концепции](#Основные-концепции-1)
#### [Примеры конфигурации](#Примеры-конфигурации-1)
#### [Ошибки и решения](#Ошибки-и-решения-1)
#### [Best Practices](#best-practices-1)

---

## Введение
Управление зависимостями в Gradle — это фундаментальный навык для backend-разработчиков, работающих с высоконагруженными системами. Некорректные зависимости могут привести к конфликтам версий, увеличению времени сборки и даже падению сервисов под нагрузкой. Этот конспект поможет вам разобраться в ключевых механизмах Gradle, чтобы избежать критических ошибок и оптимизировать процесс разработки.

---

## Основные концепции  
### Практическое определение
Управление зависимостями включает:
1. **Описание**: Указание библиотек и их версий.
2. **Разрешение**: Автоматическая загрузка из репозиториев.
3. **Использование**: Подключение зависимостей к проекту.

### Разъяснение терминов
- **Configuration** — группа зависимостей с определенной целью (например, `implementation` для компиляции, `testImplementation` для тестов).
- **ResolutionStrategy** — правила, которыми Gradle руководствуется при разрешении конфликтов версий.
- **Транзитивная зависимость** — библиотека, которая автоматически подтягивается как часть другой зависимости.

---

## Примеры конфигурации  
### Базовое описание зависимостей
```kotlin
dependencies {
    implementation("org.springframework.boot:spring-boot-starter-web:3.1.0") // Основная зависимость
    testImplementation("org.junit.jupiter:junit-jupiter:5.9.0") // Только для тестов
}
```

### Использование ResolutionStrategy
```kotlin
configurations.all {
    resolutionStrategy {
        // Принудительно использовать версию 1.7.36 для всех SLF4J-зависимостей
        force("org.slf4j:slf4j-api:1.7.36")
        
        // Прервать сборку при конфликте версий
        failOnVersionConflict()
    }
}
```

### Анализ зависимостей
Чтобы вывести дерево зависимостей, выполните:
```bash
./gradlew dependencies --configuration runtimeClasspath
```
Пример вывода:
```
+--- org.springframework.boot:spring-boot-starter-web:3.1.0
|    +--- org.springframework.boot:spring-boot-starter:3.1.0
|    |    +--- org.slf4j:slf4j-api:1.7.36 (forced)
...
```

---

## Ошибки и решения
### Конфликты версий
**Проблема**:  
Библиотеки `A` и `B` требуют разные версии `slf4j-api:1.7.30` и `slf4j-api:2.0.0`.

**Решение 1 — принудительная версия**:
```kotlin
configurations.all {
    resolutionStrategy.force("org.slf4j:slf4j-api:1.7.36")
}
```

**Решение 2 — исключение транзитивной зависимости**:
```kotlin
dependencies {
    implementation("com.example:libA:1.0") {
        exclude(group = "org.slf4j", module = "slf4j-api")
    }
}
```

### Неявные транзитивные зависимости
**Проблема**:  
Библиотека `spring-boot-starter-web` подтягивает `tomcat-embed-core`, который конфликтует с Jetty.

**Решение**:
```kotlin
dependencies {
    implementation("org.springframework.boot:spring-boot-starter-web:3.1.0") {
        exclude(group = "org.apache.tomcat.embed", module = "tomcat-embed-core")
    }
    implementation("org.eclipse.jetty:jetty-server:11.0.15")
}
```

---

## Best Practices
1. **Строгая версионная политика**  
   Всегда указывайте версии явно:
   ```kotlin
   val springVersion = "3.1.0"
   dependencies {
       implementation("org.springframework.boot:spring-boot-starter-web:$springVersion")
   }
   ```

2. **Анализ зависимостей в CI/CD**  
   Добавьте проверку в GitLab CI:
   ```yaml
   check_dependencies:
     script:
       - ./gradlew dependencies > dependencies.txt
       - grep "FAILED" dependencies.txt && exit 1 || exit 0
   ```

3. **Использование версионных диапазонов (осторожно!)**  
   Пример динамической версии:
   ```kotlin
   dependencies {
       // Берет последнюю версию 3.1.x
       implementation("org.springframework.boot:spring-boot-starter-web:3.1.+") 
   }
   ```

4. **Конфигурация для мониторинга**  
   Отслеживайте размер зависимостей в Prometheus:
   ```kotlin
   tasks.register("dependencyMetrics") {
     doLast {
       val size = configurations.runtimeClasspath.get().files.sumOf { it.length() }
       println("Dependencies size: ${size / 1024} KB")
     }
   }
   ```

5. **Кастомные конфигурации**  
   Создайте группу для специфичных зависимостей:
   ```kotlin
   configurations {
       create("metrics") {
           extendsFrom(implementation.get())
       }
   }

   dependencies {
       "metrics"("io.micrometer:micrometer-core:1.11.5")
   }
   ```
