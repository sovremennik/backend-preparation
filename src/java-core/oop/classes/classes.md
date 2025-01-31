## Создание классов и объектов

Создание классов и объектов — фундаментальный аспект объектно-ориентированного программирования. Классы служат шаблонами для объектов, а объекты представляют конкретные экземпляры этих шаблонов. В Java создание классов и объектов позволяет инкапсулировать данные и поведение.

### Определение и структура класса

Класс определяет структуру и поведение объектов. Он может содержать поля (переменные) и методы (функции).

**Пример:**
```java
public class Person {
    private String name;
    private int age;

    // Конструктор
    public Person(String name, int age) {
        this.name = name;
        this.age = age;
    }

    // Метод для получения имени
    public String getName() {
        return name;
    }

    // Метод для установки имени
    public void setName(String name) {
        this.name = name;
    }

    // Метод для вывода информации о человеке
    public void displayInfo() {
        System.out.println("Name: " + name + ", Age: " + age);
    }
}
```

### Создание объектов

Для создания объекта используется ключевое слово `new`, которое выделяет память и вызывает конструктор класса.

**Пример:**
```java
public class Main {
    public static void main(String[] args) {
        // Создание объекта класса Person
        Person person = new Person("Alice", 30);
        person.displayInfo();
    }
}
```

**Результат:**
```
Name: Alice, Age: 30
```

### Конструкторы

Конструктор — это специальный метод, который вызывается при создании объекта. Если конструктор не определён, Java автоматически создаёт конструктор по умолчанию.

**Пример:**
```java
public class Car {
    private String model;

    // Конструктор
    public Car(String model) {
        this.model = model;
    }

    public void showModel() {
        System.out.println("Car model: " + model);
    }
}
```

### Перегрузка конструкторов

Java позволяет создавать несколько конструкторов с разными параметрами.

**Пример:**
```java
public class Book {
    private String title;
    private String author;

    // Конструктор с одним параметром
    public Book(String title) {
        this.title = title;
        this.author = "Unknown";
    }

    // Конструктор с двумя параметрами
    public Book(String title, String author) {
        this.title = title;
        this.author = author;
    }

    public void displayInfo() {
        System.out.println("Title: " + title + ", Author: " + author);
    }
}
```

**Использование:**
```java
public class Main {
    public static void main(String[] args) {
        Book book1 = new Book("Effective Java");
        Book book2 = new Book("Clean Code", "Robert C. Martin");

        book1.displayInfo();
        book2.displayInfo();
    }
}
```

**Результат:**
```
Title: Effective Java, Author: Unknown
Title: Clean Code, Author: Robert C. Martin
```

### Статические поля и методы

Статические поля и методы принадлежат классу, а не его объектам.

**Пример:**
```java
public class MathUtils {
    public static final double PI = 3.14159;

    public static double square(double number) {
        return number * number;
    }
}

public class Main {
    public static void main(String[] args) {
        System.out.println("PI: " + MathUtils.PI);
        System.out.println("Square of 4: " + MathUtils.square(4));
    }
}
```

**Результат:**
```
PI: 3.14159
Square of 4: 16.0
```
### Вложенные и внутренние классы

Java поддерживает два типа классов, которые могут быть определены внутри других классов:

1. **Вложенные классы (Static Nested Classes)** — это статические классы, которые не имеют доступа к нестатическим членам внешнего класса.
2. **Внутренние классы (Inner Classes)** — это нестатические классы, которые имеют доступ ко всем полям и методам внешнего класса.

#### Вложенные классы (Static Nested Classes)

Вложенные классы полезны, когда необходимо сгруппировать вспомогательную функциональность, которая логически связана с внешним классом.

**Пример вложенного класса:**
```java
public class Outer {
    static class StaticNested {
        void display() {
            System.out.println("Это вложенный статический класс");
        }
    }
}

public class Main {
    public static void main(String[] args) {
        Outer.StaticNested nested = new Outer.StaticNested();
        nested.display();
    }
}
```
**Результат:**
```
Это вложенный статический класс
```

#### Внутренние классы (Inner Classes)

Внутренние классы могут свободно обращаться к полям и методам внешнего класса, включая приватные.

**Пример внутреннего класса:**
```java
public class Outer {
    private String message = "Это внутренний класс";

    class Inner {
        void display() {
            System.out.println(message);
        }
    }
}

public class Main {
    public static void main(String[] args) {
        Outer outer = new Outer();
        Outer.Inner inner = outer.new Inner();
        inner.display();
    }
}
```
**Результат:**
```
Это внутренний класс
```

#### Локальные внутренние классы (Local Inner Classes)

Локальные внутренние классы определяются внутри метода и доступны только в этом методе.

**Пример локального внутреннего класса:**
```java
public class Outer {
    void display() {
        class LocalInner {
            void show() {
                System.out.println("Это локальный внутренний класс");
            }
        }
        LocalInner local = new LocalInner();
        local.show();
    }
}

public class Main {
    public static void main(String[] args) {
        Outer outer = new Outer();
        outer.display();
    }
}
```
**Результат:**
```
Это локальный внутренний класс
```

#### Пример взаимодействия внешнего и внутреннего классов

**Пример:**
```java
public class DatabaseConfig {
    private static String dbUrl = "jdbc:mysql://localhost:3306/db";
    private static String user = "root";
    private static String password = "password";

    static class ConnectionManager {
        public void connect() {
            System.out.println("Подключение к базе данных: " + dbUrl);
            System.out.println("Пользователь: " + user);
        }
    }
}

public class Main {
    public static void main(String[] args) {
        DatabaseConfig.ConnectionManager manager = new DatabaseConfig.ConnectionManager();
        manager.connect();
    }
}
```
**Результат:**
```
Подключение к базе данных: jdbc:mysql://localhost:3306/db
Пользователь: root
```

### Вывод
- **Вложенные классы** полезны для создания вспомогательных структур, не требующих доступа к нестатическим членам внешнего класса.
- **Внутренние классы** позволяют логически сгруппировать связанную функциональность с доступом ко всем членам внешнего класса.
- **Локальные внутренние классы** используются для временных вспомогательных задач в методах.

Такая структура помогает повысить читаемость и упрощает поддержку кода.