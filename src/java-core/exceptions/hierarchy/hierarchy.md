#### [Основные понятия](#Основные-понятия-1)
#### [Класс `Throwable`](#Класс-throwable-1)
#### [Класс `Error`](#Класс-error-1)
#### [Класс `Exception`](#Класс-exception-1)
#### [Разница между Checked и Unchecked исключениями](#Разница-между-checked-и-unchecked-исключениями-1)
#### [Реальные примеры использования](#Реальные-примеры-использования-1)
#### [Заключение](#Заключение-1)

---
# Иерархия исключений

## Основные понятия

Исключения в Java представляют собой механизм обработки ошибок, возникающих во время выполнения программы. Все исключения являются объектами классов, которые наследуются от базового класса `Throwable`. Полная иерархия выглядит следующим образом:

```
Throwable
|
|-- Error
|   |-- StackOverflowError
|   |-- OutOfMemoryError
|   \-- ...
|
\-- Exception
    |-- RuntimeException
    |   |-- NullPointerException
    |   |-- ArrayIndexOutOfBoundsException
    |   |-- IllegalArgumentException
    |   \-- ...
    |
    \-- Checked Exceptions
        |-- IOException
        |-- SQLException
        \-- ...
```

---

## Класс `Throwable`

`Throwable` — это базовый класс для всех ошибок и исключений в Java. Он предоставляет основные методы для работы с исключениями, такие как:

- **`getMessage()`**: Возвращает сообщение, связанное с исключением.
- **`printStackTrace()`**: Выводит трассировку стека исключения.
- **`getCause()`**: Возвращает причину исключения, если она была указана.

Класс `Throwable` имеет два ключевых подкласса:

1. `Error`: Представляет ошибки уровня JVM.
2. `Exception`: Представляет ошибки, которые можно обрабатывать в коде.

---

## Класс `Error`

Класс `Error` используется для представления серьёзных проблем, возникающих на уровне JVM, которые обычно невозможно исправить в пользовательском коде. Такие ошибки сигнализируют о сбоях, требующих либо изменений в конфигурации, либо значительных исправлений в коде.

### Примеры ошибок:

1. **`StackOverflowError`**:
    - Возникает, когда стек вызовов переполняется из-за чрезмерного количества рекурсивных вызовов.
    - Указывает на проблему в логике программы, например, отсутствие условия завершения рекурсии.

2. **`OutOfMemoryError`**:
    - Возникает, когда JVM не может выделить достаточно памяти для новых объектов.
    - Может быть результатом утечки памяти или неправильной конфигурации JVM (например, недостаточного размера кучи).

### Особенности класса `Error`:

- Ошибки из этого класса обычно не обрабатываются, так как их устранение требует модификации архитектуры или конфигурации приложения.
- В продакшене ошибки, связанные с памятью (`OutOfMemoryError`), могут указывать на необходимость оптимизации использования ресурсов.

Пример различия между `StackOverflowError` и `OutOfMemoryError`:

| Ошибка                | Причина                                                     | Пример                                                                                  |
|-----------------------|------------------------------------------------------------|-----------------------------------------------------------------------------------------|
| `StackOverflowError`  | Переполнение стека вызовов.                                | Бесконечная рекурсия: метод вызывает сам себя без условий завершения.                   |
| `OutOfMemoryError`    | JVM исчерпала доступную память для объектов.               | Создание большого количества объектов, например, добавление в список без ограничения.   |


---

## Класс `Exception`

### Подкатегории

1. **Checked Exceptions**
   - Подлежат обязательной обработке с помощью `try-catch` или декларации через `throws`.
   - Примеры: `IOException`, `SQLException`.

2. **Unchecked Exceptions (RuntimeException)**
   - Не требуют обязательной обработки.
   - Возникают из-за ошибок логики программы.
   - Примеры: `NullPointerException`, `IndexOutOfBoundsException`.

---

## Разница между Checked и Unchecked исключениями

### Checked Exceptions

- Контролируются компилятором.
- Применяются для предсказуемых ошибок, например, при работе с файлами или базами данных.

Пример:

```java
import java.io.*;

public class FileExample {
    public static void main(String[] args) {
        try {
            BufferedReader reader = new BufferedReader(new FileReader("data.txt"));
            System.out.println(reader.readLine());
            reader.close();
        } catch (IOException e) {
            System.err.println("Ошибка чтения файла: " + e.getMessage());
        }
    }
}
```

### Unchecked Exceptions

- Не контролируются компилятором.
- Чаще всего указывают на ошибки в логике программы.

Пример:

```java
public class UncheckedExample {
    public static void main(String[] args) {
        int[] numbers = {1, 2, 3};

        try {
            System.out.println(numbers[5]); // ArrayIndexOutOfBoundsException
        } catch (ArrayIndexOutOfBoundsException e) {
            System.err.println("Ошибка доступа к элементу массива: " + e.getMessage());
        }
    }
}
```

---

## Реальные примеры использования

### Проверка входных данных (Checked Exceptions)

При взаимодействии с базой данных или внешними API часто используются Checked Exceptions:

```java
import java.sql.*;

public class DatabaseExample {
    public void fetchData() throws SQLException {
        String url = "jdbc:mysql://localhost:3306/app";
        String username = "user";
        String password = "password";

        try (Connection connection = DriverManager.getConnection(url, username, password);
             Statement statement = connection.createStatement()) {

            ResultSet resultSet = statement.executeQuery("SELECT * FROM users");
            while (resultSet.next()) {
                System.out.println("User: " + resultSet.getString("name"));
            }
        } catch (SQLException e) {
            throw new SQLException("Ошибка работы с базой данных", e);
        }
    }
}
```

### Обработка пользовательских ошибок (Unchecked Exceptions)

Обработка ошибок, вызванных некорректным вводом пользователя:

```java
import java.util.*;

public class UserInputExample {
    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);

        try {
            System.out.print("Введите число: ");
            int number = Integer.parseInt(scanner.nextLine());
            System.out.println("Вы ввели: " + number);
        } catch (NumberFormatException e) {
            System.err.println("Некорректный ввод. Пожалуйста, введите число.");
        }
    }
}
```

---

## Заключение

Иерархия исключений помогает структурировать обработку ошибок, делая код более читаемым и надёжным.
- Используйте Checked Exceptions для ошибок, которые можно предвидеть (например, работа с файлами).
- Используйте Unchecked Exceptions для обработки логических ошибок (например, деление на ноль).

