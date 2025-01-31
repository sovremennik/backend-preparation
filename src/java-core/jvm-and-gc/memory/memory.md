#### [Основные области памяти](#Основные-области-памяти-1)
#### [Типы ссылок](#Типы-ссылок-1)

---
# Устройство памяти

Java — это язык программирования, который использует модель управляемой памяти. Память, доступная JVM (Java Virtual Machine), делится на несколько областей, каждая из которых предназначена для выполнения определённых задач.

## Основные области памяти

1. **Heap (Куча):**
    - Основная область для хранения объектов, созданных программой, и метаданных классов.
    - Делится на следующие зоны:
        - **Young Generation:** Для недавно созданных объектов. Быстрая очистка.
        - **Old Generation:** Хранит объекты, которые пережили несколько сборок мусора.
        - **Metaspace:** Используется для хранения информации о классах. Размер ограничен только объёмом доступной памяти.

   Иллюстрация:
   ```
   +---------------------------------------+
   |               Heap                    |
   | +---------+  +--------------------+   |
   | |  Young  |  |       Old          |   |
   | |         |  |                    |   |
   | +---------+  +--------------------+   |
   |               Metaspace               |
   +---------------------------------------+
   ```

2. **Stack (Стек):**
    - Используется для хранения вызовов методов, их параметров и локальных переменных.
    - Каждый поток имеет свой собственный стек.

   Иллюстрация:
   ```
   +-----------+
   |   Stack   |
   |-----------|
   | Frame 1   |
   | Frame 2   |
   | Frame 3   |
   +-----------+
   ```

3. **Program Counter (PC):**
    - Указатель текущей инструкции, которая выполняется в потоке.
    - Хранит адрес следующей команды для выполнения.

4. **Native Method Stack:**
    - Используется для вызовов нативных методов, написанных на других языках, таких как C или C++.

5. **Code Cache (Кодовый кэш):**
    - Область для хранения скомпилированного байт-кода, преобразованного JIT-компилятором в машинный код.
    - Ускоряет выполнение часто используемых участков кода.

   Иллюстрация:
   ```
   +----------------+
   |   Code Cache   |
   |----------------|
   | Compiled Code  |
   | Optimized Code |
   +----------------+
   ```

## Типы ссылок

Java предоставляет четыре типа ссылок, которые различаются по степени их взаимодействия с Garbage Collector (GC). Давайте разберём их на практике с примерами и выводом.

### 1. Strong Reference (Сильная ссылка)

- Это стандартный тип ссылки. Пока объект имеет хотя бы одну сильную ссылку, GC не может его удалить.

#### Пример:

```java
public class StrongReferenceExample {
    public static void main(String[] args) {
        Object strongRef = new Object();
        System.out.println("Object создан: " + strongRef);

        // Присваиваем null, чтобы объект стал доступным для GC
        strongRef = null;
        System.gc();

        System.out.println("После GC объект всё ещё существует, если на него есть сильная ссылка.");
    }
}
```

#### Вывод:
```
Object создан: java.lang.Object@1540e19d
После GC объект всё ещё существует, если на него есть сильная ссылка.
```

### 2. Soft Reference (Мягкая ссылка)

- GC удаляет объекты с мягкими ссылками только при нехватке памяти. Полезно для кэширования.

#### Пример:

```java
import java.lang.ref.SoftReference;

public class SoftReferenceExample {
    public static void main(String[] args) {
        Object obj = new Object();
        SoftReference<Object> softRef = new SoftReference<>(obj);

        System.out.println("SoftReference создан: " + softRef.get());

        obj = null;
        System.gc();

        System.out.println("После GC: " + softRef.get());
    }
}
```

#### Вывод:
```
SoftReference создан: java.lang.Object@1540e19d
После GC: java.lang.Object@1540e19d
```
> Если памяти недостаточно, `softRef.get()` вернёт `null`.

### 3. Weak Reference (Слабая ссылка)

- Объекты с такими ссылками удаляются GC при следующем сборке, если нет сильных ссылок.

#### Пример:

```java
import java.lang.ref.WeakReference;

public class WeakReferenceExample {
    public static void main(String[] args) {
        Object obj = new Object();
        WeakReference<Object> weakRef = new WeakReference<>(obj);

        System.out.println("WeakReference создан: " + weakRef.get());

        obj = null;
        System.gc();

        System.out.println("После GC: " + weakRef.get());
    }
}
```

#### Вывод:
```
WeakReference создан: java.lang.Object@1540e19d
После GC: null
```

### 4. Phantom Reference (Фантомная ссылка)

- Эти ссылки используются для отслеживания удаления объекта из памяти. Они всегда возвращают `null` через `get()`.

#### Пример:

```java
import java.lang.ref.PhantomReference;
import java.lang.ref.ReferenceQueue;

public class PhantomReferenceExample {
    public static void main(String[] args) throws InterruptedException {
        Object obj = new Object();
        ReferenceQueue<Object> refQueue = new ReferenceQueue<>();
        PhantomReference<Object> phantomRef = new PhantomReference<>(obj, refQueue);

        System.out.println("PhantomReference создан: " + phantomRef.get());

        obj = null;
        System.gc();

        // Ожидаем, пока объект попадёт в очередь ссылок
        System.out.println("После GC PhantomReference в очереди: " + (refQueue.poll() != null));
    }
}
```

#### Вывод:
```
PhantomReference создан: null
После GC PhantomReference в очереди: true
```

---

### Основные выводы

1. **Strong Reference:** Объект остаётся в памяти, пока есть сильная ссылка.
2. **Soft Reference:** Удобна для кэширования, так как GC удаляет объект только при нехватке памяти.
3. **Weak Reference:** Полезна для коллекций, где объекты могут быть удалены GC при следующей сборке.
4. **Phantom Reference:** Используется для отслеживания удаления объектов и выполнения завершающих операций.

