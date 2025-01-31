#### [Преимущества и недостатки HashSet, LinkedHashSet, и TreeSet](#Преимущества-и-недостатки-hashset-linkedhashset-и-treeset-1)
#### [Применение в фильтрации данных](#Применение-в-фильтрации-данных-1)

---
# Set и разрешение дублирования

`Set` — это коллекция, которая хранит уникальные элементы без дубликатов. В зависимости от реализации, может сохранять порядок добавления или поддерживать сортировку.

## Преимущества и недостатки HashSet, LinkedHashSet и TreeSet

### HashSet
**Преимущества:**
- Быстрая работа операций добавления, удаления и поиска (в среднем O(1)).
- Подходит для хранения больших объёмов данных, где порядок не важен.

**Недостатки:**
- Не сохраняет порядок добавления элементов.
- Уязвим к некачественной реализации методов `hashCode()` и `equals()` у объектов, что может привести к коллизиям.

```java
// Кэширование ID пользователей для быстрого доступа
Set<Integer> userCache = new HashSet<>();
userCache.add(fetchUserId("user1"));
userCache.add(fetchUserId("user2"));

if (userCache.contains(fetchUserId("user3"))) {
    System.out.println("Пользователь уже в кэше.");
}
```

### LinkedHashSet
**Преимущества:**
- Сохраняет порядок добавления элементов.
- Подходит для реализации кэшей, где важен порядок.

**Недостатки:**
- Немного медленнее, чем `HashSet`, из-за дополнительного хранения порядка.
- Потребляет больше памяти.

```java
// Реализация LRU-кэша
Set<String> recentFiles = new LinkedHashSet<>();
void accessFile(String fileName) {
    recentFiles.remove(fileName);
    recentFiles.add(fileName);
    if (recentFiles.size() > 10) {
        Iterator<String> it = recentFiles.iterator();
        it.next();
        it.remove();
    }
}
```

### TreeSet
**Преимущества:**
- Элементы автоматически хранятся в отсортированном порядке.
- Поддерживает быстрый поиск наибольшего или наименьшего элемента.

**Недостатки:**
- Операции работают медленнее (O(log n)) по сравнению с `HashSet`.
- Требует реализации `Comparable` или передачи `Comparator`.

```java
// Система бронирования: хранение доступных временных слотов
Set<LocalTime> availableSlots = new TreeSet<>();
availableSlots.add(LocalTime.of(9, 0));
availableSlots.add(LocalTime.of(11, 0));
availableSlots.add(LocalTime.of(10, 0));

LocalTime nextAvailable = ((TreeSet<LocalTime>) availableSlots).higher(LocalTime.of(9, 30));
System.out.println("Следующий доступный слот: " + nextAvailable);
```

## Применение в фильтрации данных

### Удаление дубликатов из коллекции
```java
// Очистка списка email-адресов от дубликатов
List<String> emails = fetchUserEmails();
Set<String> uniqueEmails = new HashSet<>(emails);
sendNewsletter(uniqueEmails);
```

### Сохранение порядка при фильтрации
```java
// Логирование уникальных ошибок в порядке их появления
List<String> errorLogs = getErrorLogs();
Set<String> uniqueErrors = new LinkedHashSet<>(errorLogs);
uniqueErrors.forEach(System.out::println);
```

### Хранение уникальных отсортированных данных
```java
// Хранение уникальных тегов в отсортированном виде
Set<String> tags = new TreeSet<>(Arrays.asList("backend", "java", "api", "java"));
System.out.println(tags); // [api, backend, java]
```

### Фильтрация с условием (например, числа больше 10)
```java
// Отбор транзакций выше определённого порога
List<Integer> transactions = getTransactionAmounts();
Set<Integer> largeTransactions = transactions.stream()
    .filter(amount -> amount > 1000)
    .collect(Collectors.toSet());
alertFinanceTeam(largeTransactions);
```
