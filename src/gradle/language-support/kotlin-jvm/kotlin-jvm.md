#### [Введение](#введение-1)
#### [Основные концепции](#основные-концепции-1)
#### [Архитектура взаимодействия](#архитектура-взаимодействия-1)
#### [Примеры конфигурации](#примеры-конфигурации-1)
#### [Ошибки и решения](#ошибки-и-решения-1)
#### [Best Practices](#best-practices-1)

---

## Введение
Совместимость Gradle с JVM — критический аспект для высоконагруженных систем, где неверные настройки могут привести к падению производительности на 50% и более. Например, в инфраструктуре Alibaba Cloud переход на Java 21 с оптимизированными настройками Gradle сократил время обработки запросов на 30% (данные из доклада QCon 2025).

---

## Основные концепции
### 1. Что такое совместимость Gradle-JVM?
Это согласованность между:
- Версией Java, используемой Gradle для сборки
- Версией Java, на которой запускается приложение
- Настройками JVM (память, сборщик мусора)

**Решаемые проблемы:**
- Ошибки `UnsupportedClassVersionError` в production
- Утечки памяти при сборке больших проектов
- Неэффективное использование ресурсов CI/CD

### 2. Контекст из индустрии
- **Netflix:** Миграция на Java 21 + Gradle 8.6 уменьшила потребление памяти в CI на 40% (TechBlog, 2025).
- **Spotify:** Использование Toolchain API позволило одновременно собирать проекты для Java 17 и 21 (Devoxx case study).

### 3. Ключевые термины
- **Toolchain API** — механизм Gradle для управления версиями JDK (как "переключатель" между Java-версиями).
- **JVM Options** — параметры запуска JVM, влияющие на производительность (`-Xmx`, `-XX:+UseZGC`).
- **Bytecode Level** — версия байт-кода, которую генерирует компилятор (например, `targetCompatibility=JavaVersion.VERSION_21`).

---

## Архитектура взаимодействия
### 1. Градуальная компиляция
```
  ┌───────────────────────┐  
  │ CI Server (JDK 21)    │  
  └───────────┬───────────┘  
              │  
              ▼  
  ┌───────────────────────┐  
  │ Gradle Daemon         │  ← Использует JDK 21 для сборки  
  └───────────┬───────────┘  
              │  
              ▼  
  ┌───────────────────────┐  
  │ Компиляция → bytecode │  ← Генерирует байт-код для Java 21 (v65)  
  └───────────┬───────────┘  
              │  
              ▼  
  ┌───────────────────────┐  
  │ JAR с метаданными     │  ← Содержит MANIFEST.MF с версией Java  
  └───────────┬───────────┘  
              │  
              ▼  
  ┌───────────────────────┐  
  │ Production (JVM 17)   │  ← Попытка запуска → ОШИБКА!  
  └───────────────────────┘  
```  
**Пояснение:**
- **bytecode v65** — версия байт-кода Java 21.
- **JVM 17** — поддерживает только байт-код до v61.
- **Ошибка:** `UnsupportedClassVersionError` — несовместимость версий.

### 2. Архитектура
```
  ┌───────────────────────┐  
  │ CI Server (JDK 21)    │  
  └───────────┬───────────┘  
              │  
              ▼  
  ┌───────────────────────┐  
  │ Gradle Daemon         │  
  └───────────┬───────────┘  
              │  
              ▼  
  ┌───────────────────────┐  
  │ Компиляция → bytecode │  ← targetCompatibility=17 (v61)  
  └───────────┬───────────┘  
              │  
              ▼  
  ┌───────────────────────┐  
  │ JAR с метаданными     │  
  └───────────┬───────────┘  
              │  
              ▼  
  ┌───────────────────────┐  
  │ Production (JVM 17)   │  ← Успешный запуск!  
  └───────────────────────┘  
```  

---

## Примеры конфигурации
### 1. Безопасная миграция с Java 17 на 21
```kotlin (build.gradle.kts)  
java {  
    toolchain {  
        languageVersion = JavaLanguageVersion.of(21)  // JDK для сборки  
        vendor = JvmVendorSpec.ADOPTIUM  // Используем стабильный JDK  
    }  
    sourceCompatibility = JavaVersion.VERSION_17  // Исходный код совместим с Java 17  
    targetCompatibility = JavaVersion.VERSION_17   // Байт-код для Java 17  
}  

tasks.withType<JavaExec> {  
    jvmArgs = listOf(  
        "-XX:+UseZGC",           // Используем Z Garbage Collector  
        "-Xmx4g",                // Максимум 4 GB памяти  
        "-Dfile.encoding=UTF-8"  // Кодировка файлов  
    )  
}  
```  
**Пояснение:**
- `languageVersion` гарантирует, что Gradle использует JDK 21 для сборки.
- `sourceCompatibility` проверяет, что исходный код соответствует Java 17.
- `targetCompatibility` генерирует байт-код, совместимый с Java 17.

### 2. Настройка для микросервисов в Kubernetes
```yaml (helm/values.yaml)  
jvm:  
  version: 21  
  args:  
    - "-XX:+UseContainerSupport"  # Оптимизация памяти под контейнеры  
    - "-XX:MaxRAMPercentage=75.0" # Не более 75% памяти контейнера  
    - "-XX:ActiveProcessorCount=4" # Ограничение CPU  
```  
**Как это работает:**
- `UseContainerSupport` позволяет JVM учитывать ограничения памяти в контейнере.
- `MaxRAMPercentage=75.0` резервирует 25% памяти для системных процессов.

### 3. Multi-JDK конфигурация
```kotlin  
plugins {  
    id("java")  
    id("org.gradle.toolchains.foojay-resolver-convention") version "0.7.0"  # Плагин для автоматической загрузки JDK  
}  

tasks.register("buildForLegacy") {  
    javaToolchains {  
        compilerFor(JAVA_17)  # Используем JDK 17 для этой задачи  
    }  
    dependsOn(tasks.named("jar"))  
}  
```  
**Сценарий:**  
Сборка артефактов для legacy-систем, требующих Java 17, при основном JDK 21.

---

## Ошибки и решения
### 1. Ошибка: `UnsupportedClassVersionError`
**Симптом:**
```  
java.lang.UnsupportedClassVersionError: Unsupported major.minor version 65.0  
```  
**Причина:** Сборка с JDK 21, запуск на JDK 17.  
**Решение:**
```kotlin  
java {  
    targetCompatibility = JavaVersion.VERSION_17  // Явное указание версии байт-кода  
}  
```  
**Профилактика:**
- Добавить проверку версии в CI:
  ```bash  
  ./gradlew checkJavaVersion -PexpectedVersion=17  
  ```  

### 2. Проблема: Утечки памяти в Gradle Daemon
**Симптом:**  
Потребление памяти >8GB при сборке 100+ модулей.  
**Исправление:**
```properties  
# gradle.properties  
org.gradle.jvmargs=-Xmx4g -XX:+UseZGC -XX:MaxMetaspaceSize=1g  
```  
**Разбор параметров:**
- `-Xmx4g` — лимит оперативной памяти.
- `-XX:+UseZGC` — сборщик мусора с паузами <1ms.
- `-XX:MaxMetaspaceSize=1g` — ограничение памяти для метаданных классов.

### 3. Сканирование уязвимостей (OWASP Dependency-Check)
**Проблема:** Уязвимости в зависимостях (CVE).  
**Решение:**
```kotlin  
plugins {  
    id("org.owasp.dependencycheck") version "8.4.0"  
}  

dependencyCheck {  
    formats = listOf("HTML", "JSON")  # Форматы отчетов  
    failBuildOnCVSS = 7.0             # Прерывать сборку при критических уязвимостях  
    suppressionFile = "suppressions.xml"  # Игнорируемые CVE  
}  
```  
**Шаги:**
1. Создать файл `suppressions.xml` для исключения ложных срабатываний:
   ```xml  
   <?xml version="1.0" encoding="UTF-8"?>  
   <suppressions>  
       <suppress>  
           <cve>CVE-2023-1234</cve> <!-- Ложное срабатывание для log4j -->  
       </suppress>  
   </suppressions>  
   ```  
2. Запустить сканирование:
   ```bash  
   ./gradlew dependencyCheckAnalyze  
   ```  
3. Отчеты будут в `build/reports/dependency-check/`.

**Пример отчета:**
```  
[INFO] Found 1 vulnerabilities in project 'my-app'  
[CRITICAL] CVE-2023-1234: log4j-core 2.14.1  
```  

---

## Best Practices
### 1. Правило "Явного указания версий"
```kotlin  
java {  
    toolchain {  
        languageVersion = JavaLanguageVersion.of(21)  
        vendor = JvmVendorSpec.ADOPTIUM  
    }  
    sourceCompatibility = JavaVersion.VERSION_17  
    targetCompatibility = JavaVersion.VERSION_17  
}  
```  
**Зачем:**
- Избегать "дрейфа" версий между средами (dev, staging, prod).

### 2. Мониторинг через Micrometer + Prometheus
```kotlin  
dependencies {  
    implementation("io.micrometer:micrometer-registry-prometheus:1.12.0")  
}  

tasks.withType<JavaExec> {  
    jvmArgs = listOf(  
        "-javaagent:micrometer-agent.jar",  
        "-Dmanagement.endpoints.web.exposure.include=metrics"  
    )  
}  
```  
**Дашборд Grafana:**
- **Метрики:**
    - `jvm_memory_used_bytes{area="heap"}` — использование кучи.
    - `jvm_gc_pause_seconds_max` — максимальное время пауз GC.

### 3. Интеграция в GitLab CI
```yaml  
stages:  
  - security  
  - build  

security_scan:  
  stage: security  
  image: gradle:8.6-jdk21  
  script:  
    - ./gradlew dependencyCheckAnalyze  
  artifacts:  
    paths:  
      - build/reports/dependency-check/  

build_job:  
  stage: build  
  image: gradle:8.6-jdk21  
  script:  
    - ./gradlew build -PjvmVersion=21  
  variables:  
    GRADLE_JVM_OPTS: "-Xmx4g -XX:+UseZGC"  
```  
**Логика:**
- Отдельный этап для проверки уязвимостей.
- Кэширование отчетов для анализа.

### 4. Оптимизация сборки для больших проектов
```kotlin  
tasks.withType<JavaCompile> {  
    options.compilerArgs.addAll(  
        "-Xmaxerrs", "500",     // Лимит ошибок  
        "-Xlint:all",           // Все предупреждения  
        "-Werror"               // Превратить предупреждения в ошибки  
    )  
    options.isIncremental = true  
    options.isFork = true  
}  
```
