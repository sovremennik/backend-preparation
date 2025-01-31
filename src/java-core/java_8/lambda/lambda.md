#### [Основы Lambda-выражений](#Основы-lambda-выражений-1)
#### [Применение Lambda-выражений](#Применение-lambda-выражений-1)
#### [Ограничения и особенности](#Ограничения-и-особенности-1)
#### [Продвинутые примеры](#Продвинутые-примеры-1)

---
# Lambda-выражения в Java

Lambda-выражения (или лямбды) были введены в Java 8 и стали ключевым инструментом для работы с функциональными интерфейсами и парадигмой функционального программирования.

Лямбды сокращают код, упрощают объявление анонимных классов и улучшают читаемость, что делает их особенно полезными для продакшн-разработки.


## Основы Lambda-выражений

### Формат записи

```
(параметры) -> тело лямбды
```

- **Параметры:** Могут отсутствовать (если параметров нет), один параметр не требует скобок.
- **Тело:** Одно выражение не требует фигурных скобок. Для нескольких выражений используется блок фигурных скобок `{}`.

### Примеры

#### Однострочное выражение:

```java
Function<Integer, Integer> square = x -> x * x;
System.out.println(square.apply(5)); // Вывод: 25
```

#### Многострочное выражение:

```java
Consumer<String> printer = message -> {
    System.out.println("Logging message:");
    System.out.println(message);
};
printer.accept("Запуск сервиса");
```

## Применение Lambda-выражений

### 1. **Функциональные интерфейсы**

Lambda-выражения используются с функциональными интерфейсами — интерфейсами, у которых есть только один абстрактный метод. Примеры из стандартной библиотеки Java:

- `Function<T, R>`: Преобразование данных из типа `T` в тип `R`.
- `Consumer<T>`: Выполняет действие над объектом типа `T`.
- `Supplier<T>`: Поставляет объект типа `T`.
- `Predicate<T>`: Проверяет условие для объекта типа `T`.

### 2. **Реальные примеры**

#### Пример 1: Фильтрация списка (Predicate)

Вместо ручного перебора:

```java
List<String> servers = List.of("server1", "test-server", "prod-server");

// Оставляем только тестовые серверы
List<String> filtered = servers.stream()
                               .filter(name -> name.contains("test"))
                               .toList();
System.out.println(filtered); // ["test-server"]
```

#### Пример 2: Преобразование данных (Function)

```java
List<Integer> ports = List.of(8080, 9090, 4040);

// Добавляем префикс к каждому порту
List<String> portDescriptions = ports.stream()
    .map(port -> "Порт: " + port)
    .toList();

portDescriptions.forEach(System.out::println);
// Вывод:
// Порт: 8080
// Порт: 9090
// Порт: 4040
```

#### Пример 3: Обработка исключений внутри лямбд

Если ваш функциональный код может выбрасывать исключения:

```java
List<String> files = List.of("config.yml", "data.json");

files.forEach(file -> {
    try {
        Files.readString(Path.of(file));
        System.out.println("Файл обработан: " + file);
    } catch (IOException e) {
        System.err.println("Ошибка чтения файла: " + file);
    }
});
```

## Ограничения и особенности

### 1. **Типы параметров**

- Типы параметров можно указывать явно:

```java
BiFunction<Integer, Integer, Integer> adder = (Integer x, Integer y) -> x + y;
```

- Либо использовать выведение типов (type inference):

```java
BiFunction<Integer, Integer, Integer> adder = (x, y) -> x + y;
```

### 2. **Использование переменных**

- Лямбды могут использовать **эффективно финальные** переменные из внешнего контекста:

```java
String prefix = "User: ";
Consumer<String> greeter = name -> System.out.println(prefix + name);
greeter.accept("Alex");
```

Если переменная изменяется после создания лямбды, это вызовет ошибку компиляции.

### 3. **Отсутствие контекста this**

Внутри лямбды ключевое слово `this` ссылается на внешний класс, а не на тело самой лямбды. Это отличие от анонимных классов.

## Продвинутые примеры

### 1. **Лямбда с кастомным функциональным интерфейсом**

#### Интерфейс:

```java
@FunctionalInterface
public interface Retryable {
    boolean executeWithRetry(int attempts);
}
```

#### Использование:

```java
Retryable retryable = attempts -> {
    for (int i = 0; i < attempts; i++) {
        System.out.println("Попытка " + (i + 1));
        if (Math.random() > 0.7) {
            return true; // Успешно
        }
    }
    return false; // Все попытки не удались
};

boolean success = retryable.executeWithRetry(5);
System.out.println("Результат: " + (success ? "Успех" : "Неудача"));
```

### 2. **Комбинирование функций**

```java
Function<String, String> addBrackets = s -> "[" + s + "]";
Function<String, String> toUpperCase = String::toUpperCase;

Function<String, String> combined = addBrackets.andThen(toUpperCase);
System.out.println(combined.apply("test")); // [TEST]
```
