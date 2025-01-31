### **Hibernate Core**

---

#### 1. **Основы Hibernate**
- **Что такое Hibernate:**  
  - ORM (Object-Relational Mapping).  
  - Преимущества использования Hibernate.  
- **Установка и настройка:**  
  - Добавление зависимости в Gradle/Maven.  
  - Настройка `hibernate.cfg.xml` или `application.properties`.  

---

#### 2. **Основные концепции**
- **Session:**  
  - Что такое `Session` и как она работает.  
  - Методы `save`, `update`, `delete`, `get`, `load`.  
- **SessionFactory:**  
  - Роль и жизненный цикл.  
  - Настройка через `Configuration`.  
- **Transaction:**  
  - Управление транзакциями (`beginTransaction`, `commit`, `rollback`).  

---

#### 3. **Маппинг сущностей**
- **Аннотации для маппинга:**  
  - `@Entity`, `@Table`, `@Id`, `@GeneratedValue`.  
  - Поля: `@Column`, `@Transient`, `@Temporal`.  
- **Типы связей:**  
  - Один-к-одному (`@OneToOne`).  
  - Один-ко-многим (`@OneToMany`).  
  - Многие-к-одному (`@ManyToOne`).  
  - Многие-ко-многим (`@ManyToMany`).  
- **Fetch Strategies:**  
  - Ленивая (`LAZY`) и жадная (`EAGER`) загрузка.  
- **Каскадные операции:**  
  - `CascadeType` (ALL, PERSIST, MERGE, REMOVE).  

---

#### 4. **JPQL и HQL**
- **Основы JPQL:**  
  - Синтаксис и особенности.  
  - Разница между JPQL и SQL.  
- **Основные команды:**  
  - `SELECT`, `FROM`, `WHERE`.  
  - Агрегация (`COUNT`, `SUM`, `AVG`, `MAX`, `MIN`).  
  - Группировка (`GROUP BY`, `HAVING`).  
- **Named Queries:**  
  - Создание и использование `@NamedQuery`.  

---

#### 5. **Критерий API**
- **Что такое Criteria API:**  
  - Альтернатива JPQL для динамических запросов.  
- **Основные методы:**  
  - Создание запросов через `CriteriaBuilder` и `CriteriaQuery`.  
  - Условия (`Predicates`), сортировка (`Order`).  
- **Примеры:**  
  - Фильтрация, сортировка, агрегация.  

---

#### 6. **Кэширование**
- **Уровни кэша:**  
  - Первый уровень (`Session`-кэш).  
  - Второй уровень (`SessionFactory`-кэш).  
- **Провайдеры второго уровня:**  
  - EhCache, Redis, Hazelcast.  
- **Настройка кэширования:**  
  - Аннотации `@Cacheable`, конфигурация через `hibernate.cfg.xml`.  

---

#### 7. **Обработка транзакций**
- **Транзакции Hibernate:**  
  - Управление транзакциями через JPA.  
  - Аннотации: `@Transactional`.  
- **Уровни изоляции:**  
  - READ_UNCOMMITTED, READ_COMMITTED, REPEATABLE_READ, SERIALIZABLE.  
- **Оптимистичная и пессимистичная блокировка:**  
  - `@Version` для оптимистичной блокировки.  
  - Методы `lock` для пессимистичной блокировки.  

---

#### 8. **Работа с нативными SQL**
- **Native Queries:**  
  - Использование метода `createNativeQuery`.  
- **Результаты запросов:**  
  - Приведение к Entity или `ResultSet`.  
- **Примеры использования:**  
  - Обход ограничений JPQL.  

---

#### 9. **Обработка исключений**
- **HibernateException и его наследники:**  
  - `ConstraintViolationException`, `LazyInitializationException`.  
- **Обработка исключений:**  
  - Использование `try-catch` блоков.  
  - Примеры типичных ошибок и их решений.  

---

#### 10. **Миграции и обновления схемы**
- **Автоматическое создание схемы:**  
  - Настройки: `hibernate.hbm2ddl.auto`.  
- **Инструменты миграций:**  
  - Liquibase, Flyway.  
- **Практики управления изменениями схемы.**

---

#### 11. **Производительность и оптимизация**
- **Batch Processing:**  
  - Использование `batch_size` для оптимизации.  
- **N+1 проблема:**  
  - Причины и способы устранения (`JOIN FETCH`, `EntityGraph`).  
- **Мониторинг запросов:**  
  - Логирование через Hibernate (`show_sql`, `format_sql`).  
- **Пул соединений:**  
  - Использование HikariCP, настройка параметров пула.  

---

#### 12. **Расширенные возможности**
- **Custom User Types:**  
  - Создание собственных типов через `UserType`.  
- **Entity Listener:**  
  - События жизненного цикла сущностей (`@PrePersist`, `@PostUpdate`).  
- **Auditing:**  
  - Использование Envers для версионирования данных.  

---

#### 13. **Интеграция с Spring**
- **Spring Data JPA:**  
  - Роль `JpaRepository`.  
- **Конфигурация:**  
  - Настройка Entity Manager через Spring.  
  - Аннотация `@EnableJpaRepositories`.  
- **Использование транзакций:**  
  - `@Transactional` в контексте Spring.  
