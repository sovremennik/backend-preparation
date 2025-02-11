#### [Java 18: Кодировки и упрощение работы с текстами](#java-18-Кодировки-и-упрощение-работы-с-текстами-1)
#### [Java 19: Улучшения для масштабируемости и удобства](#java-19-Улучшения-для-масштабируемости-и-удобства-1)
#### [Java 20: Оптимизация шаблонов и многопоточности](#java-20-Оптимизация-шаблонов-и-многопоточности-1)
#### [Java 21: Новые возможности](#java-21-Новые-возможности-1)


---
# Изменения в языке и синтаксисе (Java 18-23)

Java активно эволюционирует, предлагая обновления, которые значительно повышают удобство работы, читаемость кода и производительность. В версиях Java 18-23 добавлены ключевые изменения, которые влияют как на повседневное программирование, так и на создание сложных систем. Ниже представлены наиболее важные нововведения с теоретическим обоснованием и прикладными примерами.

---

## Java 18: Кодировки и упрощение работы с текстами

### 1. `Charset.defaultCharset()`

Эта функция улучшает работу с текстовыми кодировками, предоставляя платформо-независимый стандарт. Это важно для приложений с международной аудиторией, где требуется обработка файлов и строк с различными локалями.

#### Пример использования:

```java
Charset defaultCharset = Charset.defaultCharset();
System.out.println("Default Charset: " + defaultCharset.name());
```

Эта функция полезна для анализа системных настроек или логирования в многоплатформенных приложениях.

### 2. Simple Web Server (JEP 408):

Позволяет разработчикам запускать встроенный HTTP-сервер для статического контента, исключая необходимость использования сторонних инструментов для локальной отладки.

#### Пример использования:

```bash
jwebserver -p 8080
```

После запуска сервера, статические ресурсы доступны по адресу [http://localhost:8080](http://localhost:8080). Это особенно полезно для быстрой интеграции и тестирования фронтенд-компонентов.

---

## Java 19: Улучшения для масштабируемости и удобства

### 1. Project Loom (Virtual Threads):

Virtual Threads (виртуальные потоки) — это революционное обновление, позволяющее создавать тысячи потоков с минимальными накладными расходами.

#### Пример использования:

```java
try (var executor = Executors.newVirtualThreadPerTaskExecutor()) {
    IntStream.range(0, 10).forEach(i ->
        executor.submit(() -> System.out.println("Task " + i))
    );
}
```

Эта технология позволяет обрабатывать тысячи параллельных запросов без значительного увеличения потребления ресурсов, что критически важно для высоконагруженных систем.

### 2. Record Patterns (JEP 405):

Расширяет использование `record` для сопоставления данных, что упрощает разбор сложных структур и делает код декларативным.

#### Пример использования:

```java
record Point(int x, int y) {}

static String processPoint(Object obj) {
    return switch (obj) {
        case Point(int x, int y) when x == y -> "Diagonal point";
        case Point(int x, int y) -> "Point(" + x + ", " + y + ")";
        default -> "Unknown";
    };
}
```

---

## Java 20: Оптимизация шаблонов и многопоточности

### 1. Pattern Matching для `switch` (JEP 433):

Расширяет возможности сопоставления с шаблоном, делая `switch` мощным инструментом для обработки данных разных типов.

#### Пример использования:

```java
static void process(Object obj) {
    switch (obj) {
        case String s -> System.out.println("String: " + s);
        case Integer i -> System.out.println("Integer: " + i);
        default -> System.out.println("Unknown type");
    }
}
```

Эта возможность особенно полезна при создании обработчиков событий и API.

### 2. Scoped Values (JEP 429):

Scoped Values предлагают альтернативу `ThreadLocal`, позволяя передавать неизменяемые данные между потоками более эффективно.

---

## Java 21: Новые возможности

### 1. Record Enhancements:

Включение новых возможностей для `record`, включая поддержку нестандартных модификаторов полей.

#### Пример использования:

```java
record Customer(String name, int id) {}

Customer customer = new Customer("John", 123);
System.out.println(customer);
```

Эти улучшения находят широкое применение в DTO-объектах для передачи данных между слоями приложения.

### 2. String Templates (JEP 430):

Предварительная функция для создания строковых шаблонов, упрощающая сложные операции форматирования.

#### Пример использования:

```java
String template = STR."Customer name: \{customer.name}, ID: \{customer.id}";
```

Это значительно улучшает читаемость кода и сокращает использование устаревших методов форматирования.

