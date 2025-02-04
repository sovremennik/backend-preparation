#### [Введение](#Введение-1)
#### [Теоретическая-база](#Теоретическая-база-1)
#### [Практическая-часть](#Практическая-часть-1)
#### [Типичные ошибки и решения](#Типичные-ошибки-и-решения-1)
#### [Рекомендации и Best Practices](#Рекомендации-и-Best-Practices-1)
#### [Дополнительные материалы](#Дополнительные-материалы-1)
#### [Заключение](#Заключение-1)

---

## Введение

### 1.1. Зачем это нужно?
Обработка ошибок в Spring MVC является критически важной частью разработки enterprise-приложений. Она позволяет:
- Предоставлять пользователю понятные сообщения об ошибках.
- Обеспечивать надежную работу приложения даже при возникновении исключений.
- Логировать ошибки для последующего анализа.

Пример применения: В крупных компаниях используется централизованная система обработки ошибок для всех микросервисов, что упрощает поддержку и мониторинг.

---

## Теоретическая база

### 2.1. Как работает обработка ошибок в Spring MVC?

Spring MVC предоставляет несколько механизмов для обработки ошибок:
1. **@ControllerAdvice**: Глобальный обработчик исключений для всего приложения.
2. **@ExceptionHandler**: Локальный обработчик исключений для конкретного контроллера.
3. **ErrorController**: Стандартный интерфейс для обработки ошибок на уровне приложения.
4. **Custom Error Pages**: Настройка собственных страниц ошибок для HTTP-статусов.

---

### 2.2. Основные механизмы обработки ошибок

#### 2.2.1. @ControllerAdvice
`@ControllerAdvice` — это аннотация, которая позволяет создать глобальный обработчик исключений для всего приложения. Он может перехватывать исключения из всех контроллеров.

**Пример использования:**
```java
@ControllerAdvice
public class GlobalExceptionHandler {

    @ExceptionHandler(ResourceNotFoundException.class)
    public ResponseEntity<ErrorResponse> handleResourceNotFound(ResourceNotFoundException ex) {
        ErrorResponse error = new ErrorResponse(HttpStatus.NOT_FOUND.value(), ex.getMessage());
        return new ResponseEntity<>(error, HttpStatus.NOT_FOUND);
    }

    @ExceptionHandler(Exception.class)
    public ResponseEntity<ErrorResponse> handleGeneralException(Exception ex) {
        ErrorResponse error = new ErrorResponse(HttpStatus.INTERNAL_SERVER_ERROR.value(), "An error occurred: " + ex.getMessage());
        return new ResponseEntity<>(error, HttpStatus.INTERNAL_SERVER_ERROR);
    }
}
```

#### 2.2.2. @ExceptionHandler
`@ExceptionHandler` — это аннотация, которая позволяет обрабатывать исключения на уровне конкретного контроллера.

**Пример использования:**
```java
@RestController
@RequestMapping("/api/users")
public class UserController {

    @GetMapping("/{id}")
    public User getUser(@PathVariable int id) {
        if (id <= 0) {
            throw new IllegalArgumentException("Invalid user ID");
        }
        return new User(id, "John Doe");
    }

    @ExceptionHandler(IllegalArgumentException.class)
    public ResponseEntity<String> handleIllegalArgumentException(IllegalArgumentException ex) {
        return new ResponseEntity<>(ex.getMessage(), HttpStatus.BAD_REQUEST);
    }
}
```

#### 2.2.3. ErrorController
`ErrorController` — это стандартный интерфейс Spring Boot, который позволяет определить логику обработки ошибок на уровне приложения.

**Пример реализации:**
```java
@Component
public class CustomErrorController implements ErrorController {

    private final ErrorAttributes errorAttributes;

    public CustomErrorController(ErrorAttributes errorAttributes) {
        this.errorAttributes = errorAttributes;
    }

    @RequestMapping("/error")
    public ResponseEntity<Map<String, Object>> handleError(HttpServletRequest request) {
        Map<String, Object> errorDetails = errorAttributes.getErrorAttributes(
            request.getAttribute(RequestDispatcher.ERROR_ATTRIBUTES), false);

        HttpStatus status = getStatus(request);
        return new ResponseEntity<>(errorDetails, status);
    }

    private HttpStatus getStatus(HttpServletRequest request) {
        Integer statusCode = (Integer) request.getAttribute(RequestDispatcher.ERROR_STATUS_CODE);
        return statusCode == null ? HttpStatus.INTERNAL_SERVER_ERROR : HttpStatus.valueOf(statusCode);
    }
}
```

#### 2.2.4. Custom Error Pages
Spring Boot позволяет настраивать собственные страницы ошибок для различных HTTP-статусов.

**Настройка через `application.properties`:**
```properties
server.error.whitelabel.enabled=false # Отключает стандартную страницу ошибок
```

**Создание кастомной страницы ошибки:**
- Создайте файл `error.html` в директории `src/main/resources/templates/`.
- Этот файл будет использоваться для отображения ошибок по умолчанию.

**Пример `error.html`:**
```html
<!DOCTYPE html>
<html xmlns:th="http://www.thymeleaf.org">
<head>
    <title>Error</title>
</head>
<body>
    <h1>An error occurred!</h1>
    <p>Status: <span th:text="${status}"></span></p>
    <p>Message: <span th:text="${message}"></span></p>
</body>
</html>
```

---

## Практическая часть

### 3.1. Базовые примеры

#### Пример 1: Глобальный обработчик исключений
```java
@ControllerAdvice
public class GlobalExceptionHandler {

    @ExceptionHandler(ResourceNotFoundException.class)
    public ResponseEntity<ErrorResponse> handleResourceNotFound(ResourceNotFoundException ex) {
        ErrorResponse error = new ErrorResponse(HttpStatus.NOT_FOUND.value(), ex.getMessage());
        return new ResponseEntity<>(error, HttpStatus.NOT_FOUND);
    }

    @ExceptionHandler(Exception.class)
    public ResponseEntity<ErrorResponse> handleGeneralException(Exception ex) {
        ErrorResponse error = new ErrorResponse(HttpStatus.INTERNAL_SERVER_ERROR.value(), "An unexpected error occurred: " + ex.getMessage());
        return new ResponseEntity<>(error, HttpStatus.INTERNAL_SERVER_ERROR);
    }
}

class ErrorResponse {
    private int status;
    private String message;

    public ErrorResponse(int status, String message) {
        this.status = status;
        this.message = message;
    }

    // Геттеры и сеттеры
}
```

#### Пример 2: Локальный обработчик исключений
```java
@RestController
@RequestMapping("/api/users")
public class UserController {

    @GetMapping("/{id}")
    public User getUser(@PathVariable int id) {
        if (id <= 0) {
            throw new IllegalArgumentException("Invalid user ID");
        }
        return new User(id, "John Doe");
    }

    @ExceptionHandler(IllegalArgumentException.class)
    public ResponseEntity<String> handleIllegalArgumentException(IllegalArgumentException ex) {
        return new ResponseEntity<>(ex.getMessage(), HttpStatus.BAD_REQUEST);
    }
}
```

#### Пример 3: Обработка ошибок с помощью ErrorController
```java
@Component
public class CustomErrorController implements ErrorController {

    private final ErrorAttributes errorAttributes;

    public CustomErrorController(ErrorAttributes errorAttributes) {
        this.errorAttributes = errorAttributes;
    }

    @RequestMapping("/error")
    public ResponseEntity<Map<String, Object>> handleError(HttpServletRequest request) {
        Map<String, Object> errorDetails = errorAttributes.getErrorAttributes(
            request.getAttribute(RequestDispatcher.ERROR_ATTRIBUTES), false);

        HttpStatus status = getStatus(request);
        return new ResponseEntity<>(errorDetails, status);
    }

    private HttpStatus getStatus(HttpServletRequest request) {
        Integer statusCode = (Integer) request.getAttribute(RequestDispatcher.ERROR_STATUS_CODE);
        return statusCode == null ? HttpStatus.INTERNAL_SERVER_ERROR : HttpStatus.valueOf(statusCode);
    }
}
```

---

### 3.2. Реальные кейсы

#### Сценарий: Обработка ошибок в REST API
```java
@RestControllerAdvice
public class RestGlobalExceptionHandler {

    @ExceptionHandler(ResourceNotFoundException.class)
    public ResponseEntity<ErrorResponse> handleResourceNotFound(ResourceNotFoundException ex) {
        ErrorResponse error = new ErrorResponse(HttpStatus.NOT_FOUND.value(), ex.getMessage());
        return new ResponseEntity<>(error, HttpStatus.NOT_FOUND);
    }

    @ExceptionHandler(MethodArgumentNotValidException.class)
    public ResponseEntity<ErrorResponse> handleValidationErrors(MethodArgumentNotValidException ex) {
        List<String> errors = ex.getBindingResult()
            .getFieldErrors()
            .stream()
            .map(error -> error.getField() + ": " + error.getDefaultMessage())
            .toList();

        ErrorResponse error = new ErrorResponse(HttpStatus.BAD_REQUEST.value(), String.join(", ", errors));
        return new ResponseEntity<>(error, HttpStatus.BAD_REQUEST);
    }
}
```

#### Сценарий: Кастомная страница ошибки
1. Создайте файл `error.html` в директории `src/main/resources/templates/`:
   ```html
   <!DOCTYPE html>
   <html xmlns:th="http://www.thymeleaf.org">
   <head>
       <title>Error</title>
   </head>
   <body>
       <h1>Error Occurred</h1>
       <p>Status: <span th:text="${status}"></span></p>
       <p>Message: <span th:text="${message}"></span></p>
   </body>
   </html>
   ```

2. Настройте отключение стандартной страницы ошибок в `application.properties`:
   ```properties
   server.error.whitelabel.enabled=false
   ```

---

## Типичные ошибки и решения

### 4.1. Распространенные ошибки

1. **Отсутствие обработчика исключений**:
    - Проблема: Исключения не перехватываются, и клиент получает стандартную страницу ошибки.
    - Решение: Добавьте глобальный обработчик с помощью `@ControllerAdvice`.

2. **Неправильное маппингование HTTP-статусов**:
    - Проблема: Исключения не преобразуются в правильные HTTP-статусы.
    - Решение: Используйте аннотацию `@ResponseStatus` или явно устанавливайте статус в ответе.

3. **Отсутствие деталей об ошибке**:
    - Проблема: Клиент не получает достаточной информации об ошибке.
    - Решение: Создайте DTO для представления ошибок (например, `ErrorResponse`).

---

## Рекомендации и Best Practices

1. **Используйте @ControllerAdvice для глобальной обработки ошибок**:
    - Это делает код более чистым и модульным.

2. **Определите специфические исключения для бизнес-логики**:
    - Например, `ResourceNotFoundException`, `AccessDeniedException`.

3. **Добавляйте детали ошибок только для production**:
    - В production-среде показывайте только общие сообщения об ошибках без технических деталей.

4. **Используйте DTO для представления ошибок**:
    - Например, класс `ErrorResponse` для REST API.

---

## Дополнительные материалы

### 6.1. Глоссарий

- **@ControllerAdvice**: Аннотация для создания глобального обработчика исключений.
- **@ExceptionHandler**: Аннотация для обработки исключений на уровне контроллера.
- **ErrorController**: Интерфейс для обработки ошибок на уровне приложения.
- **Custom Error Pages**: Механизм создания собственных страниц ошибок.

### 6.2. Полезные ссылки

- [Spring MVC Exception Handling](https://docs.spring.io/spring-boot/docs/current/reference/htmlsingle/#web.servlet.spring-mvc.exception-handling)
- [Customizing Error Pages in Spring Boot](https://spring.io/blog/2013/11/01/exception-handling-in-spring-mvc)

---

## Заключение

### 7.1. Итоги
Обработка ошибок в Spring MVC обеспечивает надежную и гибкую систему управления исключениями. Использование `@ControllerAdvice`, `@ExceptionHandler` и `ErrorController` позволяет эффективно перехватывать и обрабатывать ошибки на разных уровнях приложения.

### 7.2. Что дальше?
- Изучите интеграцию обработки ошибок с Actuator для мониторинга состояния приложения.
- Ознакомьтесь с продвинутыми возможностями логирования ошибок.
- Исследуйте использование сторонних библиотек для улучшения обработки ошибок (например, Sentry или ELK Stack).