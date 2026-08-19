---
title: "19. AssertJ"
aliases:
  - "AssertJ"
  - "19. AssertJ"
tags:
  - screening/fs
  - java
  - qa/autotests
  - tools
section: "Инструменты для автотестов"
status: evergreen
updated: 2026-07-21
---
## 19. AssertJ

> [!abstract] Коротко
> AssertJ — библиотека для читаемых assertion-проверок в Java-тестах.


**AssertJ** — библиотека для читаемых assertion-проверок в Java-тестах.

Она позволяет писать проверки в fluent-стиле:

```java
assertThat(actual).isEqualTo(expected);
```

---

### 19.1. Жизненный цикл проверки AssertJ

Проверка обычно строится так:

1. Получить фактический результат.
2. Передать его в `assertThat(actual)`.
3. Добавить одну или несколько проверок.
4. Если проверка не проходит, тест падает с понятным сообщением.

Пример:

```java
String name = user.getName();

assertThat(name)
        .isNotNull()
        .isEqualTo("Danil");
```

---

### 19.2. Основные методы

| Метод | Для чего нужен |
| --- | --- |
| `assertThat(actual)` | Начинает проверку |
| `isEqualTo(expected)` | Проверяет равенство |
| `isNotEqualTo(value)` | Проверяет неравенство |
| `isNull()` | Проверяет `null` |
| `isNotNull()` | Проверяет, что значение не `null` |
| `contains(value)` | Проверяет наличие значения |
| `hasSize(size)` | Проверяет размер коллекции |
| `isEmpty()` | Проверяет пустоту |
| `isNotEmpty()` | Проверяет, что не пусто |
| `startsWith(text)` | Проверяет начало строки |
| `endsWith(text)` | Проверяет конец строки |

---

### 19.3. Пример с коллекцией

```java
List<String> names = List.of("Danil", "Anna", "Ivan");

assertThat(names)
        .hasSize(3)
        .contains("Danil")
        .doesNotContain("Petr");
```

---

### 19.4. Пример с объектом

```java
User user = new User("Danil", 28);

assertThat(user.getName()).isEqualTo("Danil");
assertThat(user.getAge()).isGreaterThan(18);
```

---

### 19.5. Что важно сказать на скрининге

AssertJ нужен для удобных и читаемых проверок. Он не запускает тесты сам, а используется внутри JUnit или другого test runner.
---

## Связанные заметки

- [[00 - Индекс|Индекс]]
- [[18 - Selenide|Предыдущая заметка]]
- [[20 - Lombok|Следующая заметка]]
