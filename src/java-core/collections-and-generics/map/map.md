#### [Различия между HashMap, TreeMap, и LinkedHashMap](#Различия-между-hashmap-treemap-и-linkedhashmap-1)
#### [Как избежать коллизий в HashMap](#Как-избежать-коллизий-в-hashmap-1)
#### [Примеры работы с ConcurrentHashMap](#Примеры-работы-с-concurrenthashmap-1)

---
# Map — ключи и значения

`Map` — это интерфейс в Java, представляющий структуру данных, которая отображает ключи на значения. Каждый ключ уникален и связан с одним значением.

## Различия между HashMap, TreeMap, и LinkedHashMap

### HashMap
**HashMap** — реализация интерфейса `Map`, основанная на хэш-таблице.

**Особенности:**
- **Порядок хранения**: Не гарантируется.
- **Скорость операций**: Средняя сложность для операций `put`, `get`, `remove` — O(1).
- **Коллизии**: Решаются с использованием цепочек (связанного списка), а начиная с Java 8 — сбалансированных деревьев для длинных цепочек.
- **Потокобезопасность**: Не является потокобезопасной.

Пример:
```java
import java.util.HashMap;
import java.util.Map;

public class Main {
    public static void main(String[] args) {
        Map<String, Integer> userAccessCount = new HashMap<>();
        userAccessCount.put("user1", 5);
        userAccessCount.put("user2", 10);
        userAccessCount.put("admin", 25);

        System.out.println("Access count for user1: " + userAccessCount.get("user1"));
    }
}
```

---

### TreeMap
**TreeMap** — реализация интерфейса `NavigableMap`, использующая красно-черное дерево для хранения пар "ключ-значение".

**Особенности:**
- **Порядок хранения**: Ключи хранятся в отсортированном порядке (естественном или заданном компаратором).
- **Скорость операций**: Все операции выполняются за O(log n).
- **Дополнительные возможности**: Методы навигации (`headMap`, `tailMap`, `subMap`).
- **Потокобезопасность**: Не является потокобезопасной.

Пример:
```java
import java.util.TreeMap;

public class Main {
    public static void main(String[] args) {
        TreeMap<Integer, String> errorCodes = new TreeMap<>();
        errorCodes.put(404, "Not Found");
        errorCodes.put(200, "OK");
        errorCodes.put(500, "Internal Server Error");

        System.out.println("Sorted error codes: " + errorCodes);
    }
}
```

---

### LinkedHashMap
**LinkedHashMap** — расширение `HashMap`, которое сохраняет порядок вставки элементов.

**Особенности:**
- **Порядок хранения**: Соответствует порядку вставки или доступу (при использовании access-order).
- **Скорость операций**: Как и в `HashMap`, O(1) в среднем.
- **Коллизии**: Обрабатываются так же, как в `HashMap`.
- **Потокобезопасность**: Не является потокобезопасной.

Пример:
```java
import java.util.LinkedHashMap;
import java.util.Map;

public class Main {
    public static void main(String[] args) {
        Map<String, String> apiEndpoints = new LinkedHashMap<>();
        apiEndpoints.put("GET /users", "Fetch all users");
        apiEndpoints.put("POST /users", "Create a user");
        apiEndpoints.put("DELETE /users/{id}", "Delete a user");

        System.out.println("API Endpoints: " + apiEndpoints);
    }
}
```

---

## Как избежать коллизий в HashMap
Коллизии возникают, когда два разных ключа имеют одинаковый хэш-код. Для их минимизации и обработки можно использовать следующие подходы:

1. **Правильная реализация метода `hashCode` и `equals`:**
   Убедитесь, что ваши ключи переопределяют эти методы корректно, чтобы гарантировать уникальность.

2. **Использование Immutable объектов в качестве ключей:**
   Например, вместо использования изменяемых классов (таких как `StringBuilder`) лучше использовать `String`.

3. **Увеличение размера карты:**
   В `HashMap` автоматически увеличивается ёмкость, но это можно сделать вручную, если заранее известен объем данных, чтобы уменьшить количество коллизий.

Пример:
```java
import java.util.HashMap;
import java.util.Map;
import java.util.Objects;

class User {
    private final String username;
    private final int id;

    public User(String username, int id) {
        this.username = username;
        this.id = id;
    }

    @Override
    public boolean equals(Object o) {
        if (this == o) return true;
        if (o == null || getClass() != o.getClass()) return false;
        User user = (User) o;
        return id == user.id && username.equals(user.username);
    }

    @Override
    public int hashCode() {
        return Objects.hash(username, id);
    }
}

public class Main {
    public static void main(String[] args) {
        Map<User, String> userRoles = new HashMap<>();
        userRoles.put(new User("john_doe", 1), "Admin");
        userRoles.put(new User("jane_doe", 2), "User");

        System.out.println(userRoles);
    }
}
```

---

## Примеры работы с ConcurrentHashMap
**ConcurrentHashMap** — потокобезопасная реализация `HashMap`, обеспечивающая высокую производительность в многопоточных приложениях.

**Особенности:**
- Поддерживает конкурентный доступ без полной блокировки карты.
- Использует сегментацию для минимизации блокировок.
- Гарантирует согласованность при чтении и записи.

Пример:
```java
import java.util.concurrent.ConcurrentHashMap;

public class Main {
    public static void main(String[] args) {
        ConcurrentHashMap<String, Integer> inventory = new ConcurrentHashMap<>();

        // Обновление в многопоточной среде
        inventory.put("Laptops", 50);
        inventory.put("Monitors", 30);

        inventory.compute("Laptops", (key, value) -> value - 1); // Продажа одного ноутбука

        System.out.println("Current inventory: " + inventory);
    }
}
```
