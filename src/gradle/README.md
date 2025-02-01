### **Gradle**

---

#### **1. Основы Gradle**
- **Что такое Gradle**
  - Особенности и преимущества.
  - Отличия Gradle от Maven и Ant.
- **Архитектура Gradle**
  - Build Scripts.
  - Проекты и задачи (Projects and Tasks).
- **Установка и настройка**
  - Установка Gradle.
  - Проверка версии (`gradle --version`).

---

#### **2. Скрипты сборки**
- **Файлы конфигурации**
  - `build.gradle` (Groovy DSL).
  - `build.gradle.kts` (Kotlin DSL).
- **Основные блоки**
  - `plugins`, `dependencies`, `repositories`.
- **Команды Gradle**
  - `gradle build`, `gradle test`, `gradle clean`.

---

#### **3. Плагины Gradle**
- **Подключение плагинов**
  - Использование секции `plugins`.
  - Legacy-метод (`apply plugin ...`).
- **Основные плагины**
  - `java`.
  - `application`.
  - `kotlin`.
  - `spring-boot`.

---

#### **4. Задачи (Tasks)**
- **Определение задач**
  - Создание своих задач с использованием `task`.
  - Задачи с параметрами.
- **Управление зависимостями задач**
  - Зависимости через `dependsOn`.
  - Управление порядком через `mustRunAfter`, `shouldRunAfter`.
- **Типы задач**
  - Типовые задачи (`Copy`, `Exec`).
  - Настройка задач через `doFirst`, `doLast`.

---

### **Управление зависимостями**

#### **1. Подключение зависимостей**
- **Репозитории**
  - Maven Central, JCenter, локальные репозитории.
  - Настройка секции `repositories`.
- **Зависимости**
  - Секция `dependencies`.
  - Конфигурации зависимостей: `implementation`, `api`, `testImplementation`.

#### **2. Конфигурации зависимостей**
- **Виды конфигураций**
  - `compileOnly`, `runtimeOnly`, `annotationProcessor`.
- **Построение дерева зависимостей**
  - Использование `gradle dependencies`.
- **Управление версиями**
  - BOM (Bill of Materials).
  - Принудительное указание версии (`strictly`).

#### **3. Кэширование и сборка артефактов**
- Механизм кэширования Gradle.
- Настройка локального и удалённого кэша.

---

### **Многомодульные проекты**

#### **1. Структура многомодульных проектов**
- Определение корневого проекта.
- Создание модулей (Subprojects).
- Файл `settings.gradle`.

#### **2. Настройка модулей**
- Общие зависимости в корневом проекте.
- Перекрытие конфигураций в отдельных модулях.

#### **3. Задачи в многомодульных проектах**
- Выполнение задач для всех модулей (`gradle build`).
- Выполнение задач для конкретного модуля.

---

### **Оптимизация сборки**

#### **1. Кэширование и инкрементальная сборка**
- Использование Gradle Build Cache.
- Инкрементальная сборка: аннотации `@Incremental`, `@Input`.

#### **2. Параллельная сборка**
- Включение параллельного выполнения (`--parallel`).
- Оптимизация задач.

#### **3. Минимизация времени сборки**
- Анализ времени выполнения задач (`--profile`).
- Удаление ненужных зависимостей.

---

### **Сборка Java-проектов**

#### **1. Подключение Java плагина**
- Секция `plugins` для подключения `java`.
- Настройка JDK: указание версии Java.

#### **2. Компиляция и тестирование**
- Настройка компиляции: секция `java {}`.
- Подключение зависимостей для тестирования (`testImplementation`).

#### **3. Сборка JAR**
- Настройка артефакта: секция `jar {}`.
- Включение манифеста: добавление `Main-Class`.

---

### **Сборка Kotlin-проектов**

#### **1. Подключение Kotlin плагина**
- Секция `plugins`: подключение `org.jetbrains.kotlin.jvm`.

#### **2. Конфигурация компилятора Kotlin**
- Настройка версии Kotlin.
- Использование JVM-таргетов (`jvmTarget`).

#### **3. Совместимость с Java**
- Настройка зависимостей Kotlin и Java.
- Компиляция Kotlin-кода в Java-проекте.

---

### **Интеграция с инструментами**

#### **1. Spring Boot**
- Подключение Spring Boot плагина: `org.springframework.boot`.
- Настройка `application.properties` и конфигурации.

#### **2. Docker и контейнеризация**
- Генерация Docker-образа с помощью плагина.
- Примеры конфигурации для `jib`.

#### **3. CI/CD**
- Интеграция с Jenkins, GitLab CI, GitHub Actions.
- Использование Gradle Wrapper для автоматизации.

---

### **Продвинутая настройка**

#### **1. Расширение Gradle через плагины**
- Создание своих плагинов.
- Публикация в Gradle Plugin Portal.

#### **2. Скрипты-инициализаторы (Init Scripts)**
- Использование скриптов для настройки среды.

#### **3. Рефакторинг и переиспользование**
- Общие скрипты в многомодульных проектах.
- Настройка `buildSrc` для переиспользования кода.

---
```
📁 **gradle/**  
│  
├── 📄 **gradle.md**                      → Корневой файл документации (обзор всех разделов)  
│  
├── 📁 **fundamentals/**                  → Основы Gradle  
│   ├── 📄 fundamentals.md                → Обзор раздела (ссылки: overview, installation)  
│   ├── 📁 **overview/**                  → Введение в Gradle  
│   │   └── 📄 overview.md  
│   └── 📁 **installation/**              → Установка и настройка  
│       └── 📄 installation.md  
│  
├── 📁 **build-scripts/**                 → Скрипты сборки  
│   ├── 📄 build-scripts.md               → Обзор раздела (ссылки: groovy-dsl, kotlin-dsl)  
│   ├── 📁 **groovy-dsl/**                → Конфигурация на Groovy  
│   │   └── 📄 groovy-dsl.md  
│   └── 📁 **kotlin-dsl/**                → Конфигурация на Kotlin  
│       └── 📄 kotlin-dsl.md  
│  
├── 📁 **tasks/**                         → Задачи (Tasks)  
│   ├── 📄 tasks.md                       → Обзор раздела (ссылки: custom-tasks, task-dependencies)  
│   ├── 📁 **custom-tasks/**              → Создание кастомных задач  
│   │   └── 📄 custom-tasks.md  
│   └── 📁 **task-dependencies/**         → Управление зависимостями  
│       └── 📄 task-dependencies.md  
│  
├── 📁 **plugins/**                       → Плагины  
│   ├── 📄 plugins.md                     → Обзор раздела (ссылки: java-plugin, kotlin-plugin, spring-boot, docker)  
│   ├── 📁 **java-plugin/**               → Плагин для Java  
│   │   └── 📄 java-plugin.md  
│   ├── 📁 **kotlin-plugin/**             → Плагин для Kotlin  
│   │   └── 📄 kotlin-plugin.md  
│   ├── 📁 **spring-boot/**               → Spring Boot  
│   │   └── 📄 spring-boot.md  
│   └── 📁 **docker/**                    → Docker-интеграция  
│       └── 📄 docker.md  
│  
├── 📁 **dependencies/**                  → Управление зависимостями  
│   ├── 📄 dependencies.md                → Обзор раздела (ссылки: repositories, version-management)  
│   ├── 📁 **repositories/**              → Репозитории  
│   │   └── 📄 repositories.md  
│   └── 📁 **version-management/**        → Контроль версий  
│       └── 📄 version-management.md  
│  
├── 📁 **multi-module/**                  → Многомодульные проекты  
│   ├── 📄 multi-module.md                → Обзор раздела (ссылки: project-structure, module-configuration)  
│   ├── 📁 **project-structure/**         → Организация модулей  
│   │   └── 📄 project-structure.md  
│   └── 📁 **module-configuration/**      → Настройка модулей  
│       └── 📄 module-configuration.md  
│  
├── 📁 **optimization/**                  → Оптимизация сборки  
│   ├── 📄 optimization.md                → Обзор раздела (ссылки: build-cache, parallel-builds)  
│   ├── 📁 **build-cache/**               → Кэширование  
│   │   └── 📄 build-cache.md  
│   └── 📁 **parallel-builds/**           → Параллельная сборка  
│       └── 📄 parallel-builds.md  
│  
├── 📁 **language-support/**              → Поддержка языков  
│   ├── 📄 language-support.md            → Обзор раздела (ссылки: java-compilation, java-jar, kotlin-compiler, kotlin-jvm)  
│   ├── 📁 **java-compilation/**          → Компиляция Java  
│   │   └── 📄 java-compilation.md  
│   ├── 📁 **java-jar/**                  → Создание JAR для Java  
│   │   └── 📄 java-jar.md  
│   ├── 📁 **kotlin-compiler/**           → Настройки компилятора Kotlin  
│   │   └── 📄 kotlin-compiler.md  
│   └── 📁 **kotlin-jvm/**                → Совместимость Kotlin с JVM  
│       └── 📄 kotlin-jvm.md  
│  
├── 📁 **integration/**                   → Интеграция с инструментами  
│   ├── 📄 integration.md                 → Обзор раздела (ссылки: github-actions, gitlab-ci, monitoring)  
│   ├── 📁 **github-actions/**            → GitHub Actions  
│   │   └── 📄 github-actions.md  
│   ├── 📁 **gitlab-ci/**                 → GitLab CI  
│   │   └── 📄 gitlab-ci.md  
│   └── 📁 **monitoring/**                → Мониторинг  
│       └── 📄 monitoring.md  
│  
└── 📁 **advanced/**                      → Продвинутые темы  
    ├── 📄 advanced.md                     → Обзор раздела (ссылки: custom-plugins, build-src)  
    ├── 📁 **custom-plugins/**             → Создание плагинов  
    │   └── 📄 custom-plugins.md  
    └── 📁 **build-src/**                  → Переиспользование кода  
        └── 📄 build-src.md  
```