#### [Введение](#Введение-1)
#### [Теоретическая база](#Теоретическая-база-1)
#### [Практическая часть](#практическая-часть-1)
#### [Типичные ошибки и решения](#Типичные-ошибки-и-решения-1)
#### [Рекомендации и Best Practices](#Рекомендации-и-best-practices-1)
#### [Дополнительные материалы](#Дополнительные-материалы-1)
#### [Заключение](#Заключение-1)

---

## Введение
### 1.1. Зачем это нужно?
Spring Framework — это мощный инструмент для разработки enterprise-приложений на Java/Kotlin. Spring Core, являясь ядром фреймворка, предоставляет основные механизмы для управления зависимостями, внедрения инверсии контроля (IoC) и обеспечения модульности приложения. Изучение Spring Core необходимо для создания гибких, масштабируемых и легко поддерживаемых систем.

Пример применения: Компания использует Spring Core для реализации микросервисной архитектуры, где каждый сервис является автономным компонентом с минимальными зависимостями от других частей системы.

### 1.2. Базовые термины
- **IoC (Inversion of Control)**: Механизм передачи управления созданием объектов из кода в контейнер Spring.
- **DI (Dependency Injection)**: Паттерн проектирования, позволяющий внедрять зависимости в объекты через конструкторы, сеттеры или аннотации.
- **Bean**: Объект, управляемый Spring-контейнером. Определяется как класс с аннотацией `@Component` или декларируется в XML-конфигурации.
- **ApplicationContext**: Контекст приложения, который загружает конфигурацию и управляет бинами.

```
+-----------------------+
| ApplicationContext    |
|   + Bean A           |
|   + Bean B           |
|   + Bean C           |
+-----------------------+
```

---

## Теоретическая база
### 2.1. Основные концепции
Spring Core работает на основе двух ключевых принципов:
1. **Инверсия контроля (IoC)**: Разработчик не создает объекты напрямую, а делегирует эту задачу контейнеру Spring.
2. **Внедрение зависимостей (DI)**: Зависимости между компонентами устанавливаются автоматически через конфигурацию.

Архитектура Spring Core состоит из следующих слоев:
- **Core Container**: Управляет жизненным циклом бинов.
- **Data Access/Integration**: Интеграция с базами данных и другими источниками данных.
- **Web**: Поддержка разработки веб-приложений.

```
+-----------------------------+
|          Core Container     |
|                             |
|   + BeanFactory             |
|   + ApplicationContext      |
+-----------------------------+
```

### 2.2. Углубленные темы
- **Жизненный цикл бина**:
    - Создание бина (`@PostConstruct`)
    - Инициализация (`InitializingBean`)
    - Уничтожение (`DisposableBean`)

- **Сравнение с Guice**: Guice также реализует DI, но не предоставляет таких расширенных возможностей, как Spring Boot или Spring Data.

### 2.3. Теория в схемах
```
+-------------------+       +-------------------+       +-------------------+
| Configuration     | --->  | ApplicationContext | --->  | Beans             |
| (Java, XML, YAML) |       |                   |       |                   |
+-------------------+       +-------------------+       +-------------------+
```

---

## Практическая часть
### 3.1. Базовые примеры
#### Пример 1: Создание простого бина
```java
// Определение бина
@Component
public class GreetingService {
    public String greet() {
        return "Hello, World!";
    }
}

// Использование бина
@SpringBootApplication
public class MainApp {
    public static void main(String[] args) {
        ApplicationContext context = SpringApplication.run(MainApp.class, args);
        GreetingService service = context.getBean(GreetingService.class);
        System.out.println(service.greet());
    }
}
```

#### Пример 2: Внедрение зависимости
```java
@Component
public class EmailService {
    public void sendEmail(String message) {
        System.out.println("Sending email: " + message);
    }
}

@Component
public class NotificationService {
    private final EmailService emailService;

    @Autowired
    public NotificationService(EmailService emailService) {
        this.emailService = emailService;
    }

    public void notifyUser(String message) {
        emailService.sendEmail(message);
    }
}
```

### 3.2. Реальные кейсы
#### Сценарий: Логирование действий пользователя
```java
@Component
public class UserActivityLogger {
    private final AuditService auditService;

    @Autowired
    public UserActivityLogger(AuditService auditService) {
        this.auditService = auditService;
    }

    public void logAction(String action) {
        auditService.record(action);
    }
}
```

### 3.3. Задачи для самопроверки
1. Добавьте дополнительную зависимость в `NotificationService` для отправки SMS.
2. Реализуйте механизм ленивой инициализации бина с помощью аннотации `@Lazy`.

---

## Типичные ошибки и решения
### 4.1. Распространенные ошибки
- **Ошибка NoUniqueBeanDefinitionException**: Возникает, когда в контексте есть несколько бинов одного типа.
- **Ошибка BeanCreationException**: Появляется, если бин не может быть создан из-за неправильной конфигурации.

### 4.2. Как их избежать?
- Используйте аннотацию `@Qualifier`, чтобы явно указывать нужный бин.
- Проверяйте корректность конфигурации перед запуском приложения.

---

## Рекомендации и Best Practices
### 5.1. Советы экспертов
- Используйте конфигурацию на основе Java вместо XML.
- Разделяйте конфигурацию по профилям (`@Profile`) для разных сред (dev, test, prod).
- Активно применяйте аннотации для упрощения кода.

### 5.2. Инструменты и ресурсы
- **IntelliJ IDEA**: IDE с отличной поддержкой Spring.
- **Spring Boot DevTools**: Для горячей перезагрузки кода.

---

## Дополнительные материалы
### 6.1. Глоссарий
- **BeanFactory**: Интерфейс для создания и управления бинами.
- **Profiles**: Набор конфигураций для разных сред разработки.

### 6.2. Полезные ссылки
- [Официальная документация Spring](https://spring.io/docs)
- [Spring Boot Reference Guide](https://docs.spring.io/spring-boot/docs/current/reference/htmlsingle/)

### 6.3. Тесты/Опросы
1. Что такое IoC?  
   Ответ: Инверсия контроля — механизм передачи управления созданием объектов.

---

## Заключение
### 7.1. Итоги
Spring Core позволяет эффективно управлять зависимостями и создавать модульные приложения. Понимание его основных принципов — залог успешной разработки enterprise-систем.

### 7.2. Что дальше?
- Изучите Spring Boot для быстрой разработки приложений.
- Ознакомьтесь с Spring Data для работы с базами данных.