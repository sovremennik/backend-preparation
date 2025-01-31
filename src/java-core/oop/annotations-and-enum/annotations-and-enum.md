
## Аннотации и enum

### Аннотации
Аннотации в Java представляют собой способ добавления метаинформации в код, который может быть использован компилятором, инструментами сборки или во время выполнения через рефлексию. Аннотации позволяют задавать дополнительные свойства или поведение для классов, методов, полей и других элементов кода.

#### Примеры встроенных аннотаций
1. **@Override**: Указывает, что метод переопределяет метод суперкласса.
   ```java
   @Override
   public String toString() {
       return "Custom implementation";
   }
   ```

2. **@Deprecated**: Помечает метод или класс как устаревший, предупреждая разработчиков о необходимости избегать его использования.
   ```java
   @Deprecated
   public void oldMethod() {
       System.out.println("This method is deprecated");
   }
   ```

3. **@SuppressWarnings**: Подавляет предупреждения компилятора.
   ```java
   @SuppressWarnings("unchecked")
   public void processList() {
       List rawList = new ArrayList(); // предупреждение подавлено
   }
   ```

#### Пользовательские аннотации
Аннотации можно создавать для решения специфических задач.

**Синтаксис создания:**
```java
import java.lang.annotation.ElementType;
import java.lang.annotation.Retention;
import java.lang.annotation.RetentionPolicy;
import java.lang.annotation.Target;

@Retention(RetentionPolicy.RUNTIME) // Аннотация доступна во время выполнения
@Target({ElementType.METHOD, ElementType.TYPE}) // Применяется к классам и методам
public @interface Author {
    String name();
    String date();
}

@Author(name = "John Doe", date = "2025-01-01")
public class MyClass {
    @Author(name = "Jane Doe", date = "2025-01-10")
    public void someMethod() {
        System.out.println("Executing annotated method");
    }
}
```

#### Ключевые параметры аннотаций
1. **@Retention**: Определяет, как долго аннотация сохраняется (SOURCE, CLASS, RUNTIME).
2. **@Target**: Указывает, к каким элементам можно применять аннотацию (например, FIELD, METHOD, TYPE).
3. **@Inherited**: Аннотация наследуется подклассами.
4. **@Repeatable**: Позволяет применять одну и ту же аннотацию несколько раз.

**Пример Repeatable-аннотации:**
```java
import java.lang.annotation.*;

@Retention(RetentionPolicy.RUNTIME)
@Target(ElementType.TYPE)
@Repeatable(Schedules.class)
public @interface Schedule {
    String day();
    String time();
}

@Retention(RetentionPolicy.RUNTIME)
@Target(ElementType.TYPE)
public @interface Schedules {
    Schedule[] value();
}

@Schedule(day = "Monday", time = "10:00")
@Schedule(day = "Wednesday", time = "14:00")
public class Meeting {}
```

---

### Enum (Перечисления)

Перечисления (enum) используются для определения набора именованных констант. Это мощный инструмент для работы с ограниченными и фиксированными наборами данных.

#### Основы перечислений
```java
public enum Day {
    MONDAY, TUESDAY, WEDNESDAY, THURSDAY, FRIDAY, SATURDAY, SUNDAY;
}

public class Main {
    public static void main(String[] args) {
        Day today = Day.MONDAY;

        switch (today) {
            case MONDAY -> System.out.println("Start of the work week");
            case FRIDAY -> System.out.println("Almost weekend!");
            default -> System.out.println("Another day");
        }
    }
}
```

#### Свойства и методы в enum
Перечисления могут содержать методы и поля, как и обычные классы.

```java
public enum Priority {
    HIGH(3), MEDIUM(2), LOW(1);

    private final int level;

    Priority(int level) {
        this.level = level;
    }

    public int getLevel() {
        return level;
    }
}

public class Main {
    public static void main(String[] args) {
        for (Priority p : Priority.values()) {
            System.out.println(p + " has level " + p.getLevel());
        }
    }
}
```

#### Абстрактные методы в enum
Перечисления могут содержать абстрактные методы, которые обязаны реализовываться в каждом элементе перечисления.

```java
public enum Operation {
    ADD {
        @Override
        public int apply(int a, int b) {
            return a + b;
        }
    },
    SUBTRACT {
        @Override
        public int apply(int a, int b) {
            return a - b;
        }
    };

    public abstract int apply(int a, int b);
}

public class Main {
    public static void main(String[] args) {
        System.out.println(Operation.ADD.apply(5, 3)); // 8
        System.out.println(Operation.SUBTRACT.apply(5, 3)); // 2
    }
}
```

#### Преимущества использования enum
1. Улучшение читаемости и безопасности кода.
2. Возможность добавления методов и логики в перечисления.
3. Использование в switch и сравнениях для компактного и понятного кода.

---

Аннотации и перечисления являются важными инструментами в языке Java, которые значительно упрощают разработку и поддерживаемость кода. Аннотации предоставляют мощные возможности для работы с метаинформацией, а перечисления упрощают работу с ограниченными наборами данных.

