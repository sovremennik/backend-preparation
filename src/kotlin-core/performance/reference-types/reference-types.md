#### [Введение](#Введение-1)
#### [Сильные ссылки (Strong References)](#Сильные-ссылки-strong-references-1)
#### [Слабые ссылки (Weak References)](#Слабые-ссылки-weak-references-1)
#### [Фантомные ссылки (Phantom References)](#Фантомные-ссылки-phantom-references-1)
#### [Сравнение типов ссылок](#Сравнение-типов-ссылок-1)
#### [Best Practices](#best-practices-1)
#### [Антипаттерны](#Антипаттерны-1)

---
# Типы ссылок (weak, strong)

## Введение

Правильное управление ссылками в Kotlin и JVM позволяет избежать утечек памяти, оптимизировать использование ресурсов и снизить нагрузку на сборщик мусора (GC). В этой теме разберём основные типы ссылок: **сильные (strong)**, **слабые (weak)** и **фантомные (phantom)**, их применение, влияние на сборку мусора, а также рассмотрим нюансы работы в многопоточной среде.

## Сильные ссылки (Strong References)

**Сильные ссылки** — это обычные ссылки на объекты, предотвращающие их удаление сборщиком мусора (GC), пока на них есть хотя бы одна ссылка. Это основной тип ссылок в Kotlin.

### Пример
```kotlin
class Service(val name: String)

fun main() {
    val service = Service("Database") // Сильная ссылка на объект Service
    println(service.name) // Объект остаётся в памяти, пока существует переменная service
}
```

### Потенциальная проблема

Если объекты хранятся в глобальных структурах данных, таких как `Map` или `List`, они могут не освобождаться автоматически, что приводит к утечкам памяти.

```kotlin
val cache = mutableMapOf<String, Service>()
fun loadService(name: String): Service {
    return cache.getOrPut(name) { Service(name) }
}
```
*Этот код может привести к утечке памяти, если элементы не удаляются из `cache`.*

### Работа с многопоточностью

Если использовать сильные ссылки в многопоточной среде, необходимо учитывать, что объекты, хранящие ссылки, могут быть недоступны другим потокам без синхронизации:

```kotlin
@Volatile private var serviceInstance: Service? = null

fun getService(): Service {
    return serviceInstance ?: synchronized(this) {
        serviceInstance ?: Service("Main").also { serviceInstance = it }
    }
}
```

## Слабые ссылки (Weak References)

**Слабые ссылки (WeakReference)** позволяют GC удалить объект, если на него нет сильных ссылок. Используются в кэшах, слушателях событий и других сценариях, где не нужно удерживать объект в памяти, если он больше не используется.

### Пример использования `WeakReference`
```kotlin
import java.lang.ref.WeakReference

class LargeData

fun main() {
    val data = LargeData()
    val weakRef = WeakReference(data)
    println(weakRef.get()) // Объект доступен

    // Теперь удаляем сильную ссылку
    System.gc()
    println(weakRef.get()) // Может быть null, если GC удалил объект
}
```

### Использование в кешировании

```kotlin
import java.lang.ref.WeakReference

class ExpensiveResource

val cache = mutableMapOf<String, WeakReference<ExpensiveResource>>()

fun getResource(key: String): ExpensiveResource {
    return cache[key]?.get() ?: ExpensiveResource().also {
        cache[key] = WeakReference(it)
    }
}
```

## Фантомные ссылки (Phantom References)

**Фантомные ссылки (PhantomReference)** применяются для управления освобождением ресурсов перед удалением объекта. Они всегда возвращают `null`, поэтому используются с `ReferenceQueue`. Их основной плюс — возможность контролировать момент окончательного удаления объекта.

### Пример работы с `PhantomReference`
```kotlin
import java.lang.ref.PhantomReference
import java.lang.ref.ReferenceQueue

class Resource

fun main() {
    val queue = ReferenceQueue<Resource>()
    val resource = Resource()
    val phantomRef = PhantomReference(resource, queue)
    
    println(phantomRef.get()) // Всегда null
    System.gc()
    println(queue.poll() != null) // true, если объект удалён
}
```

### Применение фантомных ссылок

Фантомные ссылки часто используются в библиотеках для работы с большими объектами, такими как `ByteBuffer`, в целях освобождения памяти или других ресурсов перед удалением объекта.

```kotlin
import java.nio.ByteBuffer
import java.lang.ref.PhantomReference
import java.lang.ref.ReferenceQueue

class ManagedBuffer(size: Int) {
    private val buffer: ByteBuffer = ByteBuffer.allocateDirect(size)
}

fun main() {
    val queue = ReferenceQueue<ManagedBuffer>()
    val buffer = ManagedBuffer(1024 * 1024)
    val phantomRef = PhantomReference(buffer, queue)
    System.gc()
    println(queue.poll() != null) // Проверяем, что объект удалён
}
```

## Сравнение типов ссылок

| Тип ссылки   | GC может удалить объект? | Использование |
|-------------|-------------------------|--------------|
| Strong      | ❌ Нет                    | Обычные переменные, основные объекты |
| Weak        | ✅ Да                     | Кэши, слушатели событий |
| Phantom     | ✅ Да                     | Освобождение ресурсов перед удалением |

## Best Practices

- Использовать `WeakReference` для кэшей и временных объектов, чтобы предотвратить утечки памяти.
- Очищать коллекции, содержащие сильные ссылки, если объект больше не нужен.
- Применять `PhantomReference` для точного контроля освобождения ресурсов, например, в библиотеках обработки больших данных.
- В многопоточных приложениях учитывать, что слабые ссылки могут быть собраны GC в любой момент, и необходимо проверять их наличие перед использованием.

## Антипаттерны

- **Ненужное использование слабых ссылок**: приводит к потере данных, если GC удаляет объект раньше времени.
- **Пренебрежение очисткой сильных ссылок**: удерживает ненужные объекты в памяти, создавая утечки.
- **Использование слабых ссылок для критичных данных**: слабые ссылки не гарантируют, что объект будет доступен, поэтому их не стоит использовать для важных компонентов приложения.

```kotlin
val cache = mutableMapOf<String, WeakReference<Service>>() // ❌ Неоптимально: данные могут теряться
```
