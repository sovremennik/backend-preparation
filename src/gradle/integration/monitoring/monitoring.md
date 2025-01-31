#### [Введение](#Введение-1)
#### [Основные концепции](#Основные-концепции-1)
#### [Примеры конфигурации](#Примеры-конфигурации-1)
#### [Ошибки и решения](#Ошибки-и-решения-1)
#### [Best Practices](#best-practices-1)

---

## Введение

Мониторинг сборочного процесса Gradle необходим для оптимизации времени выполнения, контроля потребления ресурсов и быстрого выявления проблем. В высоконагруженных системах ускорение CI/CD-сборок даже на 10–20% приводит к значительному снижению времени вывода нового кода в продакшн. В этом конспекте рассмотрены ключевые инструменты и методы для мониторинга Gradle в локальной среде и CI/CD (GitHub Actions, GitLab CI).

## Основные концепции

- **Build Scan** – инструмент для профилирования сборки, позволяющий анализировать продолжительность выполнения задач и зависимости.
- **Gradle Profiler** – инструмент для глубокого анализа времени выполнения задач и выявления узких мест.
- **Использование ресурсов** – контроль загрузки CPU, памяти и I/O во время сборки.
- **Логирование и диагностика** – анализ логов Gradle (`--info`, `--debug`, `--scan`).
- **Интеграция с CI/CD** – сбор и визуализация метрик в GitHub Actions/GitLab CI.

## Примеры конфигурации

### Включение Build Scan

```sh
./gradlew build --scan
```

После выполнения в консоли появится ссылка с детальной аналитикой времени выполнения задач, зависимостей и узких мест.

### Использование Gradle Profiler

1. Установите [Gradle Profiler](https://github.com/gradle/gradle-profiler):
   ```sh
   brew install gradle-profiler
   ```
2. Запустите анализ:
   ```sh
   gradle-profiler --benchmark --project-dir .
   ```

### Мониторинг использования ресурсов

Запуск сборки с детальными логами по CPU и памяти:
```sh
./gradlew build --info | tee build.log
```

Для профилирования можно использовать `time` и `top`:
```sh
time ./gradlew build
top -pid $(pgrep -f gradle)
```

### Интеграция с GitHub Actions

#### `.github/workflows/gradle-monitoring.yml`
```yaml
name: Gradle Monitoring

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

      - name: Build with profiling
        run: ./gradlew build --scan --info

      - name: Upload logs
        uses: actions/upload-artifact@v3
        with:
          name: gradle-logs
          path: build.log
```

### Интеграция с GitLab CI

#### `.gitlab-ci.yml`
```yaml
stages:
  - build

variables:
  GRADLE_OPTS: "-Dorg.gradle.daemon=false"

build_project:
  stage: build
  image: gradle:jdk17
  script:
    - gradle build --scan --info | tee build.log
  artifacts:
    paths:
      - build.log
```

## Ошибки и решения

| Ошибка | Причина | Решение |
|--------|---------|---------|
| `Build scan is not enabled` | Включение `--scan` требует принятия условий использования | Запустить `./gradlew build --scan` и подтвердить |
| `Gradle out of memory` | Недостаточно RAM | Увеличить `GRADLE_OPTS="-Xmx4g"` |
| `High CPU usage during build` | Параллельная сборка нагружает CPU | Ограничить `org.gradle.workers.max=2` в `gradle.properties` |
| `Gradle Daemon keeps running` | Демон Gradle остается в памяти и потребляет ресурсы | Явно отключить `org.gradle.daemon=false` |

## Best Practices

1. **Используйте Build Scan** – Для детального анализа времени выполнения задач.
2. **Ограничивайте количество параллельных задач** – Установите `org.gradle.workers.max=2` для сбалансированного потребления ресурсов.
3. **Логируйте метрики** – Собирайте логи `--info` и анализируйте их в CI/CD.
4. **Настройте алерты** – В GitHub Actions используйте `on: failure` для уведомлений о падениях.
5. **Оптимизируйте кеширование** – В GitLab CI включите кеш `cache:key` для ускорения повторных сборок.

