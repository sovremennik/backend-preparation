#### [Что такое Stream API?](#Что-такое-stream-api-1)
#### [Операции с Stream](#Операции-с-stream-1)
#### [Пример работы Stream API](#Пример-работы-stream-api-1)
#### [Продвинутые примеры использования](#Продвинутые-примеры-использования-1)

---
# Stream API в Java

Stream API был введён в Java 8 и предоставляет мощный инструмент для работы с данными в функциональном стиле. Он позволяет писать выразительный, читаемый и компактный код для обработки коллекций, массивов и других источников данных.

## Что такое Stream API?

**Stream** — это последовательность элементов, поддерживающая последовательные и параллельные операции обработки данных.

Ключевые свойства:
- **Ленивая обработка:** операции выполняются только при необходимости (например, при вызове терминальных операций).
- **Неизменяемость:** Stream не изменяет источник данных.
- **Одноразовость:** Stream можно использовать только один раз.

```java
List<Integer> numbers = List.of(1, 2, 3, 4, 5);

// Увеличим каждое число на 1 и выведем только чётные числа:
numbers.stream()
       .map(n -> n + 1)
       .filter(n -> n % 2 == 0)
       .forEach(System.out::println);
```

Вывод: `2, 4, 6`.

## Операции с Stream

### 1. **Промежуточные операции**

Эти операции возвращают новый Stream и выполняются лениво:
- **filter:** фильтрация элементов.
- **map:** преобразование элементов.
- **flatMap:** "разворачивание" вложенных структур.
- **distinct:** удаление дубликатов.
- **sorted:** сортировка.

### 2. **Терминальные операции**

Эти операции завершают работу Stream:
- **forEach:** выполнение действия для каждого элемента.
- **collect:** преобразование в коллекцию.
- **reduce:** агрегирование элементов (например, суммирование).
- **count:** подсчёт элементов.

## Пример работы Stream API

Рассмотрим пример: есть список заказов, где нужно выбрать заказы на сумму более 100, отсортировать их по дате и вывести IDs.

### Исходный список:

```
Источник данных:
[ {id=1, amount=50}, {id=2, amount=200}, {id=3, amount=150} ]
```

### Операции Stream:

```
Исходный поток:
 [ {id=1, amount=50}, {id=2, amount=200}, {id=3, amount=150} ]
      |
      v
filter(amount > 100):
 [ {id=2, amount=200}, {id=3, amount=150} ]
      |
      v
sorted(по дате):
 [ {id=3, amount=150}, {id=2, amount=200} ]
      |
      v
map(to ID):
 [ 3, 2 ]
      |
      v
forEach(System.out::println):
 3
 2
```

### Код:

```java
orders.stream()
      .filter(order -> order.getAmount() > 100)
      .sorted(Comparator.comparing(Order::getDate))
      .map(Order::getId)
      .forEach(System.out::println);
```

## Продвинутые примеры использования

Stream API — мощный инструмент для обработки данных в Java, позволяющий писать лаконичный, читаемый и эффективный код. Рассмотрим его продвинутые применения, включая параллельную обработку.

### Пример 1: Параллельная обработка данных

Stream API позволяет обрабатывать данные параллельно, используя несколько потоков, что особенно полезно при работе с большими объëмами данных.

#### Синтаксис и пример:
```java
import java.util.*;
import java.util.stream.*;

public class ParallelStreamExample {
    public static void main(String[] args) {
        List<Integer> numbers = IntStream.range(1, 1_000_000)
                                         .boxed()
                                         .collect(Collectors.toList());

        long startTime = System.nanoTime();
        long sum = numbers.stream()
                          .filter(n -> n % 2 == 0)
                          .mapToLong(Integer::longValue)
                          .sum();
        long sequentialTime = System.nanoTime() - startTime;

        startTime = System.nanoTime();
        long parallelSum = numbers.parallelStream()
                                   .filter(n -> n % 2 == 0)
                                   .mapToLong(Integer::longValue)
                                   .sum();
        long parallelTime = System.nanoTime() - startTime;

        System.out.printf("Sequential sum: %d, time: %d ns\n", sum, sequentialTime);
        System.out.printf("Parallel sum: %d, time: %d ns\n", parallelSum, parallelTime);
    }
}
```

#### Объяснение кода:
- **stream()** — последовательный поток.
- **parallelStream()** — параллельный поток.
- **filter(n -> n % 2 == 0)** — фильтрация только чëтных чисел.
- **mapToLong(Integer::longValue)** — преобразование Integer в long для суммирования.

#### Результаты работы:
При больших объëмах данных параллельная обработка показывает значительное ускорение по сравнению с последовательной.


### Пример 2: Схема работы параллельного потока

### Архитектура:
1. **Разделение данных** — входной поток разбивается на чанки (части).
2. **Обработка частей** — каждый чанк обрабатывается независимо.
3. **Объединение результатов** — результаты вычислений агрегируются в финальном шаге.

#### Схема:
```
[Input Data]  -->  Splitter (fork)
   |                           \
[Chunk 1]                 [Chunk 2]
   |                           |
[Process 1]             [Process 2]  (Parallel)
   \                           /
    \                         /
       -->  Combiner (merge)  --> [Result]
```

### Пример 3: Параллельная сортировка

Stream API поддерживает эффективную сортировку больших массивов с использованием параллельной обработки.

```java
import java.util.*;

public class ParallelSortExample {
    public static void main(String[] args) {
        int[] numbers = new Random().ints(1_000_000, 0, 1_000_000).toArray();

        long startTime = System.nanoTime();
        Arrays.sort(numbers); // Последовательная сортировка
        long sequentialTime = System.nanoTime() - startTime;

        numbers = new Random().ints(1_000_000, 0, 1_000_000).toArray();
        startTime = System.nanoTime();
        Arrays.parallelSort(numbers); // Параллельная сортировка
        long parallelTime = System.nanoTime() - startTime;

        System.out.printf("Sequential sort time: %d ns\n", sequentialTime);
        System.out.printf("Parallel sort time: %d ns\n", parallelTime);
    }
}
```

#### Объяснение:
- **Arrays.sort()** — выполняет сортировку последовательно.
- **Arrays.parallelSort()** — разбивает массив на чанки, сортирует их параллельно и объединяет результаты.


### Пример 4: Использование Custom Spliterator

Для особых случаев можно написать собственный **Spliterator**, чтобы определить, как данные будут разделяться для параллельной обработки.

```java
import java.util.*;
import java.util.stream.*;

public class CustomSpliteratorExample {
    public static void main(String[] args) {
        List<String> data = Arrays.asList("Java", "Parallel", "Stream", "API", "Example");

        Spliterator<String> spliterator = new CustomSpliterator(data);
        Stream<String> stream = StreamSupport.stream(spliterator, true);
        stream.forEach(System.out::println);
    }

    static class CustomSpliterator implements Spliterator<String> {
        private final List<String> list;
        private int current = 0;

        public CustomSpliterator(List<String> list) {
            this.list = list;
        }

        @Override
        public boolean tryAdvance(Consumer<? super String> action) {
            if (current < list.size()) {
                action.accept(list.get(current++));
                return true;
            }
            return false;
        }

        @Override
        public Spliterator<String> trySplit() {
            int size = list.size() - current;
            if (size < 2) {
                return null;
            }
            int splitIndex = size / 2 + current;
            List<String> subList = list.subList(current, splitIndex);
            current = splitIndex;
            return new CustomSpliterator(subList);
        }

        @Override
        public long estimateSize() {
            return list.size() - current;
        }

        @Override
        public int characteristics() {
            return ORDERED | SIZED | SUBSIZED;
        }
    }
}
```

Параллельная обработка в Stream API — это гибкий инструмент для повышения производительности. Однако необходимо учитывать затраты на синхронизацию и балансировку чанков, чтобы избежать ухудшения производительности в небольших или сложных задачах.
