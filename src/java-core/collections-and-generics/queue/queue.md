#### [Отличия между PriorityQueue, Deque, и BlockingQueue](#Отличия-между-priorityqueue-deque-и-blockingqueue-1)
#### [Реализация буферов с помощью очередей](#Реализация-буферов-с-помощью-очередей-1)

---
# Queue и её разновидности

## Отличия между PriorityQueue, Deque, и BlockingQueue

Интерфейс `Queue` представляет структуру данных "очередь", работающую по принципу FIFO (First-In-First-Out). Он определяет базовые методы для добавления, удаления и просмотра элементов. В Java существуют разные реализации очередей, каждая из которых имеет свои особенности:

### PriorityQueue
- **Особенности**: Хранит элементы в порядке их естественной сортировки или с использованием переданного компаратора.
- **Порядок**: Не гарантирует сохранение порядка вставки.
- **Допустимость null**: Не допускает `null` в качестве элемента.
- **Потокобезопасность**: Не является потокобезопасной.
- **Пример использования**: Планирование задач с разными приоритетами.

```java
import java.util.PriorityQueue;

PriorityQueue<Integer> queue = new PriorityQueue<>();
queue.add(3);
queue.add(1);
queue.add(2);

while (!queue.isEmpty()) {
    System.out.println(queue.poll()); // Выведет 1, 2, 3
}
```

### Deque (двусторонняя очередь)
- **Особенности**: Позволяет добавлять и удалять элементы с обеих сторон.
- **Реализации**: `ArrayDeque` (рекомендуемая), `LinkedList`.
- **Потокобезопасность**: Не является потокобезопасной.
- **Пример использования**: Реализация стека или очереди с двумя концами.

```java
import java.util.ArrayDeque;

ArrayDeque<String> deque = new ArrayDeque<>();
deque.addFirst("Фронт");
deque.addLast("Тыл");

System.out.println(deque.pollFirst()); // Фронт
System.out.println(deque.pollLast());  // Тыл
```

### BlockingQueue
- **Особенности**: Поддерживает блокирующие операции для потокобезопасной работы.
- **Реализации**: `ArrayBlockingQueue`, `LinkedBlockingQueue`, `PriorityBlockingQueue`.
- **Потокобезопасность**: Является потокобезопасной.
- **Пример использования**: Очередь задач для потоков.

```java
import java.util.concurrent.ArrayBlockingQueue;

ArrayBlockingQueue<String> queue = new ArrayBlockingQueue<>(2);

queue.put("Задача 1");
queue.put("Задача 2");

new Thread(() -> {
    try {
        System.out.println(queue.take()); // Задача 1
    } catch (InterruptedException e) {
        Thread.currentThread().interrupt();
    }
}).start();
```

## Реализация буферов с помощью очередей

Очереди часто используются для реализации буферов данных в многопоточных системах. Например, для обработки входящих запросов или передачи данных между потоками.

### Буфер логирования с `BlockingQueue`
```java
import java.util.concurrent.*;

BlockingQueue<String> logBuffer = new LinkedBlockingQueue<>();

// Поток записи логов
new Thread(() -> {
    while (true) {
        try {
            String log = logBuffer.take();
            System.out.println("Лог: " + log);
        } catch (InterruptedException e) {
            Thread.currentThread().interrupt();
        }
    }
}).start();

// Поток добавления логов
new Thread(() -> {
    try {
        logBuffer.put("Ошибка в системе");
        logBuffer.put("Система восстановлена");
    } catch (InterruptedException e) {
        Thread.currentThread().interrupt();
    }
}).start();
```

В этом примере один поток записывает логи в буфер, а другой поток считывает и обрабатывает их. Это позволяет безопасно передавать сообщения между потоками без блокировок.
