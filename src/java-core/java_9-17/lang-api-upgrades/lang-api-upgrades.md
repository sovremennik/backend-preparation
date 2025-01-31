#### [Основные улучшения языка](#Основные-улучшения-языка-1)
#### [Основные улучшения API](#Основные-улучшения-api-1)
#### [Символьная иллюстрация изменений](#Символьная-иллюстрация-изменений-1)

---
# Улучшения языка и API (Java 9-17)

В версиях Java с 9 по 17 было представлено множество улучшений, как на уровне языка, так и API, которые помогают разработчикам писать код быстрее, безопаснее и удобнее. В данном конспекте представлены основные изменения, примеры их применения и схемы, иллюстрирующие работу новых возможностей.

---

## Основные улучшения языка

### 1. `var`: Локальная типизация (Java 10)
Локальная типизация позволяет использовать ключевое слово `var` для объявления переменных, чьё значение компилятор может вывести автоматически. Это упрощает код, сохраняя при этом строгую типизацию.

#### Пример:
```java
// До Java 10
Map<String, List<Integer>> data = new HashMap<>();

// С Java 10
var data = new HashMap<String, List<Integer>>();

// Компилятор выводит тип как Map<String, List<Integer>>
data.put("numbers", List.of(1, 2, 3));
```

### 2. Текстовые блоки (Java 13)
Текстовые блоки позволяют писать многострочные строки без использования громоздких символов переноса или экранирования.

#### Пример:
```java
// До Java 13
String json = "{\n" +
              "  \"name\": \"John\",\n" +
              "  \"age\": 30\n" +
              "}";

// С Java 13
String json = """
{
  "name": "John",
  "age": 30
}
""";
```

### 3. Записи (Records) (Java 14)
Записи представляют собой компактный способ создания неизменяемых классов с минимальным количеством шаблонного кода.

#### Пример:
```java
// До Java 14
public class Point {
    private final int x;
    private final int y;

    public Point(int x, int y) {
        this.x = x;
        this.y = y;
    }

    public int getX() {
        return x;
    }

    public int getY() {
        return y;
    }
}

// С Java 14
public record Point(int x, int y) {}

// Использование
var point = new Point(10, 20);
System.out.println(point.x()); // 10
System.out.println(point);    // Point[x=10, y=20]
```

### 4. Switch-выражения (Java 14, окончательная версия в Java 15)
Обновлённый `switch` позволяет возвращать значения и упрощает работу с логикой ветвлений.

#### Пример:
```java
// До Java 14
int day = 2;
String dayName;
switch (day) {
    case 1: dayName = "Monday"; break;
    case 2: dayName = "Tuesday"; break;
    default: dayName = "Unknown";
}

// С Java 14
String dayName = switch (day) {
    case 1 -> "Monday";
    case 2 -> "Tuesday";
    default -> "Unknown";
};
```

---

## Основные улучшения API

### 1. Улучшения коллекций
- **`List.of`, `Set.of`, `Map.of` (Java 9)**: Создание неизменяемых коллекций.
- **`Collectors.teeing` (Java 12)**: Комбинация результатов двух коллекций в один.

#### Пример:
```java
// Создание неизменяемой коллекции
var list = List.of("A", "B", "C");

// Teeing: суммирование и нахождение среднего
var stats = List.of(1, 2, 3, 4, 5).stream()
    .collect(Collectors.teeing(
        Collectors.summingInt(Integer::intValue),
        Collectors.counting(),
        (sum, count) -> "Sum: " + sum + ", Avg: " + (sum / count)
    ));
System.out.println(stats); // Sum: 15, Avg: 3
```

### 2. Работа с файлами
- **`Files.readString` и `Files.writeString` (Java 11)**: Упрощённое чтение и запись файлов.
- **`Path.of` (Java 11)**: Удобное создание объектов пути.

#### Пример:
```java
// Запись в файл
var path = Path.of("example.txt");
Files.writeString(path, "Hello, Java 11!");

// Чтение из файла
String content = Files.readString(path);
System.out.println(content); // Hello, Java 11!
```

### 3. Новые методы Stream API
- **`takeWhile`, `dropWhile` (Java 9)**: Управление элементами потока в зависимости от условий.
- **`iterate` с предикатом (Java 9)**: Более гибкая итерация.

#### Пример:
```java
// takeWhile и dropWhile
var numbers = List.of(1, 2, 3, 4, 5, 6);
numbers.stream().takeWhile(n -> n < 4).forEach(System.out::println); // 1, 2, 3
numbers.stream().dropWhile(n -> n < 4).forEach(System.out::println); // 4, 5, 6

// Итерация
Stream.iterate(1, n -> n < 10, n -> n * 2).forEach(System.out::println);
// 1, 2, 4, 8
```

---

## Символьная иллюстрация изменений

```
+-------------------------+
| Java 9                 |
| - List.of / Set.of     |
| - Stream.takeWhile     |
+-------------------------+
         |
         v
+-------------------------+
| Java 10                |
| - var                  |
+-------------------------+
         |
         v
+-------------------------+
| Java 11                |
| - Files.readString     |
| - HttpClient           |
+-------------------------+
         |
         v
+-------------------------+
| Java 13-14             |
| - Текстовые блоки      |
| - Switch-выражения     |
| - Записи (Records)     |
+-------------------------+
```

