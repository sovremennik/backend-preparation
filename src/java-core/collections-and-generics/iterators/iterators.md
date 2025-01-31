#### [Iterator, ListIterator, и их отличия](#iterator-listiterator-и-их-отличия-1)
#### [Использование for-each vs итераторы](#Использование-for-each-vs-итераторы-1)

---
# Итераторы

Итераторы в Java предоставляют стандартный способ перебора элементов коллекций. Они являются частью `java.util` и поддерживаются всеми стандартными коллекциями, такими как `List`, `Set`, `Map`, и другими. Итераторы упрощают доступ к элементам коллекции без необходимости работы с её внутренней структурой.

## Iterator, ListIterator, и их отличия

### Iterator
- **Описание:** Интерфейс `Iterator` позволяет перебирать элементы коллекции в одном направлении — от начала до конца.
- **Методы:**
    - `hasNext()`: проверяет, есть ли следующий элемент в коллекции.
    - `next()`: возвращает следующий элемент.
    - `remove()`: удаляет последний возвращённый элемент (опционально).
- **Поддерживаемые коллекции:** `Set`, `List`, `Queue`, и другие.

#### Пример использования:
```java
import java.util.*;

public class IteratorExample {
    public static void main(String[] args) {
        List<String> names = Arrays.asList("Alice", "Bob", "Charlie");
        Iterator<String> iterator = names.iterator();

        while (iterator.hasNext()) {
            String name = iterator.next();
            System.out.println(name);
        }
    }
}
```

### ListIterator
- **Описание:** Расширяет функциональность `Iterator` и используется исключительно с `List`. Поддерживает перебор элементов в обоих направлениях.
- **Методы:**
    - Все методы `Iterator`.
    - `hasPrevious()`: проверяет, есть ли предыдущий элемент.
    - `previous()`: возвращает предыдущий элемент.
    - `add(E e)`: добавляет элемент перед текущей позицией.
    - `set(E e)`: заменяет последний возвращённый элемент.
- **Поддерживаемые коллекции:** только `List`.

#### Пример использования:
```java
import java.util.*;

public class ListIteratorExample {
    public static void main(String[] args) {
        List<String> names = new ArrayList<>(Arrays.asList("Alice", "Bob", "Charlie"));
        ListIterator<String> listIterator = names.listIterator();

        while (listIterator.hasNext()) {
            System.out.println("Forward: " + listIterator.next());
        }

        while (listIterator.hasPrevious()) {
            System.out.println("Backward: " + listIterator.previous());
        }
    }
}
```

### Отличия между Iterator и ListIterator
| Характеристика           | Iterator                     | ListIterator                  |
|--------------------------|------------------------------|-------------------------------|
| Поддержка направлений    | Только вперёд                | Вперёд и назад                |
| Поддерживаемые коллекции | Любая коллекция              | Только `List`                 |
| Удаление элементов       | Да                           | Да                            |
| Замена элементов         | Нет                          | Да                            |
| Добавление элементов     | Нет                          | Да                            |

## Использование for-each vs итераторы

- **`for-each` (enhanced for-loop):**
    - Упрощённый синтаксис для итерации по коллекциям и массивам.
    - Менее гибок: не предоставляет возможности удаления или изменения элементов.
    - Подходит для большинства сценариев, где требуется только чтение данных.

#### Пример:
```java
List<String> names = Arrays.asList("Alice", "Bob", "Charlie");
for (String name : names) {
    System.out.println(name);
}
```

- **Итераторы:**
    - Предоставляют больше контроля (например, удаление или изменение элементов).
    - Используются в случаях, когда требуется модифицировать коллекцию во время итерации.

#### Пример:
```java
List<String> names = new ArrayList<>(Arrays.asList("Alice", "Bob", "Charlie"));
Iterator<String> iterator = names.iterator();

while (iterator.hasNext()) {
    if (iterator.next().equals("Bob")) {
        iterator.remove();
    }
}
System.out.println(names); // [Alice, Charlie]
```

## Модификация коллекций во время итерации

Модификация коллекции во время итерации без использования итератора может привести к `ConcurrentModificationException`. Это происходит из-за того, что итераторы отслеживают изменения коллекции через механизм модификационных счётчиков.

### Правильная модификация с помощью итератора
Используйте метод `remove()` итератора для безопасного удаления элементов во время итерации:
```java
List<Integer> numbers = new ArrayList<>(Arrays.asList(1, 2, 3, 4, 5));
Iterator<Integer> iterator = numbers.iterator();

while (iterator.hasNext()) {
    if (iterator.next() % 2 == 0) {
        iterator.remove();
    }
}
System.out.println(numbers); // [1, 3, 5]
```

### Альтернатива: Использование потоков (Streams)
Для функционального подхода можно использовать Stream API для фильтрации элементов:
```java
List<Integer> numbers = Arrays.asList(1, 2, 3, 4, 5);
numbers = numbers.stream().filter(n -> n % 2 != 0).toList();
System.out.println(numbers); // [1, 3, 5]
```

### Итераторы и многопоточность
Для многопоточных операций используйте безопасные коллекции, такие как `CopyOnWriteArrayList`, или синхронизируйте доступ к коллекции вручную.
