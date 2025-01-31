#### [Введение](#Введение-1)
#### [Основные концепции](#Основные-концепции-1)
#### [Примеры конфигурации](#Примеры-конфигурации-1)
#### [Ошибки и решения](#Ошибки-и-решения-1)
#### [Best Practices](#best-practices-1)

---

## Введение
GitHub Actions — это встроенная CI/CD система в GitHub, позволяющая автоматизировать сборку, тестирование и развертывание приложений. Интеграция Gradle с GitHub Actions упрощает автоматическое тестирование и публикацию Java/Kotlin проектов. Это критически важно для backend-разработчиков, работающих с высоконагруженными сервисами, так как позволяет ускорить выпуск новых версий, повысить надежность инфраструктуры и упростить процесс поддержки проектов.

## Основные концепции
- **Workflow (рабочий процесс):** Определяет последовательность автоматизированных задач, выполняемых в ответ на события (например, push в репозиторий, открытие PR).
- **Job (задача):** Отдельный блок работы внутри workflow, состоящий из шагов (steps). Jobs могут выполняться параллельно или зависеть друг от друга.
- **Step (шаг):** Конкретная операция внутри job, например, установка зависимостей, компиляция, тестирование.
- **Runner (исполняющая среда):** Виртуальная машина, на которой выполняются jobs (например, `ubuntu-latest`, `windows-latest`).
- **Кеширование:** Используется для ускорения сборки, сохраняя артефакты и зависимости между запусками, сокращая время выполнения pipeline.
- **Артефакты:** Файлы, полученные после сборки, которые можно загружать и использовать в последующих этапах (например, JAR-файлы, отчеты о тестах).

## Примеры конфигурации

Создадим workflow для сборки и тестирования проекта на Gradle.

### `.github/workflows/build.yml`
```yaml
name: Build & Test

description: CI pipeline для сборки и тестирования Gradle-проекта

on:
  push:
    branches:
      - main
  pull_request:
    branches:
      - main

jobs:
  build:
    runs-on: ubuntu-latest

    steps:
      - name: Checkout repository
        uses: actions/checkout@v4

      - name: Setup JDK
        uses: actions/setup-java@v3
        with:
          distribution: 'temurin'
          java-version: '17'
          cache: 'gradle'

      - name: Grant execute permission for Gradle
        run: chmod +x ./gradlew

      - name: Build and test
        run: ./gradlew build --no-daemon

      - name: Upload test reports
        if: always()
        uses: actions/upload-artifact@v3
        with:
          name: test-reports
          path: build/reports/tests
```

### Разбор конфигурации
1. **`on:`** – Определяет, когда запускается workflow. В данном случае — при push и pull request в `main`.
2. **`jobs:`** – Содержит список задач. `build` выполняется на `ubuntu-latest`.
3. **`actions/checkout@v4`** – Загружает код репозитория.
4. **`actions/setup-java@v3`** – Устанавливает JDK 17 и настраивает кеширование зависимостей.
5. **`chmod +x ./gradlew`** – Дает права на исполнение скрипту `gradlew`.
6. **`./gradlew build --no-daemon`** – Собирает и тестирует проект, выключая `Gradle Daemon` для стабильности.
7. **`actions/upload-artifact@v3`** – Загружает отчеты о тестах как артефакты.

## Ошибки и решения

| Ошибка | Причина | Решение |
|--------|---------|---------|
| `Permission denied` при запуске Gradle | `gradlew` не имеет прав на выполнение | Добавить `chmod +x ./gradlew` в workflow |
| `Could not resolve dependencies` | Отсутствует кеширование | Использовать `cache: 'gradle'` в `setup-java` |
| `OutOfMemoryError` | Недостаточно памяти на runner | Увеличить `Xmx` в `GRADLE_OPTS` |
| `Error: Process completed with exit code 137` | Процесс убит из-за нехватки памяти | Запустить `./gradlew build --no-daemon` |
| `Test reports are missing` | Отчеты о тестах не загружены как артефакты | Добавить `actions/upload-artifact@v3` |

## Best Practices
1. **Кеширование зависимостей** – Использование `cache: 'gradle'` сокращает время выполнения pipeline.
2. **Оптимизация Gradle** – Добавление `--no-daemon` предотвращает утечки памяти на CI.
3. **Разделение задач** – Вынос тестов в отдельный job (`test`) позволяет выполнять их параллельно.
4. **Загрузка артефактов** – Логи и отчеты тестов помогают при разборе ошибок.
5. **Мониторинг CI/CD** – Подключение уведомлений о статусе сборки (Slack, Telegram).
6. **Безопасность** – Использование `secrets.GITHUB_TOKEN` для безопасного взаимодействия с GitHub API.

