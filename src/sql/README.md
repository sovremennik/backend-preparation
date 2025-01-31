### **Базы данных и SQL**

---

#### 1. **Основы баз данных**
- **Что такое база данных:**  
  - Реляционные и нереляционные базы данных.  
  - Примеры СУБД: MySQL, PostgreSQL, Oracle, SQL Server.  
- **Архитектура реляционных баз данных:**  
  - Таблицы, строки, столбцы.  
  - Первичные и внешние ключи (Primary Key, Foreign Key).  
- **Понятие транзакции:**  
  - ACID-свойства: Atomicity, Consistency, Isolation, Durability.  

---

#### 2. **Основы SQL (Structured Query Language)**
- **Основные команды:**  
  - **DDL (Data Definition Language):** `CREATE`, `ALTER`, `DROP`.  
  - **DML (Data Manipulation Language):** `SELECT`, `INSERT`, `UPDATE`, `DELETE`.  
  - **DCL (Data Control Language):** `GRANT`, `REVOKE`.  
- **Типы данных:**  
  - Числовые: `INT`, `FLOAT`, `DECIMAL`.  
  - Строковые: `VARCHAR`, `TEXT`, `CHAR`.  
  - Даты и время: `DATE`, `TIMESTAMP`, `TIME`.  

---

#### 3. **Создание и управление базой данных**
- **Создание базы данных:**  
  - `CREATE DATABASE`.  
  - **Создание таблиц:** `CREATE TABLE`.  
- **Изменение структуры:**  
  - Добавление и удаление колонок: `ALTER TABLE`.  
- **Удаление объектов:**  
  - `DROP DATABASE`, `DROP TABLE`.  

---

#### 4. **Запросы на выборку данных (SELECT)**
- **Базовый синтаксис:**  
  - `SELECT` и `WHERE`.  
  - Условия: `AND`, `OR`, `BETWEEN`, `LIKE`, `IN`.  
- **Сортировка и ограничение:**  
  - `ORDER BY`, `LIMIT`.  
- **Агрегация данных:**  
  - Группировка: `GROUP BY`.  
  - Агрегатные функции: `COUNT`, `SUM`, `AVG`, `MIN`, `MAX`.  
- **Работа с подзапросами:**  
  - Коррелированные и некоррелированные подзапросы.  

---

#### 5. **Соединения таблиц (Joins)**
- **Типы соединений:**  
  - `INNER JOIN`.  
  - `LEFT JOIN` и `RIGHT JOIN`.  
  - `FULL OUTER JOIN`.  
  - Кросс-продукт: `CROSS JOIN`.  
- **Соединение через ключи:**  
  - Primary и Foreign Key.  

---

#### 6. **Оптимизация запросов**
- **Индексы:**  
  - Что такое индекс, виды индексов: `UNIQUE`, `FULLTEXT`.  
  - Когда индексы помогают, а когда мешают.  
- **Использование `EXPLAIN` и анализ плана выполнения запроса.**  
- **Нормализация:**  
  - Нормальные формы: 1NF, 2NF, 3NF.  
  - Денормализация: когда это нужно.  

---

#### 7. **Транзакции и управление данными**
- **Транзакции:**  
  - Начало, фиксация и откат: `BEGIN`, `COMMIT`, `ROLLBACK`.  
- **Уровни изоляции транзакций:**  
  - `READ UNCOMMITTED`, `READ COMMITTED`, `REPEATABLE READ`, `SERIALIZABLE`.  
  - Проблемы: Dirty Read, Non-repeatable Read, Phantom Read.  
- **Ограничения данных:**  
  - `NOT NULL`, `UNIQUE`, `CHECK`, `DEFAULT`.  

---

#### 8. **Работа со сложными запросами**
- **Объединение результатов:**  
  - `UNION`, `UNION ALL`, `INTERSECT`, `EXCEPT`.  
- **Работа с окнами:**  
  - Оконные функции: `ROW_NUMBER`, `RANK`, `DENSE_RANK`, `NTILE`.  
  - Использование `OVER` и `PARTITION BY`.  
- **Рекурсивные запросы:**  
  - CTE (Common Table Expressions) и рекурсия.  

---

#### 9. **Администрирование базы данных**
- **Резервное копирование и восстановление:**  
  - Команды резервного копирования: `pg_dump`, `mysqldump`.  
- **Настройка пользователей и привилегий:**  
  - Создание пользователей: `CREATE USER`.  
  - Назначение прав: `GRANT`, `REVOKE`.  
- **Мониторинг и производительность:**  
  - Логи запросов.  
  - Метрики производительности: connections, locks.  

---

#### 10. **Работа с PostgreSQL**
- **Уникальные особенности PostgreSQL:**  
  - JSON/JSONB.  
  - Массивы и функции для работы с ними.  
  - Таблицы без PRIMARY KEY (Unlogged tables).  
- **Расширения:**  
  - `PostGIS`, `pg_trgm`, `uuid-ossp`.
