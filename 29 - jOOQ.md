---
title: "29. jOOQ"
aliases:
  - "jOOQ"
  - "29. jOOQ"
tags:
  - screening/fs
  - java
  - qa/autotests
  - tools
section: "Инструменты для автотестов"
status: evergreen
updated: 2026-07-21
---
## 29. jOOQ

> [!abstract] Коротко
> jOOQ — Java-библиотека для работы с SQL через типобезопасный DSL.


**jOOQ** — Java-библиотека для работы с SQL через типобезопасный DSL.

jOOQ позволяет писать SQL-запросы в Java-коде так, чтобы компилятор помогал находить ошибки в таблицах, полях и типах данных.

---

### 29.1. Что такое jOOQ

**jOOQ** расшифровывается как **Java Object Oriented Querying**.

Это инструмент, который позволяет писать SQL почти как обычный SQL, но через Java API.

Пример:

```java
dsl.selectFrom(USERS)
        .where(USERS.EMAIL.eq("test@mail.com"))
        .fetchOne();
```

Здесь запрос пишется не строкой, а через Java-объекты: `USERS`, `USERS.EMAIL`, `eq()`, `fetchOne()`.

---

### 29.2. Для чего используется jOOQ

jOOQ используют, чтобы:

* писать SQL в Java-стиле;
* получать типобезопасные запросы;
* работать с таблицами и полями как с Java-объектами;
* доставать данные из базы для тестов;
* готовить тестовые данные;
* проверять состояние базы после API/UI-действий;
* переиспользовать запросы и результаты.

В автотестах jOOQ часто нужен для подготовки и проверки данных:

```java
UserRecord user = dsl.selectFrom(USERS)
        .where(USERS.EMAIL.eq("test@mail.com"))
        .fetchOne();

assertThat(user.getStatus()).isEqualTo("ACTIVE");
```

---

### 29.3. Как подключаемся к базе данных

Чтобы работать с базой, приложению нужны параметры подключения:

* URL базы данных;
* логин;
* пароль;
* драйвер базы;
* иногда schema/database name.

Обычно подключение создается через `DataSource` или JDBC `Connection`.

Пример через `DriverManager`:

```java
Connection connection = DriverManager.getConnection(
        "jdbc:postgresql://localhost:5432/app",
        "user",
        "password"
);
```

Затем на основе этого подключения создается объект jOOQ.

---

### 29.4. Как называется подключение в jOOQ

Главный объект для работы с jOOQ называется **DSLContext**.

Часто его называют просто `dsl`.

```java
DSLContext dsl = DSL.using(connection, SQLDialect.POSTGRES);
```

`DSLContext` — это точка входа в jOOQ.

Через него строят и выполняют запросы:

```java
dsl.selectFrom(USERS).fetch();
dsl.insertInto(USERS).set(USERS.NAME, "Danil").execute();
dsl.update(USERS).set(USERS.STATUS, "ACTIVE").execute();
dsl.deleteFrom(USERS).where(USERS.ID.eq(10)).execute();
```

Коротко: физическое соединение с базой — это `Connection` или `DataSource`, а основной объект jOOQ для запросов — `DSLContext`.

---

### 29.5. Методы jOOQ

| Метод | Для чего нужен |
| --- | --- |
| `select()` | Собрать `SELECT`-запрос |
| `selectFrom()` | Выбрать данные из таблицы |
| `insertInto()` | Собрать `INSERT` |
| `update()` | Собрать `UPDATE` |
| `deleteFrom()` | Собрать `DELETE` |
| `where()` | Добавить условие |
| `and()` / `or()` | Объединить условия |
| `join()` | Добавить join |
| `orderBy()` | Сортировка |
| `limit()` | Ограничение количества строк |
| `fetch()` | Получить список записей |
| `fetchOne()` | Получить одну запись |
| `fetchInto()` | Смаппить результат в класс |
| `execute()` | Выполнить запрос без возврата строк |

---

### 29.6. Пример SELECT

```java
UserRecord user = dsl.selectFrom(USERS)
        .where(USERS.EMAIL.eq("test@mail.com"))
        .fetchOne();
```

Здесь:

* `dsl` — объект `DSLContext`;
* `USERS` — сгенерированная таблица;
* `USERS.EMAIL` — поле таблицы;
* `eq()` — условие равенства;
* `fetchOne()` — получить одну запись.

---

### 29.7. Пример INSERT

```java
dsl.insertInto(USERS)
        .set(USERS.NAME, "Danil")
        .set(USERS.EMAIL, "test@mail.com")
        .execute();
```

`execute()` выполняет запрос и возвращает количество измененных строк.

---

### 29.8. Как переиспользовать данные из jOOQ

Полученные данные можно:

* сохранить в переменную;
* использовать в assertion;
* передать в API-запрос;
* преобразовать в DTO;
* использовать для подготовки следующего шага теста.

```java
UserRecord user = userRepository.findByEmail("test@mail.com");

assertThat(user.getName()).isEqualTo("Danil");

String userId = user.getId().toString();
```

---

### 29.9. JDBC vs jOOQ

| JDBC | jOOQ |
| --- | --- |
| Низкоуровневый API | Более удобная DSL-обертка |
| SQL пишется строкой | SQL собирается Java-кодом |
| Меньше типобезопасности | Больше типобезопасности |
| Больше ручного кода | Меньше шаблонного кода |

---

### 29.10. Базы данных есть опыт? SQL знаешь?

Пример ответа:

Да, базово работал с базами данных и SQL. Понимаю, как выполнять `SELECT`, `INSERT`, `UPDATE`, `DELETE`, как использовать `WHERE`, `JOIN`, сортировку и ограничения. В тестах база может использоваться для подготовки тестовых данных и проверки результата после API или UI-действий.

Примеры SQL:

```sql
select * from users where email = 'test@mail.com';
```

```sql
update users set status = 'ACTIVE' where id = 10;
```

```sql
select u.id, u.name, o.id as order_id
from users u
join orders o on o.user_id = u.id
where u.email = 'test@mail.com';
```

---

### 29.11. Что важно сказать на скрининге

jOOQ нужен для удобной и типобезопасной работы с SQL из Java. Физическое подключение к базе обычно идет через `Connection` или `DataSource`, а главный объект jOOQ для запросов называется `DSLContext`. В тестах jOOQ часто используют для подготовки данных, проверки состояния базы и переиспользования полученных данных в шагах теста.
---

## Связанные заметки

- [[00 - Индекс|Индекс]]
- [[28 - JDBC|Предыдущая заметка]]
- [[30 - Rest Assured|Следующая заметка]]
