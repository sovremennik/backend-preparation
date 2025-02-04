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
Инверсия управления (IoC) позволяет делегировать создание объектов и управление их зависимостями внешнему контейнеру. Это упрощает тестирование, повышает гибкость кода и облегчает его поддержку.

Пример применения: В крупной компании используется IoC для создания микросервисов с минимальными зависимостями друг от друга.

---

## Теоретическая база
### 2.1. Основные концепции
IoC работает на основе следующих принципов:
1. **Контейнер управляет созданием объектов**.
2. **Зависимости между объектами устанавливаются автоматически**.

Основные типы внедрения зависимостей:
- **Конструкторная инъекция**.
- **Сеттерная инъекция**.
- **Полевая инъекция**.

### 2.2. Углубленные темы
- **Жизненный цикл бина**:
    - Создание (`@PostConstruct`)
    - Инициализация (`InitializingBean`)
    - Уничтожение (`DisposableBean`)

- **Сравнение с традиционным подходом**: В классическом подходе разработчик самостоятельно создает объекты и устанавливает зависимости. В IoC это делает контейнер.

---

## Практическая часть
### 3.1. Базовые примеры
#### Пример 1: Конструкторная инъекция
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

    // Конструкторная инъекция
    @Autowired
    public NotificationService(EmailService emailService) {
        this.emailService = emailService;
    }

    public void notifyUser(String message) {
        emailService.sendEmail(message);
    }
}
```

#### Пример 2: Сеттерная инъекция
```java
@Component
public class LoggingService {
    private String logMessage;

    // Сеттерная инъекция
    @Autowired
    public void setLogMessage(String logMessage) {
        this.logMessage = logMessage;
    }

    public void log() {
        System.out.println("Logging: " + logMessage);
    }
}
```

#### Пример 3: Полевая инъекция
```java
@Component
public class UserService {
    @Autowired
    private UserRepository userRepository;

    public void createUser(String name) {
        userRepository.save(name);
    }
}
```

### 3.2. Реальные кейсы
#### Сценарий: Реализация системы аутентификации
```java
@Component
public class AuthenticationService {
    private final UserDetailsService userDetailsService;

    @Autowired
    public AuthenticationService(UserDetailsService userDetailsService) {
        this.userDetailsService = userDetailsService;
    }

    public boolean authenticate(String username, String password) {
        UserDetails user = userDetailsService.loadUserByUsername(username);
        return passwordEncoder.matches(password, user.getPassword());
    }
}
```

### 3.3. Задачи для самопроверки
1. Реализуйте систему логирования действий пользователя с использованием IoC.
2. Создайте два бина одного типа и настройте выбор нужного бина через аннотацию `@Qualifier`.

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
- Предпочтительнее использовать конструкторную инъекцию для обязательных зависимостей.
- Избегайте использования полевой инъекции в публичных API.
- Разделяйте конфигурацию по профилям (`@Profile`) для разных сред (dev, test, prod).

---

## Дополнительные материалы
### 6.1. Глоссарий
- **Autowiring**: Процесс автоматического связывания зависимостей между компонентами.
- **Lazy Initialization**: Механизм отложенной инициализации бинов, который позволяет создавать объекты только тогда, когда они действительно нужны.
- **Primary Bean**: Аннотация `@Primary`, которая указывает Spring, какой бин использовать по умолчанию, если есть несколько кандидатов одного типа.

### 6.2. Полезные ссылки
- [Официальная документация Spring](https://spring.io/docs)
- [IoC in Spring Framework](https://docs.spring.io/spring-framework/docs/current/reference/html/core.html#beans)

---

## Заключение
### 7.1. Итоги
IoC — это мощный инструмент для управления зависимостями, который позволяет создавать модульные и легко тестируемые приложения.

### 7.2. Что дальше?
- Изучите Spring Boot для быстрой разработки приложений.
- Ознакомьтесь с продвинутыми возможностями DI, такими как условное создание бинов (`@Conditional`).
