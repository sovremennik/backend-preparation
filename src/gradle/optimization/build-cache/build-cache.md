#### [Введение](#Введение-1)
#### [Основные концепции](#Основные-концепции-1)
#### [Архитектура кэширования в Gradle](#Архитектура-кэширования-в-gradle-1)
#### [Примеры конфигурации](#Примеры-конфигурации-1)
#### [Ошибки и решения](#Ошибки-и-решения-1)
#### [Best Practices](#best-practices-1)

---

## Введение
Кэширование в Gradle — это не просто оптимизация, а фундаментальный механизм для выживания CI/CD-систем при нагрузках в 1000+ сборок в день. Например, в микросервисной архитектуре Netflix отсутствие кэширования задач компиляции увеличивает время деплоя критических фиксов на 300%, что недопустимо в SLA с downtime <1 минуты в год.

---

## Основные концепции
### Практическое определение
Кэширование в Gradle — это система **детерминированного сохранения результатов задач**, где:
- **Входные данные (inputs):** Файлы, переменные, метаданные, влияющие на результат задачи.
- **Кэш-ключ:** Хэш, вычисляемый из inputs + типа задачи + версии Gradle.
- **Кэш-хранилище:** Локальное (`.gradle/caches`) или удаленное (S3, GCS, Artifactory).

### Контекст из индустрии
- **Google Android Studio:** В 2023 году внедрение распределенного кэша для 1500+ модулей сократило время сборки с 22 до 4 минут (данные из Android Dev Summit).
- **JetBrains IntelliJ:** Использование `--build-cache` + remote cache в GitHub Actions уменьшило потребление CPU на 40% (отчет Q3 2023).

### Детализация процесса
1. **Инкрементальная сборка:**
   ```  
   [Изменение файла] → [Gradle вычисляет delta] → [Пересборка только затронутых задач]  
   ```  
2. **Кэширование:**
   ```  
   [Задача выполнена] → [Хэширование inputs] → [Сохранение outputs в кэш]  
   ```  
3. **Восстановление:**
   ```  
   [Задача запущена] → [Проверка кэша по хэшу] → [Загрузка outputs из кэша, если совпадение]  
   ```  

---

## Архитектура кэширования в Gradle
### Компоненты системы
1. **Local Cache:**
    - Расположение: `~/.gradle/caches/build-cache-1`.
    - Особенность: Автоматическая очистка при достижении лимита (по умолчанию 5 ГБ).
2. **Remote Cache:**
    - Протоколы: HTTP/REST, S3, GCS.
    - Безопасность: Поддержка TLS и аутентификации через заголовки.

### Взаимодействие с инфраструктурой
```  
CI Server (Jenkins) → [Запуск сборки] → Проверка кэша в S3 → [Кэш найден] → Пропуск задачи  
                                   ↓  
                               [Кэш отсутствует] → Выполнение задачи → Сохранение в S3  
```  

---

## Примеры конфигурации
### 1. Настройка кэша для Kotlin Multiplatform
```kotlin (build.gradle.kts)  
tasks.withType<org.jetbrains.kotlin.gradle.tasks.KotlinCompile>().configureEach {  
    inputs.property("kotlinVersion", KotlinVersion.CURRENT)  
    outputs.cacheIf {  
        !project.hasProperty("disableCache") // Условное кэширование  
    }  
}  

// Настройка удаленного кэша через S3  
buildCache {  
    remote<AwsS3BuildCache> {  
        region = "us-east-1"  
        bucket = "gradle-cache-company"  
        prefix = "project-alpha"  
        credentials {  
            accessKey = System.getenv("AWS_ACCESS_KEY")  
            secretKey = System.getenv("AWS_SECRET_KEY")  
        }  
    }  
}  
```  

### 2. Исключение кэширования для недетерминированных задач
```kotlin  
tasks.register("generateUniqueId") {  
    outputs.doNotCacheIf("Зависит от времени") { true }  
    doLast {  
        File("$buildDir/unique.id").writeText(System.currentTimeMillis().toString())  
    }  
}  
```  

### 3. Интеграция с Kubernetes (Helm)
```yaml (values.yaml)  
gradle:  
  cache:  
    enabled: true  
    s3:  
      bucket: "{{ .Values.environment }}-gradle-cache"  
      endpoint: "https://s3.amazonaws.com"  
```  

---

## Ошибки и решения
### Фатальные ошибки
1. **Кэширование задач с `doLast { ... }` без объявления `outputs`:**
    - **Симптом:** Сборка падает с `Cache entry is corrupt`.
    - **Решение:** Всегда объявлять `inputs`/`outputs` для кастомных задач.

2. **Использование `buildCache { local { ... } }` в CI:**
    - **Проблема:** Каждая сборка начинается с чистого кэша.
    - **Исправление:** Подключить удаленный кэш + `push=true` для CI.

### Сравнение стратегий инвалидации кэша
| Метод                     | Когда использовать          | Риски                     |  
|---------------------------|-----------------------------|---------------------------|  
| `--refresh-dependencies`  | При подозрении на артефакты | Полная пересборка deps    |  
| `cleanBuildCache`          | Смена версии Gradle         | Потеря всех кэшей         |  
| Ручное удаление по ключу   | Точечные исправления        | Сложность администрирования |  

---

## Best Practices
### 1. Приоритизация кэшируемых задач
- **Топ-3 кандидата:**
    1. `compileJava` / `compileKotlin` (экономия до 70% времени).
    2. `testClasses` (если тесты не зависят от окружения).
    3. `dockerBuild` (с кэшем слоев).

### 2. Мониторинг и анализ
- **Gradle Enterprise:** Настройка дашбордов для:
    - Hit Rate: `(cache hits / total tasks) * 100`.
    - Экономия времени: `SUM(saved_time_per_task * hits)`.
- **Prometheus-метрики:**
  ```yaml  
  - pattern: 'org.gradle.cache.<cache>.<type>:<name> <value>'  
    name: 'gradle_cache_${cache}_${type}'  
    labels:  
      project: '$name'  
  ```  

### 3. Security Hardening
- **Подпись кэша:**
  ```kotlin  
  buildCache {  
    remote<HttpBuildCache> {  
        isUseExpectContinue = true  
        isPush = true  
        credentials {  
            username = System.getenv("CACHE_USER")  
            password = System.getenv("CACHE_PASSWORD")  
        }  
        // GPG-подпись артефактов  
        signingKey = File("secret-key.asc").readText()  
    }  
  }  
  ```  

### 4. Оптимизация для монолитов
```kotlin  
// build.gradle.kts монолита  
allprojects {  
    tasks.withType<JavaCompile>().configureEach {  
        options.isIncremental = true  
        options.compilerArgs.addAll("-parameters", "-Xlint:unchecked")  
    }  
    // Кэшировать только release-сборки  
    buildCache {  
        local {  
            enabled = !project.hasProperty("ci")  
        }  
    }  
}  
```  

### 5. Disaster Recovery
- **Репликация кэша:** Настройка cross-region replication в S3.
- **Ежедневный бэкап:**
  ```bash  
  aws s3 sync s3://gradle-cache s3://gradle-cache-backup --delete  
  ```  
