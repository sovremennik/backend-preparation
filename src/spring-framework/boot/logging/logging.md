#### [Введение](#Введение-1)
#### [Теоретическая-база](#Теоретическая-база-1)
#### [Практическая-часть](#Практическая-часть-1)
#### [Типичные ошибки и решения](#Типичные-ошибки-и-решения-1)
#### [Рекомендации и Best Practices](#Рекомендации-и-Best-Practices-1)
#### [Дополнительные материалы](#Дополнительные-материалы-1)
#### [Заключение](#Заключение-1)

---

## Введение

### 1.1. Зачем это нужно?
Логирование и мониторинг — это ключевые инструменты для отслеживания состояния приложения, обнаружения проблем и анализа производительности. Spring Boot предоставляет встроенные механизмы для логирования и интеграции с системами мониторинга через модуль **Spring Boot Actuator**.

Пример применения: Компании используют Actuator для сбора метрик, отслеживания здоровья микросервисов и настройки уровня детализации логов в зависимости от среды (разработка, тестирование, production).

---

## Теоретическая база

### 2.1. Логирование в Spring Boot

#### 2.1.1. Стандартные логгеры
Spring Boot по умолчанию использует **Logback** как бэкенд для логирования. Он поддерживает следующие уровни логирования:
- `TRACE`: Детальная информация о работе системы.
- `DEBUG`: Информация для отладки.
- `INFO`: Общая информация о работе приложения.
- `WARN`: Предупреждения о потенциальных проблемах.
- `ERROR`: Ошибки, которые могут повлиять на работу приложения.

#### 2.1.2. Настройка логирования
Настройка логирования выполняется через файл `application.properties` или `application.yml`.

**Пример настройки в `application.properties`:**
```properties
# Установка глобального уровня логирования
logging.level.root=INFO

# Настройка уровня логирования для конкретных пакетов
logging.level.org.springframework.web=DEBUG
logging.level.com.example.myapp=TRACE

# Настройка файла логов
logging.file.name=logs/app.log
logging.file.max-size=10MB
```

#### 2.1.3. Создание собственных логгеров
Для создания логгеров используется стандартный API SLF4J (Simple Logging Facade for Java). Пример:
```java
import org.slf4j.Logger;
import org.slf4j.LoggerFactory;

public class MyService {
    private static final Logger logger = LoggerFactory.getLogger(MyService.class);

    public void performTask() {
        logger.info("Task started");
        try {
            // Бизнес-логика
            logger.debug("Processing data...");
        } catch (Exception e) {
            logger.error("Error occurred", e);
        }
        logger.info("Task completed");
    }
}
```

---

### 2.2. Мониторинг с помощью Spring Boot Actuator

#### 2.2.1. Что такое Actuator?
Actuator — это модуль Spring Boot, который предоставляет готовые эндпоинты для мониторинга и управления приложением. Он позволяет:
- Отслеживать состояние приложения.
- Собирать метрики производительности.
- Выполнять административные операции (например, перезагрузку конфигурации).

#### 2.2.2. Основные эндпоинты Actuator
| Эндпоинт               | Описание                                                                 |
|------------------------|--------------------------------------------------------------------------|
| `/actuator/health`     | Проверка работоспособности приложения.                                   |
| `/actuator/metrics`    | Сбор метрик (например, использование CPU, памяти, запросов в секунду).   |
| `/actuator/loggers`     | Управление уровнями логирования в runtime.                               |
| `/actuator/env`        | Информация о переменных окружения.                                       |
| `/actuator/info`       | Пользовательская информация о приложении (например, версия, build info). |

#### 2.2.3. Настройка Actuator
Добавьте зависимость `spring-boot-starter-actuator` в проект:
```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-actuator</artifactId>
</dependency>
```

По умолчанию доступ к эндпоинтам ограничен. Чтобы разрешить доступ, добавьте следующие свойства:
```properties
management.endpoints.web.exposure.include=health,metrics,loggers,env,info
management.endpoint.health.show-details=always
```

---

### 2.3. Интеграция с внешними системами мониторинга

#### 2.3.1. Prometheus и Grafana
Prometheus — это система мониторинга, которая может собирать метрики из Spring Boot приложений через Actuator. Для интеграции добавьте зависимость:
```xml
<dependency>
    <groupId>io.micrometer</groupId>
    <artifactId>micrometer-registry-prometheus</artifactId>
</dependency>
```

Эндпоинт `/actuator/prometheus` будет предоставлять метрики в формате, понятном Prometheus.

#### 2.3.2. ELK Stack (Elasticsearch, Logstash, Kibana)
ELK Stack используется для централизованного хранения и анализа логов. Для интеграции можно использовать библиотеку Logstash:
```xml
<dependency>
    <groupId>net.logstash.logback</groupId>
    <artifactId>logstash-logback-encoder</artifactId>
</dependency>
```

Настройте логирование JSON-формата в `logback-spring.xml`:
```xml
<appender name="LOGSTASH" class="net.logstash.logback.appender.LogstashTcpSocketAppender">
    <destination>localhost:5000</destination>
</appender>
```

---

## Практическая часть

### 3.1. Базовые примеры

#### Пример 1: Настройка логирования
Создайте файл `logback-spring.xml` для настройки логгера:
```xml
<configuration>
    <appender name="STDOUT" class="ch.qos.logback.core.ConsoleAppender">
        <encoder>
            <pattern>%d{yyyy-MM-dd HH:mm:ss} %-5level %logger{36} - %msg%n</pattern>
        </encoder>
    </appender>

    <root level="INFO">
        <appender-ref ref="STDOUT"/>
    </root>
</configuration>
```

Используйте логгер в коде:
```java
import org.slf4j.Logger;
import org.slf4j.LoggerFactory;

@RestController
public class HelloController {
    private static final Logger logger = LoggerFactory.getLogger(HelloController.class);

    @GetMapping("/hello")
    public String sayHello() {
        logger.info("Handling request for /hello");
        return "Hello, World!";
    }
}
```

---

#### Пример 2: Использование Actuator
Добавьте зависимость `spring-boot-starter-actuator`:
```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-actuator</artifactId>
</dependency>
```

Настройте экспозу эндпоинтов:
```properties
management.endpoints.web.exposure.include=health,metrics,loggers,env,info
management.endpoint.health.show-details=always
```

Проверьте доступность эндпоинтов:
- `/actuator/health`: Проверка состояния приложения.
- `/actuator/metrics`: Сбор метрик.
- `/actuator/loggers`: Управление уровнями логирования.

---

### 3.2. Реальные кейсы

#### Сценарий: Централизованное логирование с ELK Stack
1. **Настройка Logstash**:
    - Создайте конфигурацию Logstash для приема логов от приложения.
    - Пример конфигурации:
      ```plaintext
      input {
          tcp {
              port => 5000
              codec => json
          }
      }
      output {
          elasticsearch {
              hosts => ["http://localhost:9200"]
          }
      }
      ```

2. **Настройка логгера в Spring Boot**:
    - Добавьте зависимость для Logstash:
      ```xml
      <dependency>
          <groupId>net.logstash.logback</groupId>
          <artifactId>logstash-logback-encoder</artifactId>
      </dependency>
      ```
    - Настройте логгер в `logback-spring.xml`:
      ```xml
      <appender name="LOGSTASH" class="net.logstash.logback.appender.LogstashTcpSocketAppender">
          <destination>localhost:5000</destination>
      </appender>
      ```

3. **Анализ логов в Kibana**:
    - Подключитесь к Kibana и создайте дашборд для анализа логов.

---

#### Сценарий: Мониторинг с Prometheus и Grafana
1. **Настройка Prometheus**:
    - Добавьте dependency для Micrometer:
      ```xml
      <dependency>
          <groupId>io.micrometer</groupId>
          <artifactId>micrometer-registry-prometheus</artifactId>
      </dependency>
      ```
    - Настройте Prometheus для скraping метрик с эндпоинта `/actuator/prometheus`.

2. **Настройка Grafana**:
    - Создайте дашборд в Grafana для визуализации метрик.
    - Используйте предопределенные шаблоны для Spring Boot приложений.

---

## Типичные ошибки и решения

### 4.1. Распространенные ошибки

1. **Недоступные эндпоинты Actuator**:
    - Проблема: Эндпоинты не доступны после добавления зависимости.
    - Решение: Убедитесь, что вы правильно настроили экспозу эндпоинтов в `application.properties`.

2. **Конфликты между логгерами**:
    - Проблема: Конфликт между различными логгерами (например, Logback и Log4j).
    - Решение: Используйте только один бэкенд для логирования.

---

## Рекомендации и Best Practices

1. **Настройте уровень логирования для production**:
    - Используйте уровень `INFO` или выше для минимизации объема логов.

2. **Используйте профили для разных сред**:
    - Создайте отдельные файлы конфигурации (`application-dev.properties`, `application-prod.properties`) для каждой среды.

3. **Централизуйте логирование**:
    - Используйте ELK Stack или другие системы для хранения и анализа логов.

4. **Мониторьте важные метрики**:
    - Следите за использованием ресурсов (CPU, память), временем выполнения запросов и другими ключевыми показателями.

---

## Дополнительные материалы

### 6.1. Глоссарий

- **Actuator**: Модуль Spring Boot для мониторинга и управления приложением.
- **Micrometer**: Библиотека для сбора метрик, интегрируемая с Actuator.
- **ELK Stack**: Система для централизованного логирования (Elasticsearch, Logstash, Kibana).

### 6.2. Полезные ссылки

- [Официальная документация Spring Boot Actuator](https://docs.spring.io/spring-boot/docs/current/reference/htmlsingle/#actuator)
- [Micrometer Documentation](https://micrometer.io/docs)
- [Prometheus Guide](https://prometheus.io/docs/introduction/overview/)
- [Grafana Tutorials](https://grafana.com/docs/tutorials/)

---

## Заключение

### 7.1. Итоги
Логирование и мониторинг являются важными аспектами enterprise-приложений. Spring Boot Actuator предоставляет мощные инструменты для отслеживания состояния приложения, сбора метрик и управления логированием.

### 7.2. Что дальше?
- Изучите продвинутые возможности Actuator, такие как health checks и management endpoints.
- Ознакомьтесь с интеграцией Actuator с другими системами мониторинга (например, New Relic, Datadog).
- Исследуйте возможности централизованного логирования через ELK Stack или других решений.