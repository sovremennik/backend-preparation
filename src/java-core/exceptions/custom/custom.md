#### [Что такое пользовательские исключения](#Что-такое-пользовательские-исключения-1)
#### [Создание пользовательского исключения](#Создание-пользовательского-исключения-1)
#### [Примеры использования пользовательских исключений](#Примеры-использования-пользовательских-исключений-1)
#### [Преимущества использования пользовательских исключений](#Преимущества-использования-пользовательских-исключений-1)
#### [Рекомендации по использованию](#Рекомендации-по-использованию-1)


---
# Пользовательские исключения

## Что такое пользовательские исключения

Пользовательские исключения (сustom exceptions) — это классы, созданные разработчиком и наследуемые от существующих классов исключений, таких как `Exception` или `RuntimeException`. Они используются для представления специфических ошибок в приложении, которые невозможно выразить стандартными исключениями.

Пользовательские исключения помогают:
- Улучшить читаемость кода, благодаря понятным названиям исключений.
- Локализовать обработку ошибок в рамках бизнес-логики.
- Обеспечить более детальную информацию о возникших ошибках.

---

## Создание пользовательского исключения

Для создания пользовательского исключения нужно:
1. Унаследоваться от подходящего базового класса (`Exception` или `RuntimeException`).
2. Определить необходимые конструкторы для передачи сообщений и причин ошибок.

Пример пользовательского исключения для проверки бизнес-логики:

```java
public class InsufficientFundsException extends Exception {
    public InsufficientFundsException(String message) {
        super(message);
    }

    public InsufficientFundsException(String message, Throwable cause) {
        super(message, cause);
    }
}
```

### Когда использовать `Exception` или `RuntimeException`

- **`Exception` (Checked Exception):** Используйте для ошибок, которые должны быть явно обработаны (например, при работе с файлами или внешними API).
- **`RuntimeException` (Unchecked Exception):** Используйте для ошибок, связанных с некорректным состоянием программы, которые обычно возникают из-за ошибок программиста (например, `IllegalStateException`).

---

## Примеры использования пользовательских исключений

### 1. Обработка ошибок в банковской системе

```java
import java.math.BigDecimal;

public class BankAccount {
    private BigDecimal balance = BigDecimal.ZERO;

    public void withdraw(BigDecimal amount) throws InsufficientFundsException {
        if (amount.compareTo(balance) > 0) {
            throw new InsufficientFundsException("Недостаточно средств на счете. Баланс: " + balance);
        }
        balance = balance.subtract(amount);
    }
}

public class BankApp {
    public static void main(String[] args) {
        BankAccount account = new BankAccount();

        try {
            account.withdraw(new BigDecimal("100"));
        } catch (InsufficientFundsException e) {
            System.err.println("Ошибка: " + e.getMessage());
        }
    }
}
```

### 2. Обработка ошибок в REST API

```java
@ResponseStatus(HttpStatus.NOT_FOUND)
public class ResourceNotFoundException extends RuntimeException {
    public ResourceNotFoundException(String resourceName, String fieldName, Object fieldValue) {
        super(String.format("%s с %s '%s' не найден", resourceName, fieldName, fieldValue));
    }
}

@RestController
@RequestMapping("/api")
public class UserController {

    @GetMapping("/users/{id}")
    public User getUserById(@PathVariable Long id) {
        return userRepository.findById(id)
            .orElseThrow(() -> new ResourceNotFoundException("Пользователь", "id", id));
    }
}
```

### 3. Логирование ошибок

```java
public class OrderService {

    private static final Logger logger = LoggerFactory.getLogger(OrderService.class);

    public void processOrder(Order order) {
        try {
            validateOrder(order);
        } catch (InvalidOrderException e) {
            logger.error("Ошибка обработки заказа: " + e.getMessage(), e);
            throw e;
        }
    }

    private void validateOrder(Order order) throws InvalidOrderException {
        if (order.getItems().isEmpty()) {
            throw new InvalidOrderException("Заказ должен содержать хотя бы один товар.");
        }
    }
}
```

---

## Преимущества использования пользовательских исключений

1. **Ясность кода:** Конкретные названия классов исключений делают код понятнее.
2. **Гибкость:** Возможность добавлять дополнительные поля и методы для передачи контекстной информации (например, идентификатор ресурса или метка времени).
3. **Единая обработка:** Позволяет централизовать обработку специфических ошибок в приложении.

---

## Рекомендации по использованию

1. **Минимизируйте количество пользовательских исключений:** Создавайте их только в случаях, когда стандартные исключения не подходят.
2. **Предоставляйте детальные сообщения:** Используйте понятные описания ошибок для облегчения отладки.
3. **Указывайте причины ошибок:** Передавайте исходные исключения через конструктор `Throwable cause`, чтобы сохранить стек вызовов.

---

Пользовательские исключения — это мощный инструмент для повышения читаемости и надёжности кода. Они помогают изолировать логику обработки ошибок и предоставляют более детальную информацию, что особенно важно в сложных приложениях.

