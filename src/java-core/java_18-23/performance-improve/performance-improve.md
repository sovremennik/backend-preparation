#### [Введение](#Введение-1)
#### [Улучшения в производительности](#Улучшения-в-производительности-1)
#### [Улучшения в управлении памятью](#Улучшения-в-управлении-памятью-1)
#### [Новые api для управления памятью](#Новые-api-для-управления-памятью-1)

---
# Улучшения производительности и памяти (Java 18-23)

## Введение

Java 18-23 сосредоточилась на оптимизации производительности и более эффективной работе с памятью. Эти версии улучшили время выполнения программ, уменьшили потребление ресурсов и обеспечили более эффективное управление данными, что особенно важно для высоконагруженных систем. Рассмотрим ключевые изменения, их применение и внутреннюю реализацию.

---

## Улучшения в производительности

### 1. **Шаблоны Vector API**

**Цель:** предоставить высокопроизводительное API для работы с векторными вычислениями, используя SIMD (Single Instruction Multiple Data). Это позволяет обрабатывать несколько элементов данных за одну операцию, что значительно ускоряет вычисления.

**Пример применения:**

```java
import jdk.incubator.vector.*;

public class VectorExample {
    public static void main(String[] args) {
        var species = FloatVector.SPECIES_PREFERRED;

        float[] a = {1.0f, 2.0f, 3.0f, 4.0f};
        float[] b = {5.0f, 6.0f, 7.0f, 8.0f};
        float[] c = new float[4];

        var va = FloatVector.fromArray(species, a, 0);
        var vb = FloatVector.fromArray(species, b, 0);
        var vc = va.add(vb);

        vc.intoArray(c, 0);

        System.out.println("Result: " + Arrays.toString(c));
    }
}
```

**Output:**

```
Result: [6.0, 8.0, 10.0, 12.0]
```

Под капотом: используется SIMD, что позволяет процессору обрабатывать несколько чисел одновременно, избегая цикла на уровне JVM.

---

### 2. **Постоянные классы (JEP 395)**

**Цель:** ускорить загрузку и повысить производительность путём создания неизменяемых объектов, таких как `java.lang.String`.

**Пример:**

```java
record Point(int x, int y) {
    public Point {
        if (x < 0 || y < 0) {
            throw new IllegalArgumentException("Coordinates must be non-negative.");
        }
    }
}

public class RecordExample {
    public static void main(String[] args) {
        Point p1 = new Point(10, 20);
        System.out.println(p1);
    }
}
```

**Output:**

```
Point[x=10, y=20]
```

Под капотом: компилятор автоматически генерирует неизменяемый класс с методами `equals`, `hashCode` и `toString`, что упрощает код и снижает ошибки.

---

## Улучшения в управлении памятью

### 1. **G1 Garbage Collector (JEP 346)**

G1 GC получил улучшения в скорости выполнения сборки мусора и распределении памяти. Теперь уменьшено время паузы, а обработка больших объектов стала более оптимальной.

**Схема работы G1 GC:**

```
+-----------------+-----------------+
| Eden Space      | Survivor Space |
+-----------------+-----------------+
         |
         v
+-----------------------------------+
|          Old Generation           |
+-----------------------------------+
```

**Пример:**

```java
public class GCDemo {
    public static void main(String[] args) {
        for (int i = 0; i < 1_000_000; i++) {
            String data = "Data" + i;
            if (i % 100_000 == 0) {
                System.out.println("Iteration: " + i);
            }
        }
    }
}
```

**Output:**

```
Iteration: 0
Iteration: 100000
Iteration: 200000
...
```

Под капотом: объекты сначала помещаются в Eden Space, затем перемещаются в Survivor Space, а позже в Old Generation, если они долго живут. G1 GC минимизирует перемещения для сокращения пауз.

---

### 2. **Z Garbage Collector (JEP 377)**

**Цель:** минимизировать задержки при сборке мусора, обеспечивая паузы в пределах нескольких миллисекунд.

**Особенности:**

- Работает с большими объемами памяти (>16 ТБ).
- Практически мгновенное освобождение памяти.

**Пример:**

Запуск с использованием ZGC:

```bash
java -XX:+UseZGC -Xmx10g MyApp
```

**Символьная схема ZGC:**

```
[ Allocation ] --> [ Marking ] --> [ Relocation ]
```

Под капотом: ZGC разбивает память на регионы, отслеживает живые объекты с минимальными накладными расходами и перемещает их при необходимости.

---

## Новые API для управления памятью

### 1. **Foreign Function & Memory API (JEP 424)**

Позволяет работать с памятью за пределами Java Heap, что снижает накладные расходы для задач, требующих взаимодействия с нативным кодом.

**Пример:**

```java
import java.lang.foreign.*;

public class MemoryAccessExample {
    public static void main(String[] args) {
        try (Arena arena = Arena.ofConfined()) {
            MemorySegment segment = arena.allocate(100);

            segment.setAtIndex(ValueLayout.JAVA_INT, 0, 42);
            System.out.println("Value: " + segment.get(ValueLayout.JAVA_INT, 0));
        }
    }
}
```

**Output:**

```
Value: 42
```

Под капотом: API позволяет выделять память, аналогично работе с `malloc` в C, с добавлением автоматического управления жизненным циклом через `Arena`.
