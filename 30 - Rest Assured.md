---
title: "30. Rest Assured"
aliases:
  - "Rest Assured"
  - "30. Rest Assured"
tags:
  - screening/fs
  - java
  - qa/autotests
  - tools
section: "Инструменты для автотестов"
status: evergreen
updated: 2026-07-21
---
## 30. Rest Assured

> [!abstract] Коротко
> Rest Assured — Java-библиотека для тестирования REST API.


**Rest Assured** — Java-библиотека для тестирования REST API.

Она позволяет:

* отправлять HTTP-запросы;
* передавать headers, query params и body;
* проверять status code;
* проверять JSON-ответ;
* доставать данные из response.

---

### 30.1. Структура запроса

Классическая структура Rest Assured:

```java
given()
        .header("Authorization", "Bearer token")
        .contentType(ContentType.JSON)
        .body(requestBody)
.when()
        .post("/users")
.then()
        .statusCode(201)
        .body("name", equalTo("Danil"));
```

Блоки:

* `given()` — подготовка запроса;
* `when()` — выполнение запроса;
* `then()` — проверка ответа.

---

### 30.2. Настройка вызовов

Часто настраивают:

* `baseURI`;
* `basePath`;
* headers;
* query params;
* content type;
* авторизацию;
* логирование.

```java
RestAssured.baseURI = "https://api.example.com";
RestAssured.basePath = "/api/v1";
```

```java
given()
        .log().all()
        .header("Authorization", "Bearer token")
        .queryParam("page", 1)
.when()
        .get("/users")
.then()
        .log().all()
        .statusCode(200);
```

---

### 30.3. Основные методы

| Метод | Для чего нужен |
| --- | --- |
| `given()` | Подготовить запрос |
| `header()` | Добавить header |
| `queryParam()` | Добавить query parameter |
| `body()` | Передать тело запроса |
| `contentType()` | Указать тип контента |
| `get()` | GET-запрос |
| `post()` | POST-запрос |
| `put()` | PUT-запрос |
| `delete()` | DELETE-запрос |
| `statusCode()` | Проверить HTTP-статус |
| `body()` | Проверить тело ответа |
| `extract()` | Достать данные из ответа |

---

### 30.4. Как достать значение из ответа

```java
String token = given()
        .body(loginRequest)
.when()
        .post("/login")
.then()
        .statusCode(200)
        .extract()
        .path("token");
```

---

### 30.5. Что важно сказать на скрининге

Rest Assured нужен для API-тестов. Запрос строится через `given`, выполняется в `when`, проверяется в `then`. Можно настраивать headers, параметры, body, авторизацию и проверять status code и JSON-ответ.
---

## Связанные заметки

- [[00 - Индекс|Индекс]]
- [[29 - jOOQ|Предыдущая заметка]]
- [[31 - Git|Следующая заметка]]
