### **Spring Core**

---

#### **1. Введение в Spring Framework**
- **Что такое Spring:**
  - Основные возможности и философия.
  - Почему Spring: DI, AOP, масштабируемость.
- **Архитектура Spring:**
  - Основные модули: Core, Data, Web, Security.
  - Понятие контекста приложений (`ApplicationContext`).
- **Подходы к конфигурации:**
  - XML-конфигурация.
  - Аннотации.
  - Java Config (класс `@Configuration`).

---

#### **2. Инверсия управления (IoC)**
- **Понятие IoC и DI (Dependency Injection).**
- **Контейнеры Spring:**
  - `BeanFactory` vs `ApplicationContext`.
- **Определение бинов:**
  - Аннотации: `@Component`, `@Service`, `@Repository`.
  - XML-конфигурация.
- **Внедрение зависимостей:**
  - Через конструктор, сеттеры, поля.
  - Аннотации: `@Autowired`, `@Qualifier`, `@Primary`.

---

#### **3. Жизненный цикл бинов**
- **Определение и инициализация бинов:**
  - Методы `@PostConstruct` и `@PreDestroy`.
  - Интерфейсы `InitializingBean` и `DisposableBean`.
- **Скоупы бинов:**
  - `singleton`, `prototype`, `request`, `session`.
  - Конфигурация скоупов через `@Scope`.

---

#### **4. Aspect-Oriented Programming (AOP)**
- **Основы AOP:**
  - Что такое AOP, понятие аспектов.
  - Основные понятия: Advice, Pointcut, JoinPoint, Weaving.
- **Аннотации:**
  - `@Aspect`, `@Before`, `@After`, `@Around`.
- **Реализация аспектов:**
  - XML или Java Config.

---

### **Spring Boot**

---

#### **1. Введение в Spring Boot**
- **Отличия Spring и Spring Boot.**
- **Преимущества Spring Boot:**
  - Автоконфигурация.
  - Встроенные серверы (Tomcat, Jetty).
- **Структура Spring Boot-приложения:**
  - `@SpringBootApplication`.
  - Файл `application.yml` или `application.properties`.

---

#### **2. Автоконфигурация и контекст**
- **Что такое автоконфигурация:**
  - Механизм Spring Boot.
- **Работа с профилями:**
  - `@Profile`.
  - Указание активного профиля.

---

#### **3. Работа с зависимостями**
- **Использование Maven/Gradle:**
  - Starter зависимости: `spring-boot-starter-web`, `spring-boot-starter-data-jpa`.
- **Управление версиями через Spring Boot BOM.**

---

#### **4. Логи и мониторинг**
- **Логгирование:**
  - Подключение Logback/Log4j.
  - Настройка уровней логов.
- **Spring Actuator:**
  - Мониторинг метрик и статуса приложения.

---

### **Spring MVC**

---

#### **1. Основы Spring MVC**
- **Архитектура MVC в Spring:**
  - Model, View, Controller.
- **Аннотации:**
  - `@Controller`, `@RestController`.
  - `@RequestMapping`, `@GetMapping`, `@PostMapping`.
- **Работа с параметрами запросов:**
  - `@RequestParam`, `@PathVariable`.

---

#### **2. Работа с представлениями**
- **Поддерживаемые представления:**
  - Thymeleaf, JSP, FreeMarker.
- **Использование модели:**
  - `Model`, `ModelAndView`.

---

#### **3. Обработка ошибок**
- **Пользовательские обработчики ошибок:**
  - `@ControllerAdvice`, `@ExceptionHandler`.

---

#### **4. Валидация данных**
- **Аннотации для валидации:**
  - `@NotNull`, `@Size`, `@Pattern`.
- **Подключение валидации через `@Valid` и `BindingResult`.**

---

### **Spring Data**

---

#### **1. Работа с базами данных**
- **Подключение JPA и Hibernate через Spring Data.**
- **Репозитории:**
  - `JpaRepository`, `CrudRepository`.
  - Кастомные запросы с `@Query`.
- **Миграции базы данных:**
  - Использование Liquibase или Flyway.

---

#### **2. Работа с NoSQL**
- **Подключение MongoDB:**
  - Репозитории Mongo.
- **Использование Redis для кэширования.**

---

#### **3. Spring Transactions**
- **Управление транзакциями:**
  - Аннотация `@Transactional`.
  - Настройка уровней изоляции и откатов.

---

### **Spring Security**

---

#### **1. Основы Spring Security**
- **Настройка безопасности:**
  - Конфигурация через Java Config.
- **Аутентификация и авторизация:**
  - Настройка ролей и пользователей.

---

#### **2. OAuth2 и JWT**
- **Настройка OAuth2 для аутентификации.**
- **Использование JWT для токенов.**

---

#### **3. Безопасность REST API**
- **Защита API через токены.**
- **Кросс-доменная безопасность (CORS).**

---

### **Spring Integration и микросервисы**

---

#### **1. Работа с Apache Kafka**
- **Интеграция через Spring Kafka.**
- **Продюсеры и консьюмеры.**

---

#### **2. Микросервисы с Spring Cloud**
- **Основные модули Spring Cloud:**
  - Eureka, Config Server, Gateway.
- **Балансировка нагрузки и устойчивость.**

---

#### **3. Обмен сообщениями**
- **Интеграция с RabbitMQ или Kafka.**
- **Использование Spring Messaging.**

---
📁 **spring-framework/**
│
├── 📄 **spring.md**                       → Корневой файл документации
│
├── 📁 **core/**                           → Spring Core
│   ├── 📄 core.md                         → Обзор
│   ├── 📁 **introduction/**               → 1. Введение
│   │   └── 📄 introduction.md
│   ├── 📁 **ioc/**                        → 2. IoC
│   │   └── 📄 ioc.md
│   ├── 📁 **bean-lifecycle/**             → 3. Жизненный цикл
│   │   └── 📄 bean-lifecycle.md
│   └── 📁 **aop/**                        → 4. AOP
│       └── 📄 aop.md
│
├── 📁 **boot/**                           → Spring Boot
│   ├── 📄 boot.md                         → Обзор
│   ├── 📁 **introduction/**               → 1. Введение
│   │   └── 📄 introduction.md
│   ├── 📁 **autoconfig/**                 → 2. Автоконфигурация
│   │   └── 📄 autoconfig.md
│   ├── 📁 **dependencies/**               → 3. Зависимости
│   │   └── 📄 dependencies.md
│   └── 📁 **logging/**                    → 4. Логи и мониторинг
│       └── 📄 logging.md
│
├── 📁 **mvc/**                            → Spring MVC
│   ├── 📄 mvc.md                          → Обзор
│   ├── 📁 **basics/**                     → 1. Основы
│   │   └── 📄 basics.md
│   ├── 📁 **views/**                      → 2. Представления
│   │   └── 📄 views.md
│   ├── 📁 **error-handling/**             → 3. Обработка ошибок
│   │   └── 📄 error-handling.md
│   └── 📁 **validation/**                 → 4. Валидация
│       └── 📄 validation.md
│
├── 📁 **data/**                           → Spring Data
│   ├── 📄 data.md                         → Обзор
│   ├── 📁 **jpa/**                        → 1. JPA и Hibernate
│   │   └── 📄 jpa.md
│   ├── 📁 **nosql/**                      → 2. NoSQL
│   │   └── 📄 nosql.md
│   └── 📁 **transactions/**               → 3. Транзакции
│       └── 📄 transactions.md
│
├── 📁 **security/**                       → Spring Security
│   ├── 📄 security.md                     → Обзор
│   ├── 📁 **basics/**                     → 1. Основы
│   │   └── 📄 basics.md
│   ├── 📁 **oauth-jwt/**                  → 2. OAuth2/JWT
│   │   └── 📄 oauth-jwt.md
│   └── 📁 **rest-security/**              → 3. REST API
│       └── 📄 rest-security.md
│
└── 📁 **integration/**                    → Интеграция
    ├── 📄 integration.md                  → Обзор
    ├── 📁 **kafka/**                      → 1. Kafka
    │   └── 📄 kafka.md
    ├── 📁 **cloud/**                      → 2. Spring Cloud
    │   └── 📄 cloud.md
    └── 📁 **messaging/**                  → 3. Сообщения
        └── 📄 messaging.md