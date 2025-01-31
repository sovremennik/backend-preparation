
## Сериализация

### Определение
Сериализация — это процесс преобразования объекта в поток байтов для последующего сохранения в файл, передачи по сети или сохранения в базе данных. Обратный процесс называется десериализацией, при котором поток байтов преобразуется обратно в объект.

---

### Основные особенности
1. **Интерфейс Serializable**
    - Класс должен реализовывать интерфейс `java.io.Serializable` для возможности сериализации.
    - Этот интерфейс является маркерным (не содержит методов), указывающим JVM, что объект данного класса может быть сериализован.

2. **Ключевые аспекты сериализации**
    - Поля, объявленные как `transient`, не сериализуются.
    - Поля, объявленные как `static`, также не сериализуются, так как они принадлежат классу, а не конкретному объекту.
    - Сериализация используется только для сохранения состояния объекта.

3. **serialVersionUID**
    - Поле `serialVersionUID` служит уникальным идентификатором версии класса. Оно используется для проверки совместимости класса при десериализации.
    - Если идентификаторы не совпадают, выбрасывается исключение `InvalidClassException`.

```java
private static final long serialVersionUID = 1L;
```

---

### Пример сериализации и десериализации
```java
import java.io.*;

public class Employee implements Serializable {
    private static final long serialVersionUID = 1L;

    private String name;
    private int age;
    private transient String password;

    public Employee(String name, int age, String password) {
        this.name = name;
        this.age = age;
        this.password = password;
    }

    @Override
    public String toString() {
        return "Employee{" +
                "name='" + name + '\'' +
                ", age=" + age +
                ", password='" + password + '\'' +
                '}';
    }

    public static void main(String[] args) {
        Employee emp = new Employee("Alice", 30, "secret123");

        // Сериализация
        try (ObjectOutputStream oos = new ObjectOutputStream(new FileOutputStream("employee.ser"))) {
            oos.writeObject(emp);
        } catch (IOException e) {
            e.printStackTrace();
        }

        // Десериализация
        try (ObjectInputStream ois = new ObjectInputStream(new FileInputStream("employee.ser"))) {
            Employee deserializedEmp = (Employee) ois.readObject();
            System.out.println(deserializedEmp);
            // Output: Employee{name='Alice', age=30, password='null'}
        } catch (IOException | ClassNotFoundException e) {
            e.printStackTrace();
        }
    }
}
```

**Объяснение примера:**
- Поле `password`, объявленное как `transient`, не сериализуется и после десериализации становится `null`.
- Поле `serialVersionUID` гарантирует совместимость версии класса.

---

### Коллизии serialVersionUID

#### Что такое коллизия?
Коллизия происходит, когда сериализованный объект класса одной версии пытаются десериализовать в объект класса с изменённой структурой, но с тем же `serialVersionUID`.

#### Причины коллизий
- Изменение структуры класса: добавление или удаление полей, изменение их типов.
- Неявное управление `serialVersionUID` (например, если поле не задано, JVM генерирует его автоматически на основе структуры класса).

#### Пример проблемы:
```java
// Первая версия класса
public class Person implements Serializable {
    private String name;
    private int age;
}

// Вторая версия класса (добавлено новое поле)
public class Person implements Serializable {
    private String name;
    private int age;
    private String address; // Новое поле
}
```
Если при десериализации объекта первой версии используется вторая версия класса, может возникнуть `InvalidClassException`.

#### Решение проблемы:
1. **Явное указание serialVersionUID:**
    - Всегда задавайте поле `serialVersionUID` вручную для контроля версий.

```java
private static final long serialVersionUID = 1L;
```

2. **Контроль изменений структуры класса:**
    - При необходимости изменения структуры, учитывайте, что десериализация может быть несовместимой.

3. **Использование пользовательской логики десериализации:**
    - Реализуйте методы `writeObject` и `readObject` для управления процессом сериализации/десериализации.

---

### Итоги
| **Особенность**            | **Описание**                                                                           |
|-----------------------------|---------------------------------------------------------------------------------------|
| `Serializable`             | Маркерный интерфейс для сериализации объектов.                                        |
| `transient`                | Исключает поле из сериализации.                                                      |
| `serialVersionUID`         | Идентификатор версии класса для контроля совместимости.                              |
| Коллизии `serialVersionUID` | Возникают при несовпадении версии класса между сериализацией и десериализацией.       |

Сериализация является мощным механизмом для работы с объектами в Java, но требует тщательного подхода при изменении структуры классов, чтобы избежать проблем совместимости.
