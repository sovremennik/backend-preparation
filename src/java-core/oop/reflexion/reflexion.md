#### [Что такое рефлексия](#Что-такое-рефлексия-1)
#### [Основные классы для работы с рефлексией](#Основные-классы-для-работы-с-рефлексией-1)
#### [Примеры использования рефлексии](#Примеры-использования-рефлексии-1)
#### [Особенности работы с модификаторами](#Особенности-работы-с-модификаторами-1)
#### [Преимущества и недостатки](#Преимущества-и-недостатки-1)

## Что такое рефлексия
Рефлексия (Reflection) в Java — это механизм, предоставляющий возможность изучать структуру классов и объектов, а также модифицировать их поведение во время выполнения программы. Она используется для динамического получения информации о классах, полях, методах и конструкторах. Рефлексия реализована через API из пакета `java.lang.reflect`.

Основные задачи, решаемые с помощью рефлексии:
- Исследование структуры класса.
- Динамическое создание объектов.
- Вызов методов.
- Изменение значений полей, включая приватные.

---

## Основные классы для работы с рефлексией

1. **`Class`** — предоставляет информацию о классе или интерфейсе.
    - Методы, которые часто используются:
        - `getName()`, `getSimpleName()` — получение имени класса.
        - `getDeclaredFields()`, `getDeclaredMethods()` — получение всех (включая приватные) полей и методов.
        - `getSuperclass()` — получение родительского класса.
        - `getInterfaces()` — получение реализованных интерфейсов.

2. **`Field`** — предоставляет доступ к полям класса.
    - Можно получить значения полей (включая приватные) с помощью `get()`, а изменить — через `set()`.
    - Методы: `getName()`, `getType()`, `setAccessible(true)`.

3. **`Method`** — предоставляет информацию о методах класса.
    - Методы: `invoke()`, `getParameterTypes()`, `getReturnType()`, `getModifiers()`.

4. **`Constructor`** — позволяет работать с конструкторами.
    - Методы: `newInstance()`, `getParameterTypes()`.

---

## Примеры использования рефлексии

### 1. Получение информации о классе

```java
import java.lang.reflect.*;

public class ReflectionExample {
    public static void main(String[] args) {
        Class<?> clazz = ExampleClass.class;

        // Получение имени класса
        System.out.println("Class Name: " + clazz.getName());

        // Получение всех полей
        Field[] fields = clazz.getDeclaredFields();
        for (Field field : fields) {
            System.out.println("Field: " + field.getName() + " (Type: " + field.getType() + ")");
        }

        // Получение всех методов
        Method[] methods = clazz.getDeclaredMethods();
        for (Method method : methods) {
            System.out.println("Method: " + method.getName() + " (Return Type: " + method.getReturnType() + ")");
        }

        // Получение всех конструкторов
        Constructor<?>[] constructors = clazz.getDeclaredConstructors();
        for (Constructor<?> constructor : constructors) {
            System.out.println("Constructor: " + constructor.getName());
        }
    }
}

class ExampleClass {
    private String privateField;
    public int publicField;

    public ExampleClass() {}
    public ExampleClass(String privateField, int publicField) {
        this.privateField = privateField;
        this.publicField = publicField;
    }

    public String getPrivateField() {
        return privateField;
    }

    private void privateMethod() {}
}
```

#### Результат выполнения:
```
Class Name: ExampleClass
Field: privateField (Type: class java.lang.String)
Field: publicField (Type: int)
Method: getPrivateField (Return Type: class java.lang.String)
Method: privateMethod (Return Type: void)
Constructor: ExampleClass
Constructor: ExampleClass
```

---

### 2. Динамическое создание объектов

```java
import java.lang.reflect.*;

public class DynamicInstanceCreation {
    public static void main(String[] args) throws Exception {
        Class<?> clazz = ExampleClass.class;

        // Получение конструктора с параметрами и создание объекта
        Constructor<?> constructor = clazz.getConstructor(String.class, int.class);
        ExampleClass instance = (ExampleClass) constructor.newInstance("Example", 42);

        System.out.println("Created instance: " + instance);
    }
}

class ExampleClass {
    private String name;
    private int value;

    public ExampleClass(String name, int value) {
        this.name = name;
        this.value = value;
    }

    @Override
    public String toString() {
        return "ExampleClass{name='" + name + "', value=" + value + "}";
    }
}
```

#### Вывод:
```
Created instance: ExampleClass{name='Example', value=42}
```

---

### 3. Изменение значений приватных полей

```java
import java.lang.reflect.*;

public class ModifyPrivateFields {
    public static void main(String[] args) throws Exception {
        ExampleClass instance = new ExampleClass("Initial", 100);

        Class<?> clazz = instance.getClass();

        Field field = clazz.getDeclaredField("name");
        field.setAccessible(true); // Разрешаем доступ к приватному полю

        field.set(instance, "Modified");
        System.out.println("Updated instance: " + instance);
    }
}

class ExampleClass {
    private String name;
    private int value;

    public ExampleClass(String name, int value) {
        this.name = name;
        this.value = value;
    }

    @Override
    public String toString() {
        return "ExampleClass{name='" + name + "', value=" + value + "}";
    }
}
```

#### Вывод:
```
Updated instance: ExampleClass{name='Modified', value=100}
```

---

## Особенности работы с модификаторами

1. **Поля и методы с модификатором `private`** доступны только после вызова `setAccessible(true)`.
2. **Статические поля и методы** можно получать с помощью рефлексии, но не требуется создание экземпляра класса.
3. **Модификаторы:**
    - Используйте `Modifier.isStatic()`, `Modifier.isPrivate()` для проверки модификаторов метода или поля.

Пример проверки статического поля:

```java
Field staticField = clazz.getDeclaredField("staticField");
if (Modifier.isStatic(staticField.getModifiers())) {
    System.out.println("Field is static");
}
```

---

## Преимущества и недостатки

### Преимущества:
- Позволяет писать универсальный код, работающий с любыми классами.
- Широко используется в фреймворках (например, Spring, Hibernate).

### Недостатки:
- **Производительность:** рефлексия работает медленнее, чем обычные вызовы методов.
- **Безопасность:** нарушает инкапсуляцию и доступ к приватным данным.
- **Сложность отладки:** динамические вызовы сложнее анализировать.

