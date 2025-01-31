#### [Основные классы](#Основные-классы-1)
#### [Примеры использования](#Примеры-использования-1)
#### [Почему новый API лучше?](#Почему-новый-api-лучше-1)

---
# Новая API даты и времени (Java 8+)

Java 8 представила новый пакет `java.time`, который заменяет устаревшие классы `Date`, `Calendar` и другие, предоставляя более удобные и надежные инструменты для работы с датой и временем. Новый API вдохновлен библиотекой Joda-Time и создан для того, чтобы быть более читаемым, безопасным и гибким.

## Основные классы

1. **`LocalDate`** — представление только даты (без времени и часового пояса).
   ```java
   LocalDate date = LocalDate.now();
   System.out.println("Текущая дата: " + date); // Output: Текущая дата: 2025-01-13
   ```

2. **`LocalTime`** — представление только времени (без даты и часового пояса).
   ```java
   LocalTime time = LocalTime.now();
   System.out.println("Текущее время: " + time); // Output: Текущее время: 14:35:12.345
   ```

3. **`LocalDateTime`** — объединение даты и времени (без часового пояса).
   ```java
   LocalDateTime dateTime = LocalDateTime.now();
   System.out.println("Дата и время: " + dateTime); // Output: Дата и время: 2025-01-13T14:35:12.345
   ```

4. **`ZonedDateTime`** — дата и время с учетом часового пояса.
   ```java
   ZonedDateTime zonedDateTime = ZonedDateTime.now();
   System.out.println("Дата, время и зона: " + zonedDateTime);
   // Output: Дата, время и зона: 2025-01-13T14:35:12.345+03:00[Europe/Moscow]
   ```

5. **`Instant`** — точка во времени (timestamp).
   ```java
   Instant instant = Instant.now();
   System.out.println("Момент времени: " + instant); // Output: Момент времени: 2025-01-13T11:35:12.345Z
   ```

6. **`Duration`** и **`Period`** — работа с интервалами времени:
    - `Duration` для интервалов на уровне секунд и наносекунд.
    - `Period` для интервалов на уровне дней, месяцев и лет.

## Примеры использования

### 1. **Создание объектов**

- **Явное указание даты и времени:**
   ```java
   LocalDate specificDate = LocalDate.of(2023, Month.DECEMBER, 31);
   System.out.println("Заданная дата: " + specificDate);
   // Output: Заданная дата: 2023-12-31

   LocalTime specificTime = LocalTime.of(10, 15, 30);
   System.out.println("Заданное время: " + specificTime);
   // Output: Заданное время: 10:15:30
   ```

- **Разбор из строки:**
   ```java
   LocalDate parsedDate = LocalDate.parse("2023-12-31");
   System.out.println("Разобранная дата: " + parsedDate);
   // Output: Разобранная дата: 2023-12-31

   LocalTime parsedTime = LocalTime.parse("10:15:30");
   System.out.println("Разобранное время: " + parsedTime);
   // Output: Разобранное время: 10:15:30
   ```

### 2. **Операции с датами и временем**

- **Добавление или вычитание:**
   ```java
   LocalDate today = LocalDate.now();
   LocalDate nextWeek = today.plusWeeks(1);
   LocalDate lastMonth = today.minusMonths(1);

   System.out.println("Сегодня: " + today); // Output: Сегодня: 2025-01-13
   System.out.println("Через неделю: " + nextWeek); // Output: Через неделю: 2025-01-20
   System.out.println("Прошлый месяц: " + lastMonth); // Output: Прошлый месяц: 2024-12-13
   ```

- **Разница между датами:**
   ```java
   LocalDate startDate = LocalDate.of(2022, Month.JANUARY, 1);
   LocalDate endDate = LocalDate.of(2025, Month.JANUARY, 1);

   Period period = Period.between(startDate, endDate);
   System.out.println("Прошло лет: " + period.getYears()); // Output: Прошло лет: 3
   ```

- **Сравнение:**
   ```java
   boolean isBefore = LocalDate.of(2023, 12, 31).isBefore(LocalDate.of(2025, 1, 1));
   System.out.println("2023-12-31 раньше 2025-01-01? " + isBefore);
   // Output: 2023-12-31 раньше 2025-01-01? true
   ```

### 3. **Работа с часовыми поясами**

- **Получение списка доступных часовых поясов:**
   ```java
   ZoneId.getAvailableZoneIds().forEach(System.out::println);
   ```

- **Конвертация времени в другой часовой пояс:**
   ```java
   ZonedDateTime moscowTime = ZonedDateTime.now(ZoneId.of("Europe/Moscow"));
   ZonedDateTime newYorkTime = moscowTime.withZoneSameInstant(ZoneId.of("America/New_York"));

   System.out.println("Москва: " + moscowTime);
   System.out.println("Нью-Йорк: " + newYorkTime);
   ```

### 4. **Интервалы времени: `Duration` и `Period`**

- **`Duration` для времени:**
   ```java
   Duration duration = Duration.ofHours(5);
   System.out.println("Длительность в секундах: " + duration.getSeconds());
   // Output: Длительность в секундах: 18000
   ```

- **`Period` для дат:**
   ```java
   Period period = Period.of(2, 3, 15); // 2 года, 3 месяца и 15 дней
   System.out.println("Период: " + period);
   // Output: Период: P2Y3M15D
   ```

### 5. **Форматирование даты и времени**

- **По умолчанию:**
   ```java
   LocalDateTime now = LocalDateTime.now();
   System.out.println("По умолчанию: " + now);
   // Output: По умолчанию: 2025-01-13T14:35:12.345
   ```

- **С заданным шаблоном:**
   ```java
   DateTimeFormatter formatter = DateTimeFormatter.ofPattern("dd-MM-yyyy HH:mm:ss");
   String formattedDate = now.format(formatter);
   System.out.println("Форматированная дата: " + formattedDate);
   // Output: Форматированная дата: 13-01-2025 14:35:12
   ```

## Почему новый API лучше?

1. **Неизменяемость**: Все классы `java.time` являются неизменяемыми, что делает их потокобезопасными.
2. **Читаемость**: Методы и классы названы интуитивно понятно.
3. **Учет часовых поясов**: Теперь можно работать с `ZoneId` и `ZonedDateTime`.
4. **Гибкость форматирования**: Новый API поддерживает мощное форматирование и разбор строк.
5. **Интеграция с `Instant`**: Легкость работы с точными временными метками.

Новый API даты и времени — это современный инструмент для управления временными данными в приложениях любого масштаба.
