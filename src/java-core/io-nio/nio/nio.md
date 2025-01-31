#### [Основные особенности NIO](#Основные-особенности-nio-1)
#### [Основные компоненты NIO](#Основные-компоненты-nio-1)
#### [Достоинства NIO](#Достоинства-nio-1)
#### [Ограничения NIO](#Ограничения-nio-1)
#### [Когда использовать NIO](#Когда-использовать-nio-1)

---
# NIO

NIO (“New Input/Output”, “Non-blocking I/O”) — это API, представленное в Java начиная с версии 1.4, для работы с неблокирующим вводом-выводом. Оно дополняет классическое IO, предоставляя более гибкие и высокопроизводительные инструменты для работы с потоками данных.


## Основные особенности NIO

1. **Неблокирующий ввод-вывод**
    - Методы чтения и записи не блокируют поток, что позволяет обрабатывать большое количество соединений эффективно.

2. **Буферы (Buffers)**
    - В NIO данные читаются и записываются через буферы, а не через потоки.
    - Классы буферов предоставляют методы для работы с данными: запись, чтение, переключение режимов (write/read).

3. **Каналы (Channels)**
    - Каналы являются двунаправленными, что позволяет читать и записывать данные через один объект.
    - В отличие от потоков, каналы поддерживают асинхронные операции.

4. **Селекторы (Selectors)**
    - Позволяют одному потоку управлять несколькими каналами, что существенно увеличивает производительность.

---

## Основные компоненты NIO

### Буферы

- **Buffer** — основной класс для работы с данными.

  Основные типы:
    - `ByteBuffer` — для работы с байтами.
    - `CharBuffer` — для работы с символами.
    - `IntBuffer`, `LongBuffer`, `FloatBuffer`, `DoubleBuffer` — для работы с числовыми типами.

**Пример использования буфера:**
```java
ByteBuffer buffer = ByteBuffer.allocate(1024);

// Запись данных в буфер
buffer.put("Hello, NIO!".getBytes());

// Переключение в режим чтения
buffer.flip();

// Чтение данных из буфера
while (buffer.hasRemaining()) {
    System.out.print((char) buffer.get());
}
```

### Каналы

- Каналы — интерфейсы для работы с потоками данных. Основные реализации:
    - `FileChannel` — для работы с файлами.
    - `SocketChannel` — для работы с сокетами.
    - `ServerSocketChannel` — для серверных сокетов.
    - `DatagramChannel` — для работы с UDP.

**Пример чтения файла с использованием FileChannel:**
```java
try (FileChannel fileChannel = FileChannel.open(Paths.get("example.txt"), StandardOpenOption.READ)) {
    ByteBuffer buffer = ByteBuffer.allocate(1024);
    while (fileChannel.read(buffer) > 0) {
        buffer.flip();
        while (buffer.hasRemaining()) {
            System.out.print((char) buffer.get());
        }
        buffer.clear();
    }
} catch (IOException e) {
    e.printStackTrace();
}
```

### Селекторы

- Селекторы позволяют одному потоку обрабатывать несколько каналов одновременно.
- Основной класс: `Selector`.

**Пример работы с Selector:**
```java
try (Selector selector = Selector.open();
     ServerSocketChannel serverChannel = ServerSocketChannel.open()) {

    serverChannel.bind(new InetSocketAddress(8080));
    serverChannel.configureBlocking(false);
    serverChannel.register(selector, SelectionKey.OP_ACCEPT);

    while (true) {
        selector.select();
        Set<SelectionKey> selectedKeys = selector.selectedKeys();
        Iterator<SelectionKey> iterator = selectedKeys.iterator();

        while (iterator.hasNext()) {
            SelectionKey key = iterator.next();
            if (key.isAcceptable()) {
                SocketChannel clientChannel = serverChannel.accept();
                clientChannel.configureBlocking(false);
                clientChannel.register(selector, SelectionKey.OP_READ);
            } else if (key.isReadable()) {
                SocketChannel clientChannel = (SocketChannel) key.channel();
                ByteBuffer buffer = ByteBuffer.allocate(256);
                clientChannel.read(buffer);
                System.out.println("Received: " + new String(buffer.array()).trim());
            }
            iterator.remove();
        }
    }
} catch (IOException e) {
    e.printStackTrace();
}
```

---

## Достоинства NIO

1. **Высокая производительность**
    - NIO подходит для приложений с большим количеством соединений (например, серверов).

2. **Гибкость**
    - Поддержка работы с файлами, сетевыми соединениями и другими источниками данных.

3. **Асинхронность**
    - Возможность обработки данных без блокировки потоков.

---

## Ограничения NIO

1. **Сложность**
    - Код с использованием NIO может быть более сложным и трудным для отладки.

2. **Поддержка старых JVM**
    - Для использования всех возможностей NIO может потребоваться современная версия JVM.

---

## Когда использовать NIO

- Высоконагруженные серверы.
- Приложения, требующие асинхронного ввода-вывода.
- Работа с большими объемами данных в условиях ограниченных ресурсов.

