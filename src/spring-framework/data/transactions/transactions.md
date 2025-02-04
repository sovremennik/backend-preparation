#### [Введение](#Введение-1)
#### [Теоретическая база](#Теоретическая-база-1)
#### [Практическая часть](#Практическая-часть-1)
#### [Типичные ошибки и решения](#Типичные-ошибки-и-решения-1)
#### [Рекомендации и Best Practices](#Рекомендации-и-Best-Practices-1)
#### [Дополнительные материалы](#Дополнительные-материалы-1)
#### [Заключение](#Заключение-1)

---

## Введение

### 1.1. Зачем это нужно?
Управление транзакциями — это ключевой аспект разработки enterprise-приложений, особенно в финансовых системах, где важна целостность данных и гарантия выполнения операций "все или ничего". Spring Framework предоставляет мощные инструменты для управления транзакциями через декларативный (`@Transactional`) и программный подход.

Пример применения: В банках и платежных системах транзакции используются для обеспечения надежности операций, таких как перевод средств между счетами или создание новых транзакций.

---

## Теоретическая база

### 2.1. Что такое транзакции?

Транзакция — это логически завершенная единица работы с базой данных, которая должна удовлетворять принципам **ACID**:
- **Atomicity (Атомарность)**: Все операции внутри транзакции либо выполняются полностью, либо не выполняются вообще.
- **Consistency (Согласованность)**: Транзакция приводит базу данных от одного корректного состояния к другому.
- **Isolation (Изолированность)**: Транзакции не должны влиять друг на друга во время выполнения.
- **Durability (Прочность)**: После завершения транзакции изменения сохраняются в базе данных навсегда.

---

### 2.2. Как работает управление транзакциями в Spring?

Spring предоставляет два основных подхода к управлению транзакциями:
1. **Декларативное управление**: Через аннотацию `@Transactional`.
2. **Программное управление**: Через объект `TransactionTemplate` или интерфейс `PlatformTransactionManager`.

Основные компоненты:
- **Transaction Manager**: Управляет началом, коммитом и откатом транзакций.
- **Propagation**: Определяет поведение транзакции (например, создание новой или присоединение к существующей).
- **Isolation**: Устанавливает уровень изоляции для транзакции.

---

### 2.3. Параметры транзакций

#### 2.3.1. Propagation
Определяет, как должна вести себя транзакция при вызове методов.

| Значение              | Описание                                                                 |
|-----------------------|--------------------------------------------------------------------------|
| `REQUIRED` (по умолчанию) | Если транзакция существует, присоединяется к ней; если нет, создается новая. |
| `REQUIRES_NEW`        | Создает новую транзакцию, даже если текущая уже существует.               |
| `SUPPORTS`            | Выполняет работу в рамках существующей транзакции, если она есть.         |
| `MANDATORY`           | Требует существующей транзакции; выбрасывает исключение, если ее нет.     |
| `NOT_SUPPORTED`       | Отключает транзакцию для данного метода.                                  |
| `NEVER`               | Запрещает выполнение метода в рамках транзакции.                         |

#### 2.3.2. Isolation
Устанавливает уровень изоляции для транзакции.

| Значение              | Описание                                                                 |
|-----------------------|--------------------------------------------------------------------------|
| `DEFAULT` (по умолчанию) | Использует уровень изоляции, установленный базой данных.                |
| `READ_UNCOMMITTED`     | Разрешает чтение незавершенных изменений других транзакций.                |
| `READ_COMMITTED`       | Разрешает чтение только завершенных изменений других транзакций.           |
| `REPEATABLE_READ`      | Гарантирует, что повторное чтение одних и тех же данных даст одинаковый результат. |
| `SERIALIZABLE`         | Наивысший уровень изоляции; предотвращает все виды конкуренции.          |

#### 2.3.3. Timeout
Устанавливает максимальное время выполнения транзакции. Если транзакция не завершится за указанное время, она будет отменена.

#### 2.3.4. Rollback Rules
Определяет, при каких исключениях транзакция должна быть отменена.

---

## Практическая часть

### 3.1. Базовые примеры

#### Пример 1: Декларативное управление (`@Transactional`)

**Код:**
```java
@Service
public class TransactionService {

    @Autowired
    private AccountRepository accountRepository;

    /**
     * Метод для перевода средств между счетами.
     * Аннотация @Transactional гарантирует, что все операции внутри метода выполняются как одна транзакция.
     * Если возникает исключение, все изменения откатываются.
     *
     * @param fromAccountId ID отправителя
     * @param toAccountId   ID получателя
     * @param amount        Сумма перевода
     */
    @Transactional // Все операции внутри метода выполняются как одна транзакция
    public void transferFunds(Long fromAccountId, Long toAccountId, double amount) {
        // Находим отправителя
        Account fromAccount = accountRepository.findById(fromAccountId).orElseThrow(() -> 
            new AccountNotFoundException("Account not found with ID: " + fromAccountId));

        // Находим получателя
        Account toAccount = accountRepository.findById(toAccountId).orElseThrow(() -> 
            new AccountNotFoundException("Account not found with ID: " + toAccountId));

        // Проверяем баланс отправителя
        if (fromAccount.getBalance() < amount) {
            throw new InsufficientFundsException("Not enough funds on account: " + fromAccountId);
        }

        // Выполняем перевод
        fromAccount.setBalance(fromAccount.getBalance() - amount); // Уменьшаем баланс отправителя
        toAccount.setBalance(toAccount.getBalance() + amount); // Увеличиваем баланс получателя

        // Сохраняем изменения в базе данных
        accountRepository.save(fromAccount);
        accountRepository.save(toAccount);

        // Simulate an error (для тестирования rollback)
        if (amount > 10000) {
            throw new FraudDetectionException("Large transaction detected");
        }
    }
}
```

**Пояснение:**
1. **Аннотация `@Transactional`**: Обеспечивает транзакционность метода. Если возникает исключение, все изменения откатываются.
2. **Поиск счетов**: Используется `accountRepository.findById()` для получения счетов отправителя и получателя. Если счет не найден, выбрасывается исключение `AccountNotFoundException`.
3. **Проверка баланса**: Перед переводом проверяется, достаточно ли средств на счете отправителя.
4. **Сохранение изменений**: После успешного перевода изменения сохраняются в базе данных через `accountRepository.save()`.
5. **Simulated Error**: Для тестирования rollback добавлено условие, которое выбрасывает исключение при переводе суммы больше 10,000.

---

#### Пример 2: Программное управление (`TransactionTemplate`)

**Код:**
```java
@Service
public class TransactionService {

    @Autowired
    private AccountRepository accountRepository;

    @Autowired
    private TransactionTemplate transactionTemplate;

    /**
     * Метод для перевода средств между счетами с использованием TransactionTemplate.
     * TransactionTemplate позволяет программно управлять транзакцией.
     *
     * @param fromAccountId ID отправителя
     * @param toAccountId   ID получателя
     * @param amount        Сумма перевода
     */
    public void transferFunds(Long fromAccountId, Long toAccountId, double amount) {
        transactionTemplate.execute(status -> {
            try {
                // Находим отправителя
                Account fromAccount = accountRepository.findById(fromAccountId).orElseThrow(() -> 
                    new AccountNotFoundException("Account not found with ID: " + fromAccountId));

                // Находим получателя
                Account toAccount = accountRepository.findById(toAccountId).orElseThrow(() -> 
                    new AccountNotFoundException("Account not found with ID: " + toAccountId));

                // Проверяем баланс отправителя
                if (fromAccount.getBalance() < amount) {
                    throw new InsufficientFundsException("Not enough funds on account: " + fromAccountId);
                }

                // Выполняем перевод
                fromAccount.setBalance(fromAccount.getBalance() - amount); // Уменьшаем баланс отправителя
                toAccount.setBalance(toAccount.getBalance() + amount); // Увеличиваем баланс получателя

                // Сохраняем изменения в базе данных
                accountRepository.save(fromAccount);
                accountRepository.save(toAccount);

                // Simulate an error (для тестирования rollback)
                if (amount > 10000) {
                    throw new FraudDetectionException("Large transaction detected");
                }

                return null; // Транзакция успешно завершена
            } catch (Exception ex) {
                status.setRollbackOnly(); // Откатываем транзакцию при ошибке
                throw ex;
            }
        });
    }
}
```

**Пояснение:**
1. **TransactionTemplate**: Позволяет программно управлять транзакцией. Полезно в случаях, когда нужно более гибкое управление.
2. **status.setRollbackOnly()**: Явно указывает, что транзакция должна быть отменена при возникновении ошибки.
3. **Try-Catch Block**: Ловит все исключения внутри транзакции и выполняет откат, если необходимо.

---

### 3.2. Реальные кейсы

#### Сценарий 1: Перевод средств между счетами с логированием

**Код:**
```java
@Service
public class BankService {

    @Autowired
    private AccountRepository accountRepository;

    @Autowired
    private TransactionLogRepository logRepository;

    /**
     * Метод для перевода средств между счетами с логированием операции.
     * Если возникает исключение, все изменения откатываются, но лог создается вне транзакции.
     *
     * @param fromAccountId ID отправителя
     * @param toAccountId   ID получателя
     * @param amount        Сумма перевода
     */
    @Transactional
    public void transferFundsWithLogging(Long fromAccountId, Long toAccountId, double amount) {
        try {
            // Начинаем транзакцию
            Account fromAccount = accountRepository.findById(fromAccountId).orElseThrow(() -> 
                new AccountNotFoundException("Account not found with ID: " + fromAccountId));

            Account toAccount = accountRepository.findById(toAccountId).orElseThrow(() -> 
                new AccountNotFoundException("Account not found with ID: " + toAccountId));

            if (fromAccount.getBalance() < amount) {
                throw new InsufficientFundsException("Not enough funds on account: " + fromAccountId);
            }

            // Выполняем перевод
            fromAccount.setBalance(fromAccount.getBalance() - amount);
            toAccount.setBalance(toAccount.getBalance() + amount);

            // Сохраняем изменения
            accountRepository.save(fromAccount);
            accountRepository.save(toAccount);

        } catch (Exception ex) {
            // Создаем лог ошибки вне транзакции
            TransactionLog log = new TransactionLog(fromAccountId, toAccountId, amount, "FAILED", ex.getMessage());
            logRepository.save(log); // Лог сохраняется даже при ошибке
            throw ex; // Перебрасываем исключение дальше
        }

        // Создаем лог успеха
        TransactionLog log = new TransactionLog(fromAccountId, toAccountId, amount, "SUCCESS", "Transaction completed");
        logRepository.save(log); // Лог сохраняется после успешного завершения транзакции
    }
}
```

**Пояснение:**
1. **Логирование ошибок**: Если перевод не удается (например, недостаточно средств), создается лог ошибки вне транзакции.
2. **Логирование успеха**: После успешного перевода создается лог успеха.
3. **Изолированность**: Изменения балансов и логи хранятся в разных таблицах, что обеспечивает изолированность данных.

---

#### Сценарий 2: Распределенная транзакция между двумя базами данных

**Код:**
```java
@Service
public class DistributedTransactionService {

    @Autowired
    private AccountRepository accountRepository;

    @Autowired
    private ExternalPaymentService externalPaymentService;

    @Transactional // Основная транзакция
    public void processPayment(Long accountId, double amount) {
        // Находим счет
        Account account = accountRepository.findById(accountId).orElseThrow(() -> 
            new AccountNotFoundException("Account not found with ID: " + accountId));

        // Проверяем баланс
        if (account.getBalance() < amount) {
            throw new InsufficientFundsException("Not enough funds on account: " + accountId);
        }

        // Уменьшаем баланс
        account.setBalance(account.getBalance() - amount);
        accountRepository.save(account);

        // Выполняем внешний платеж
        boolean paymentSuccess = externalPaymentService.processPayment(accountId, amount);

        // Если внешний платеж не удался, выбрасываем исключение
        if (!paymentSuccess) {
            throw new PaymentFailedException("External payment failed for account: " + accountId);
        }
    }
}
```

**Пояснение:**
1. **Основная транзакция**: Управляет изменениями в локальной базе данных.
2. **Внешний сервис**: Выполняет платеж в другой системе (например, платежный шлюз).
3. **Откат при ошибке**: Если внешний платеж не удается, основная транзакция откатывается, чтобы сохранить целостность данных.

---

#### Сценарий 3: Параллельные транзакции с `REQUIRES_NEW`

**Код:**
```java
@Service
public class ConcurrentTransactionService {

    @Autowired
    private AccountRepository accountRepository;

    @Autowired
    private AuditLogRepository auditLogRepository;

    /**
     * Метод для выполнения параллельных транзакций.
     * Вторая транзакция (`auditTransaction`) создается независимо от первой.
     *
     * @param accountId ID счета
     * @param amount    Сумма пополнения
     */
    @Transactional
    public void depositAndAudit(Long accountId, double amount) {
        // Основная транзакция
        Account account = accountRepository.findById(accountId).orElseThrow(() -> 
            new AccountNotFoundException("Account not found with ID: " + accountId));

        account.setBalance(account.getBalance() + amount);
        accountRepository.save(account);

        // Создаем новую транзакцию для аудита
        auditTransaction(accountId, amount);
    }

    /**
     * Метод для создания записи аудита.
     * Используется propagation = REQUIRES_NEW, чтобы создать независимую транзакцию.
     *
     * @param accountId ID счета
     * @param amount    Сумма операции
     */
    @Transactional(propagation = Propagation.REQUIRES_NEW)
    public void auditTransaction(Long accountId, double amount) {
        AuditLog log = new AuditLog(accountId, amount, "DEPOSIT");
        auditLogRepository.save(log); // Запись аудита
    }
}
```

**Пояснение:**
1. **Основная транзакция**: Управляет изменением баланса счета.
2. **Новая транзакция (`REQUIRES_NEW`)**: Создает запись аудита независимо от основной транзакции.
3. **Преимущества**: Если основная транзакция откатывается, запись аудита остается в базе данных.

---

#### Сценарий 4: Работа с таймаутами

**Код:**
```java
@Service
public class TimeoutTransactionService {

    @Autowired
    private AccountRepository accountRepository;

    /**
     * Метод для выполнения операции с таймаутом.
     * Если операция занимает более 5 секунд, она откатывается.
     *
     * @param accountId ID счета
     * @param amount    Сумма операции
     */
    @Transactional(timeout = 5) // Таймаут 5 секунд
    public void updateAccountWithTimeout(Long accountId, double amount) {
        // Находим счет
        Account account = accountRepository.findById(accountId).orElseThrow(() -> 
            new AccountNotFoundException("Account not found with ID: " + accountId));

        // Моделируем долгую операцию
        try {
            Thread.sleep(6000); // Задержка 6 секунд
        } catch (InterruptedException e) {
            Thread.currentThread().interrupt();
        }

        // Обновляем баланс
        account.setBalance(account.getBalance() + amount);
        accountRepository.save(account);
    }
}
```

**Пояснение:**
1. **Timeout**: Установлен таймаут в 5 секунд. Если операция занимает больше времени, транзакция откатывается.
2. **Thread.sleep()**: Имитирует долгую операцию (например, вызов внешнего API).
3. **Откат при превышении таймаута**: Spring автоматически откатывает транзакцию, если она не завершилась за указанное время.

---

#### Сценарий 5: Уровень изоляции `SERIALIZABLE`

**Код:**
```java
@Service
public class SerializableTransactionService {

    @Autowired
    private AccountRepository accountRepository;

    /**
     * Метод для чтения данных с уровнем изоляции SERIALIZABLE.
     * SERIALIZABLE предотвращает любые виды конкуренции между транзакциями.
     *
     * @param accountId ID счета
     * @return Информация о счете
     */
    @Transactional(isolation = Isolation.SERIALIZABLE)
    public Account getAccountWithSerializableIsolation(Long accountId) {
        // Читаем данные с уровнем изоляции SERIALIZABLE
        return accountRepository.findById(accountId).orElseThrow(() -> 
            new AccountNotFoundException("Account not found with ID: " + accountId));
    }
}
```

**Пояснение:**
1. **Isolation.SERIALIZABLE**: Наивысший уровень изоляции, который предотвращает все виды конкуренции (dirty reads, non-repeatable reads, phantom reads).
2. **Применение**: Используется в критических сценариях, где важно получить согласованные данные без влияния других транзакций.

---

### 3.3. Пример обработки исключений

#### Код:
```java
@Service
public class ExceptionHandlingService {

    @Autowired
    private AccountRepository accountRepository;

    /**
     * Метод для перевода средств с обработкой исключений.
     * Если возникает исключение, транзакция откатывается.
     *
     * @param fromAccountId ID отправителя
     * @param toAccountId   ID получателя
     * @param amount        Сумма перевода
     */
    @Transactional(rollbackFor = {InsufficientFundsException.class, FraudDetectionException.class})
    public void transferFundsWithExceptionHandling(Long fromAccountId, Long toAccountId, double amount) {
        // Находим отправителя
        Account fromAccount = accountRepository.findById(fromAccountId).orElseThrow(() -> 
            new AccountNotFoundException("Account not found with ID: " + fromAccountId));

        // Находим получателя
        Account toAccount = accountRepository.findById(toAccountId).orElseThrow(() -> 
            new AccountNotFoundException("Account not found with ID: " + toAccountId));

        // Проверяем баланс
        if (fromAccount.getBalance() < amount) {
            throw new InsufficientFundsException("Not enough funds on account: " + fromAccountId);
        }

        // Выполняем перевод
        fromAccount.setBalance(fromAccount.getBalance() - amount);
        toAccount.setBalance(toAccount.getBalance() + amount);

        // Сохраняем изменения
        accountRepository.save(fromAccount);
        accountRepository.save(toAccount);

        // Моделируем ошибку
        if (amount > 10000) {
            throw new FraudDetectionException("Large transaction detected");
        }
    }
}
```

**Пояснение:**
1. **rollbackFor**: Указывает, при каких исключениях должна происходить отмена транзакции.
2. **InsufficientFundsException**: Исключение, которое выбрасывается, если недостаточно средств на счете.
3. **FraudDetectionException**: Исключение, которое выбрасывается при подозрительных операциях.

---

### 3.4. Пример использования `Propagation.REQUIRES_NEW`

#### Код:
```java
@Service
public class NestedTransactionService {

    @Autowired
    private AccountRepository accountRepository;

    @Autowired
    private AuditLogRepository auditLogRepository;

    /**
     * Метод для выполнения перевода с созданием записи аудита.
     * Запись аудита создается в независимой транзакции.
     *
     * @param fromAccountId ID отправителя
     * @param toAccountId   ID получателя
     * @param amount        Сумма перевода
     */
    @Transactional
    public void transferWithAudit(Long fromAccountId, Long toAccountId, double amount) {
        // Основная транзакция
        Account fromAccount = accountRepository.findById(fromAccountId).orElseThrow(() -> 
            new AccountNotFoundException("Account not found with ID: " + fromAccountId));

        Account toAccount = accountRepository.findById(toAccountId).orElseThrow(() -> 
            new AccountNotFoundException("Account not found with ID: " + toAccountId));

        if (fromAccount.getBalance() < amount) {
            throw new InsufficientFundsException("Not enough funds on account: " + fromAccountId);
        }

        // Выполняем перевод
        fromAccount.setBalance(fromAccount.getBalance() - amount);
        toAccount.setBalance(toAccount.getBalance() + amount);

        // Сохраняем изменения
        accountRepository.save(fromAccount);
        accountRepository.save(toAccount);

        // Создаем запись аудита в новой транзакции
        createAuditLog(fromAccountId, toAccountId, amount);
    }

    /**
     * Метод для создания записи аудита.
     * Используется propagation = REQUIRES_NEW для создания независимой транзакции.
     *
     * @param fromAccountId ID отправителя
     * @param toAccountId   ID получателя
     * @param amount        Сумма операции
     */
    @Transactional(propagation = Propagation.REQUIRES_NEW)
    public void createAuditLog(Long fromAccountId, Long toAccountId, double amount) {
        AuditLog log = new AuditLog(fromAccountId, toAccountId, amount, "TRANSFER");
        auditLogRepository.save(log); // Запись аудита
    }
}
```

**Пояснение:**
1. **Основная транзакция**: Управляет переводом средств между счетами.
2. **Новая транзакция (`REQUIRES_NEW`)**: Создает запись аудита независимо от основной транзакции.
3. **Преимущество**: Если основная транзакция откатывается, запись аудита все равно сохраняется.

---

## Типичные ошибки и решения

### 4.1. Распространенные ошибки

1. **Отсутствие аннотации `@Transactional`**:
    - Проблема: Изменения не сохраняются в базе данных.
    - Решение: Убедитесь, что методы, работающие с данными, помечены как `@Transactional`.

2. **Неправильная настройка rollback**:
    - Проблема: Транзакция не откатывается при возникновении определенных исключений.
    - Решение: Используйте параметр `rollbackFor` для явного указания исключений.

3. **Проблемы с propagation**:
    - Проблема: Несколько транзакций пересекаются, вызывая несогласованность данных.
    - Решение: Настройте правильную стратегию propagation (например, `REQUIRES_NEW` для критичных операций).

---

## Рекомендации и Best Practices

1. **Используйте декларативный подход (`@Transactional`)**:
    - Это делает код более чистым и понятным.

2. **Настройте timeout для долгих операций**:
    - Например, для сложных расчетов или внешних вызовов API.

3. **Избегайте вложенных транзакций без необходимости**:
    - Вместо этого используйте параметр `propagation`.

4. **Логируйте ошибки транзакций**:
    - Это поможет быстро находить проблемы в production.

5. **Используйте тесты для проверки транзакционности**:
    - Напишите unit-тесты с имитацией исключений.

---

## Дополнительные материалы

### 6.1. Глоссарий

- **ACID**: Принципы транзакций (Atomicity, Consistency, Isolation, Durability).
- **Propagation**: Поведение транзакции при вызове методов.
- **Isolation**: Уровень изоляции транзакций.
- **Timeout**: Максимальное время выполнения транзакции.

### 6.2. Полезные ссылки

- [Spring Transaction Management Documentation](https://docs.spring.io/spring-framework/docs/current/reference/html/data-access.html#transaction)
- [ACID Properties in Transactions](https://en.wikipedia.org/wiki/ACID)
- [Best Practices for Transaction Management](https://spring.io/blog/2011/08/15/configuring-spring-and-jta-without-full-java-ee/)

---

## Заключение

### 7.1. Итоги
Управление транзакциями в Spring является критически важным для создания надежных enterprise-приложений. Финансовые системы особенно требовательны к целостности данных, поэтому правильная настройка транзакций гарантирует:
- Атомарность операций.
- Согласованность данных.
- Защиту от параллельных изменений.

### 7.2. Что дальше?
- Изучите продвинутые возможности управления транзакциями, такие как распределенные транзакции.
- Ознакомьтесь с интеграцией транзакций в микросервисной архитектуре.
- Исследуйте использование транзакций с NoSQL базами данных (например, MongoDB или Redis).