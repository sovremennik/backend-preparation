#### [Основные методы Optional](#Основные-методы-optional-1)
#### [Работа с Optional](#Работа-с-optional-1)
#### [Примеры из реальной практики](#Примеры-из-реальной-практики-1)
#### [Продвинутые сценарии](#Продвинутые-сценарии-1)
#### [Под капотом](#Под-капотом-1)
#### [Практические советы](#Практические-советы-1)

---
# Optional в Java: Основы и примеры

`Optional` — это класс, упрощающий работу с возможным отсутствием значения. Он помогает избежать распространенных ошибок, таких как `NullPointerException`, и делает код более предсказуемым и читаемым.

## Основные методы Optional

### Создание Optional

1. **Создание пустого Optional:**

```java
Optional<String> emptyOptional = Optional.empty();
System.out.println(emptyOptional.isPresent()); // Output: false
```

2. **Создание Optional с значением:**

```java
Optional<Integer> optionalWithValue = Optional.of(42);
System.out.println(optionalWithValue.get()); // Output: 42
```

3. **Создание Optional, допускающего null:**

```java
Optional<String> optionalNullable = Optional.ofNullable(null);
System.out.println(optionalNullable.isPresent()); // Output: false
```

## Работа с Optional

### Проверка наличия значения

```java
Optional<Double> optionalValue = Optional.of(3.14);
if (optionalValue.isPresent()) {
    System.out.println("Value: " + optionalValue.get()); // Output: Value: 3.14
}
```

### Рекомендуемый способ: `ifPresent`

```java
optionalValue.ifPresent(value -> System.out.println("Value: " + value));
// Output: Value: 3.14
```

### Использование `orElse` и `orElseGet`

- **`orElse`**: Возвращает значение по умолчанию, если Optional пуст.

```java
Optional<String> optionalName = Optional.empty();
String name = optionalName.orElse("Default Name");
System.out.println(name); // Output: Default Name
```

- **`orElseGet`**: Использует функцию для вычисления значения по умолчанию.

```java
String nameWithFunction = optionalName.orElseGet(() -> "Generated Name");
System.out.println(nameWithFunction); // Output: Generated Name
```

### Исключения с `orElseThrow`

```java
Optional<Integer> emptyOptional = Optional.empty();
Integer result = emptyOptional.orElseThrow(() -> new IllegalStateException("No value present"));
// Исключение: IllegalStateException: No value present
```

## Примеры из реальной практики

### Чтение конфигурации

```java
public Optional<String> getConfigValue(String key) {
    Map<String, String> config = Map.of(
        "timeout", "5000",
        "endpoint", "https://api.example.com"
    );
    return Optional.ofNullable(config.get(key));
}

public void processConfiguration() {
    String timeout = getConfigValue("timeout").orElse("1000");
    System.out.println("Timeout: " + timeout); // Output: Timeout: 5000
}
```

### Поиск записи в базе данных

```java
public Optional<User> findUserById(int userId) {
    return users.stream()
                .filter(user -> user.getId() == userId)
                .findFirst();
}

public void processUser(int userId) {
    findUserById(userId)
        .ifPresentOrElse(
            user -> System.out.println("Found user: " + user.getName()),
            () -> System.out.println("User not found")
        );
}
```

**Output:**
- Если пользователь найден: `Found user: John`
- Если пользователь не найден: `User not found`

### Чтение файлов

```java
public Optional<String> readFile(String path) {
    try {
        return Optional.of(Files.readString(Path.of(path)));
    } catch (IOException e) {
        return Optional.empty();
    }
}

public void processFile(String path) {
    readFile(path).ifPresentOrElse(
        content -> System.out.println("File content: \n" + content),
        () -> System.out.println("File not found")
    );
}
```

**Output:**
- Если файл найден: `File content: ...`
- Если файл отсутствует: `File not found`

## Продвинутые сценарии

### Преобразование значения

```java
Optional<String> optionalName = Optional.of("John Doe");
Optional<Integer> nameLength = optionalName.map(String::length);
System.out.println("Name length: " + nameLength.orElse(0)); // Output: Name length: 8
```

### Вложенные Optional

При работе с вложенными Optional лучше использовать `flatMap`:

```java
public Optional<Address> getAddress(User user) {
    return Optional.ofNullable(user.getAddress());
}

public Optional<String> getCity(User user) {
    return getAddress(user).flatMap(Address::getCity);
}
```

### Чейнинг операций

```java
Optional<String> finalResult = Optional.ofNullable(fetchData())
    .filter(data -> !data.isEmpty())
    .map(String::toUpperCase);

System.out.println(finalResult.orElse("No data available"));
```

**Output:**
- Если данные пусты: `No data available`
- Если данные существуют: результат преобразуется в верхний регистр.

## Под капотом

1. `Optional` использует приватное финальное поле для хранения значения. Если значение отсутствует, это поле равно `null`.
2. Методы, такие как `orElse` и `map`, реализованы с минимальными накладными расходами для производительности.
3. `Optional` не является сериализуемым, поэтому его не следует использовать как поле класса.

## Практические советы

1. **Используйте Optional для методов, где значение может отсутствовать.**
2. **Не используйте Optional для полей класса или параметров метода.** Это увеличивает сложность.
3. **Избегайте методов `.get()`.** Используйте `orElse`, `ifPresent` или `orElseThrow` для обработки значений.
4. **Оптимизируйте вложенные Optional с помощью `flatMap`.**

`Optional` — это мощный инструмент для упрощения работы с отсутствующими значениями. Его правильное применение позволяет писать более безопасный и предсказуемый код.

