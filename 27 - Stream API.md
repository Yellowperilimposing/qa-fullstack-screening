---
title: "27. Stream API"
aliases:
  - "Stream API"
  - "27. Stream API"
tags:
  - screening/fs
  - java
  - qa/autotests
  - tools
section: "Инструменты для автотестов"
status: evergreen
updated: 2026-07-21
---
## 27. Stream API

> [!abstract] Коротко
> Stream API — инструмент Java для обработки данных в функциональном стиле.


**Stream API** — инструмент Java для обработки данных в функциональном стиле.

Stream чаще всего используют с коллекциями, чтобы:

* фильтровать элементы;
* преобразовывать элементы;
* сортировать;
* искать значения;
* собирать результат в новую коллекцию.

---

### 27.1. Как работает Stream

Обычно работа со Stream состоит из трех частей:

1. Источник данных: коллекция, массив или другой источник.
2. Промежуточные операции: `filter`, `map`, `sorted`.
3. Терминальная операция: `collect`, `toList`, `findFirst`, `count`.

Пример:

```java
List<String> names = List.of("Danil", "Anna", "Ivan");

List<String> result = names.stream()
        .filter(name -> name.startsWith("A"))
        .toList();
```

---

### 27.2. Основные методы Stream API

| Метод | Для чего нужен |
| --- | --- |
| `filter()` | Оставляет элементы по условию |
| `map()` | Преобразует элементы |
| `sorted()` | Сортирует элементы |
| `distinct()` | Убирает дубликаты |
| `limit()` | Ограничивает количество элементов |
| `forEach()` | Выполняет действие для каждого элемента |
| `toList()` | Собирает результат в список |
| `collect()` | Собирает результат через collector |
| `findFirst()` | Возвращает первый подходящий элемент |
| `count()` | Считает количество элементов |
| `anyMatch()` | Проверяет, есть ли хотя бы один подходящий элемент |

---

### 27.3. Примеры

Фильтрация:

```java
List<Integer> result = numbers.stream()
        .filter(number -> number > 10)
        .toList();
```

Преобразование:

```java
List<String> upperNames = names.stream()
        .map(String::toUpperCase)
        .toList();
```

Поиск:

```java
Optional<String> first = names.stream()
        .filter(name -> name.startsWith("D"))
        .findFirst();
```

---

### 27.4. Что важно сказать на скрининге

Stream API нужен для удобной обработки коллекций: фильтрации, преобразования, сортировки и поиска. Stream не изменяет исходную коллекцию, а строит цепочку операций и возвращает результат.
---

## Связанные заметки

- [[00 - Индекс|Индекс]]
- [[26 - Аннотации в Java|Предыдущая заметка]]
- [[28 - JDBC|Следующая заметка]]
