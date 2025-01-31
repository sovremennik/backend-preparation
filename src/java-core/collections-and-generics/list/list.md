#### [Сравнение ArrayList, LinkedList и Vector](#Сравнение-arraylist-linkedlist-и-vector-1)
#### [Примеры использования в реальных задачах](#Примеры-использования-в-реальных-задачах-1)

---
# List и его реализации

## Сравнение ArrayList, LinkedList и Vector

### ArrayList
- **Структура данных:** динамический массив.
- **Доступ к элементам:** быстрый (O(1)) за счёт индексации.
- **Вставка/удаление:** медленные (O(n)) из-за необходимости сдвига элементов.
- **Потокобезопасность:** не потокобезопасен.
- **Использование:** предпочтителен при частом доступе к элементам и редких изменениях размера.

### LinkedList
- **Структура данных:** двусвязный список.
- **Доступ к элементам:** медленный (O(n)) из-за последовательного обхода.
- **Вставка/удаление:** быстрые (O(1)) при добавлении/удалении с начала или конца.
- **Потокобезопасность:** не потокобезопасен.
- **Использование:** подходит для сценариев с частыми вставками и удалениями.

### Vector
- **Структура данных:** динамический массив.
- **Доступ к элементам:** быстрый (O(1)).
- **Вставка/удаление:** медленные (O(n)).
- **Потокобезопасность:** синхронизирован (потокобезопасен).
- **Использование:** устаревший, рекомендуется заменять на `ArrayList` с внешней синхронизацией.

## Примеры использования в реальных задачах

### ArrayList — Кэширование результатов запросов
```java
import java.util.ArrayList;

public class ProductCache {
    private final ArrayList<String> productCache = new ArrayList<>();

    public void addProduct(String product) {
        productCache.add(product);
    }

    public String getProduct(int index) {
        return productCache.get(index);
    }
}
```
*Используется для кэширования товаров в интернет-магазине, где важен быстрый доступ по индексу.*

### LinkedList — Управление задачами в очереди
```java
import java.util.LinkedList;

public class TaskQueue {
    private final LinkedList<String> tasks = new LinkedList<>();

    public void addTask(String task) {
        tasks.addLast(task);
    }

    public String getNextTask() {
        return tasks.pollFirst();
    }
}
```
*Используется для управления задачами в системе обработки заказов, где важна быстрая вставка и удаление.*

### Vector — Логирование событий в многопоточном приложении
```java
import java.util.Vector;

public class LogService {
    private final Vector<String> logs = new Vector<>();

    public synchronized void logEvent(String event) {
        logs.add(event);
    }

    public synchronized void printLogs() {
        for (String log : logs) {
            System.out.println(log);
        }
    }
}
```
*Используется для логирования в многопоточном приложении, где важна потокобезопасность.*
