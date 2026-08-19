---
title: "28. JDBC"
aliases:
  - "JDBC"
  - "28. JDBC"
tags:
  - screening/fs
  - java
  - qa/autotests
  - tools
section: "Инструменты для автотестов"
status: evergreen
updated: 2026-07-21
---
## 28. JDBC

> [!abstract] Коротко
> JDBC — стандартный Java API для работы с базами данных.


**JDBC** — стандартный Java API для работы с базами данных.

JDBC позволяет:

* подключаться к базе данных;
* выполнять SQL-запросы;
* получать результаты;
* управлять транзакциями.

---

### 28.1. Основные элементы JDBC

| Элемент | Для чего нужен |
| --- | --- |
| `DriverManager` | Создает подключение к базе |
| `Connection` | Соединение с базой данных |
| `Statement` | Выполняет простой SQL-запрос |
| `PreparedStatement` | Выполняет SQL с параметрами |
| `ResultSet` | Хранит результат `SELECT` |

---

### 28.2. Пример SELECT

```java
String sql = "select id, name from users where id = ?";

try (Connection connection = DriverManager.getConnection(url, user, password);
     PreparedStatement statement = connection.prepareStatement(sql)) {

    statement.setInt(1, 10);

    ResultSet resultSet = statement.executeQuery();

    if (resultSet.next()) {
        String name = resultSet.getString("name");
        System.out.println(name);
    }
}
```

---

### 28.3. Statement vs PreparedStatement

`Statement` используют для простых запросов без параметров.

`PreparedStatement` лучше использовать почти всегда, потому что он:

* поддерживает параметры;
* безопаснее;
* помогает избегать SQL-инъекций;
* удобнее для повторного выполнения запроса.

---

### 28.4. Что важно сказать на скрининге

JDBC — базовый Java API для работы с базой данных. Через `Connection` открывается соединение, через `PreparedStatement` выполняется SQL, а результат читается из `ResultSet`.
---

## Связанные заметки

- [[00 - Индекс|Индекс]]
- [[27 - Stream API|Предыдущая заметка]]
- [[29 - jOOQ|Следующая заметка]]
