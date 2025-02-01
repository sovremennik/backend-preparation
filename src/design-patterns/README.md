# Паттерны проектирования (Design Patterns)

## 1. Введение в паттерны проектирования
### 1.1. Что такое паттерны проектирования?
- Повторяемые решения типичных задач проектирования  
- Повышение читаемости и гибкости кода  

### 1.2. Зачем нужны паттерны?
- Упрощение проектирования сложных систем  
- Повышение масштабируемости и сопровождаемости  
- Переиспользование проверенных решений  

### 1.3. История и классификация
- Книга "Design Patterns: Elements of Reusable Object-Oriented Software" (GoF, 1994)  
- Три группы паттернов: порождающие, структурные, поведенческие  

### 1.4. Принципы, лежащие в основе паттернов
- **SOLID**  
- **DRY** (Don't Repeat Yourself)  
- **KISS** (Keep It Simple, Stupid)  
- **YAGNI** (You Aren’t Gonna Need It)  

### 1.5. Когда использовать паттерны?
- Решение типичных архитектурных задач  
- Баланс между гибкостью и сложностью  
- Недопустимость избыточного использования (антипаттерны)  

## 2. Порождающие паттерны (Creational Patterns)
### 2.1. Singleton (Одиночка)  
- Цель: Гарантирует существование единственного экземпляра класса  
- Примеры реализации (ленивая, жадная, потокобезопасная)  
- Примеры в Java/Kotlin  

### 2.2. Factory Method (Фабричный метод)  
- Цель: Делегирует создание объектов подклассам  
- Примеры реализации  

### 2.3. Abstract Factory (Абстрактная фабрика)  
- Цель: Создание семейств связанных объектов без указания их конкретных классов  
- Примеры реализации  

### 2.4. Builder (Строитель)  
- Цель: Пошаговое создание сложных объектов  
- Fluent интерфейс  

### 2.5. Prototype (Прототип)  
- Цель: Клонирование объектов без привязки к их классам  
- Поверхностное и глубокое копирование  

## 3. Структурные паттерны (Structural Patterns)
### 3.1. Adapter (Адаптер)  
- Цель: Совместимость несовместимых интерфейсов  
- Объектный и классический адаптер  

### 3.2. Bridge (Мост)  
- Цель: Разделение абстракции и реализации  
- Пример: DAO слой  

### 3.3. Composite (Компоновщик)  
- Цель: Работа с деревьями объектов как с единым целым  
- Примеры: UI-компоненты  

### 3.4. Decorator (Декоратор)  
- Цель: Динамическое добавление функционала объекту  
- Отличие от наследования  

### 3.5. Facade (Фасад)  
- Цель: Упрощение взаимодействия с подсистемами  
- Примеры в Spring  

### 3.6. Flyweight (Легковес)  
- Цель: Экономия памяти при большом количестве схожих объектов  
- Пример: кэширование объектов  

### 3.7. Proxy (Заместитель)  
- Цель: Управление доступом к объекту  
- Типы: виртуальный, защитный, удалённый  

## 4. Поведенческие паттерны (Behavioral Patterns)
### 4.1. Chain of Responsibility (Цепочка обязанностей)  
- Цель: Передача запроса по цепочке обработчиков  
- Примеры: фильтры в веб-сервисах  

### 4.2. Command (Команда)  
- Цель: Инкапсуляция запроса в объект  
- Отмена операций  

### 4.3. Interpreter (Интерпретатор)  
- Цель: Определение грамматики и интерпретация предложений языка  

### 4.4. Iterator (Итератор)  
- Цель: Последовательный доступ к элементам коллекции  

### 4.5. Mediator (Посредник)  
- Цель: Упрощение взаимодействия между объектами  
- Пример: GUI-компоненты  

### 4.6. Memento (Снимок)  
- Цель: Сохранение и восстановление состояния объекта  

### 4.7. Observer (Наблюдатель)  
- Цель: Автоматическое оповещение об изменениях состояния  
- Примеры: событийные системы  

### 4.8. State (Состояние)  
- Цель: Изменение поведения объекта в зависимости от его состояния  

### 4.9. Strategy (Стратегия)  
- Цель: Выбор алгоритма во время выполнения  
- Примеры: сортировки, аутентификация  

### 4.10. Template Method (Шаблонный метод)  
- Цель: Определение структуры алгоритма с возможностью переопределения шагов  

### 4.11. Visitor (Посетитель)  
- Цель: Добавление операций к объектам без изменения их классов  

## 5. Принципы SOLID и их связь с паттернами
- **S**: Single Responsibility Principle (Decorator, Strategy)  
- **O**: Open/Closed Principle (Strategy, Observer)  
- **L**: Liskov Substitution Principle  
- **I**: Interface Segregation Principle  
- **D**: Dependency Inversion Principle (Factory Method, Singleton)  

## 6. Антипаттерны (Anti-patterns)
- Спагетти-код (Spaghetti Code)  
- Золотой молоток (Golden Hammer)  
- Ленивая загрузка (Lazy Loading) — злоупотребление  

## 7. Практическое применение паттернов
- Паттерны в Spring Framework  
- Паттерны в Hibernate  
- Реализация через Java/Kotlin  

## 8. Полезные ссылки и ресурсы
- **Книги:**  
  - *Design Patterns: Elements of Reusable Object-Oriented Software* — Erich Gamma, Richard Helm, Ralph Johnson, John Vlissides (GoF)  
  - *Head First Design Patterns* — Eric Freeman, Elisabeth Robson  
  - *Design Patterns Explained: A New Perspective on Object-Oriented Design* — Alan Shalloway, James R. Trott  
- **Онлайн-курсы:**  
  - Udemy, Coursera, Pluralsight  
- **Платформы для практики:**  
  - LeetCode, HackerRank, Refactoring.Guru  

---
📁 design-patterns/
├── 📄 design-patterns.md
├── 📁 1-introduction/
│   ├── 📄 1-introduction.md
│   ├── 📁 what-are-design-patterns/
│   │   └── 📄 what-are-design-patterns.md
│   ├── 📁 principles/
│   │   └── 📄 principles.md
│   ├── 📁 history-and-classification/
│   │   └── 📄 history-and-classification.md
│   └── 📁 when-to-use-patterns/
│       └── 📄 when-to-use-patterns.md
├── 📁 2-creational-patterns/
│   ├── 📄 2-creational-patterns.md
│   ├── 📁 singleton/
│   │   └── 📄 singleton.md
│   ├── 📁 factory-method/
│   │   └── 📄 factory-method.md
│   ├── 📁 abstract-factory/
│   │   └── 📄 abstract-factory.md
│   ├── 📁 builder/
│   │   └── 📄 builder.md
│   └── 📁 prototype/
│       └── 📄 prototype.md
├── 📁 3-structural-patterns/
│   ├── 📄 3-structural-patterns.md
│   ├── 📁 adapter/
│   │   └── 📄 adapter.md
│   ├── 📁 bridge/
│   │   └── 📄 bridge.md
│   ├── 📁 composite/
│   │   └── 📄 composite.md
│   ├── 📁 decorator/
│   │   └── 📄 decorator.md
│   ├── 📁 facade/
│   │   └── 📄 facade.md
│   ├── 📁 flyweight/
│   │   └── 📄 flyweight.md
│   └── 📁 proxy/
│       └── 📄 proxy.md
├── 📁 4-behavioral-patterns/
│   ├── 📄 4-behavioral-patterns.md
│   ├── 📁 chain-of-responsibility/
│   │   └── 📄 chain-of-responsibility.md
│   ├── 📁 command/
│   │   └── 📄 command.md
│   ├── 📁 interpreter/
│   │   └── 📄 interpreter.md
│   ├── 📁 iterator/
│   │   └── 📄 iterator.md
│   ├── 📁 mediator/
│   │   └── 📄 mediator.md
│   ├── 📁 memento/
│   │   └── 📄 memento.md
│   ├── 📁 observer/
│   │   └── 📄 observer.md
│   ├── 📁 state/
│   │   └── 📄 state.md
│   ├── 📁 strategy/
│   │   └── 📄 strategy.md
│   ├── 📁 template-method/
│   │   └── 📄 template-method.md
│   └── 📁 visitor/
│       └── 📄 visitor.md
├── 📁 5-solid-principles/
│   ├── 📄 5-solid-principles.md
│   ├── 📁 single-responsibility/
│   │   └── 📄 single-responsibility.md
│   ├── 📁 open-closed/
│   │   └── 📄 open-closed.md
│   ├── 📁 liskov-substitution/
│   │   └── 📄 liskov-substitution.md
│   ├── 📁 interface-segregation/
│   │   └── 📄 interface-segregation.md
│   └── 📁 dependency-inversion/
│       └── 📄 dependency-inversion.md
├── 📁 6-anti-patterns/
│   ├── 📄 6-anti-patterns.md
│   ├── 📁 spaghetti-code/
│   │   └── 📄 spaghetti-code.md
│   ├── 📁 golden-hammer/
│   │   └── 📄 golden-hammer.md
│   └── 📁 lazy-loading/
│       └── 📄 lazy-loading.md
├── 📁 7-practical-usage/
│   ├── 📄 7-practical-usage.md
│   ├── 📁 spring-framework/
│   │   └── 📄 spring-framework.md
│   ├── 📁 hibernate/
│   │   └── 📄 hibernate.md
│   └── 📁 java-kotlin-examples/
│       └── 📄 java-kotlin-examples.md
└── 📁 8-resources/
    ├── 📄 8-resources.md
    ├── 📁 books/
    │   └── 📄 books.md
    ├── 📁 online-courses/
    │   └── 📄 online-courses.md
    └── 📁 practice-platforms/
        └── 📄 practice-platforms.md