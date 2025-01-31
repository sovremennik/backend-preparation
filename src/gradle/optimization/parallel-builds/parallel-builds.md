#### [Введение](#Введение-1)
#### [Основные концепции](#Основные-концепции-1)
#### [Архитектура параллельной сборки](#Архитектура-параллельной-сборки-1)
#### [Примеры конфигурации](#Примеры-конфигурации-1)
#### [Ошибки и решения](#Ошибки-и-решения-1)
#### [Best Practices](#best-practices-1)

---

## Введение
Параллельная сборка в Gradle — это не просто ускорение билдов, а обязательное требование для систем с 500+ микросервисами, где ежедневные сборки потребляют тысячи CPU-часов. Например, в инфраструктуре Alibaba Cloud включение параллелизма сократило время деплоя критических обновлений с 47 до 9 минут (данные из доклада QCon 2024).

---

## Основные концепции
### Практическое определение
Параллельная сборка — это выполнение **независимых задач одновременно** в рамках одного проекта или мультипроекта.
- **Worker API:** Механизм Gradle для управления пулом потоков.
- **Task dependencies:** Граф зависимостей, определяющий порядок выполнения.
- **Project isolation:** Запуск задач из разных проектов в отдельных процессах (через `--parallel-projects`).

### Контекст из индустрии
- **Netflix:** В 2024 году переход на параллельную сборку 1200+ Java-модулей снизил нагрузку на CI-серверы на 55% (источник: Netflix TechBlog).
- **LinkedIn:** Использование `--parallel` + `--max-workers=8` в Kotlin Multiplatform проектах уменьшило время сборки с 18 до 4 минут.

### Детализация процесса
1. **Анализ графа задач:**
   ```  
   [Проект A: taskX] → [Проект B: taskY]  
   [Проект C: taskZ]  
   ```  
   → Задачи без зависимостей выполняются параллельно.

2. **Распределение ресурсов:**
   ```  
   Максимальное число воркеров = min(CPU cores, --max-workers)  
   ```  

3. **Синхронизация:**
   ```  
   Задача D (проект 1) --depends-on--> Задача E (проект 2)  
   → E запускается только после завершения D.  
   ```  

---

## Архитектура параллельной сборки
### Компоненты системы
1. **Task Execution Graph:**
    - Формируется до начала сборки.
    - Определяет, какие задачи могут выполняться параллельно.

2. **Worker Daemons:**
    - Демоны Gradle, управляющие пулом потоков.
    - Переиспользуются между сборками для минимизации накладных расходов.

3. **Проектная изоляция:**
   ```mermaid  
   graph LR  
     A[Проект 1] -->|Собственный ClassLoader| B[Task 1]  
     C[Проект 2] -->|Изолированный процесс| D[Task 2]  
   ```  

### Ограничения
- **Память:** Каждый воркер потребляет 200-500 MB RAM.
- **I/O-бутылочное горлышко:** Параллельная компиляция 100+ модулей может перегрузить диск.

---

## Примеры конфигурации
### 1. Настройка параллелизма в `gradle.properties`
```properties  
# Максимальное количество параллельных задач  
org.gradle.parallel=true  
org.gradle.workers.max=4  

# Параллелизм на уровне проектов  
org.gradle.parallel.projects=true  

# Ограничение памяти для воркеров  
org.gradle.jvmargs=-Xmx2g -XX:MaxMetaspaceSize=512m  
```  

### 2. Исключение задач из параллельного выполнения
```kotlin (build.gradle.kts)  
tasks.register("generateConfig") {  
    doLast {  
        // Генерация конфигов с блокировкой файла  
        File("config.env").writeText("API_KEY=${System.getenv("SECRET")}")  
    }  
    // Запрет параллелизма для задач с общим ресурсом  
    mustRunAfter(allprojects.map { it.tasks.named("compileJava") })  
}  
```  

### 3. Интеграция с Kubernetes (оптимизация ресурсов)
```yaml (Deployment для CI)  
spec:  
  containers:  
  - name: gradle-runner  
    resources:  
      limits:  
        cpu: "4"  
        memory: 8Gi  
    env:  
    - name: ORG_GRADLE_WORKERS_MAX  
      value: "4"  
```  

---

## Ошибки и решения
### Критические антипаттерны
1. **Параллельный доступ к shared resources без синхронизации:**
    - **Симптом:** Случайные падения сборки с `FileNotFoundException`.
    - **Решение:** Использовать `synchronized` блоки или `@SharedResource` аннотации.

2. **Игнорирование `mustRunAfter`/`shouldRunAfter`:**
    - **Пример ошибки:**
      ```  
      Задача кэширования (projectA) ← Конфликт → Задача очистки (projectB)  
      ```  
    - **Исправление:**
      ```kotlin  
      tasks.named("clean").configure {  
          mustRunAfter(tasks.named("cacheSetup"))  
      }  
      ```  

### Сравнение стратегий
| Подход                  | Рекомендуемый сценарий          | Риски                     |  
|-------------------------|----------------------------------|---------------------------|  
| `--parallel`            | Мультипроектные сборки          | Конфликты ресурсов        |  
| `--max-workers=N`       | Серверы с 16+ ядрами            | Перегрузка памяти         |  
| Изоляция проектов       | Несовместимые версии библиотек  | Увеличение времени старта |  

---

## Best Practices
### 1. Динамическое управление воркерами
```kotlin  
// Автоматическая настройка воркеров по количеству ядер  
val maxWorkers = Runtime.getRuntime().availableProcessors() - 1  
tasks.withType<JavaCompile>().configureEach {  
    options.isFork = true  
    options.forkOptions.javaHome = File(System.getenv("JAVA_HOME"))  
    // Ограничение для тяжелых задач  
    if (name.contains("Test")) {  
        options.forkOptions.maxHeapSize = "1g"  
    }  
}  
```  

### 2. Мониторинг параллелизма
- **Gradle Enterprise Metrics:**
  ```sql  
  SELECT project, task_name, wall_time  
  FROM build_metrics  
  WHERE parallel_worker_id IS NOT NULL  
  ORDER BY wall_time DESC  
  ```  
- **Prometheus Alert:**
  ```yaml  
  - alert: GradleParallelOverload  
    expr: avg(gradle_worker_memory_usage) > 85%  
    for: 5m  
    labels:  
      severity: critical  
  ```  

### 3. Интеграция с GitLab CI
```yaml  
build:  
  stage: build  
  script:  
    - ./gradlew assemble --parallel --max-workers=$CI_AVAILABLE_CPUS  
  artifacts:  
    paths:  
      - build/libs/  
  resource_group: gradle_build_${CI_PROJECT_ID} # Блокировка для shared resources  
```  

### 4. Оптимизация для монолитов
```kotlin  
allprojects {  
    tasks.withType<Test>().configureEach {  
        maxParallelForks = 3 // Параллельный запуск тестов  
        forkEvery = 100       // Перезапуск JVM каждые 100 тестов  
    }  
}  
```  

### 5. Безопасность при параллелизме
- **Изоляция секретов:**
  ```kotlin  
  tasks.withType<ProcessResources>().configureEach {  
    inputs.property("apiKey", providers.environmentVariable("API_KEY").forUseAtConfigurationTime())  
    // Запрет передачи секретов через память воркеров  
    doLast {  
      delete("${outputDir}/secrets.properties")  
    }  
  }  
  ```  
