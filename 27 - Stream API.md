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
updated: 2026-09-14
---
## 27. Stream API

> [!abstract] Коротко
> Stream API описывает конвейер обработки данных: источник → промежуточные операции → терминальная операция.

### 27.1. Stream — не коллекция

Stream не хранит данные. Он обрабатывает элементы из источника: коллекции, массива, файла, генератора и т.д.

```java
List<String> result = names.stream()
        .filter(name -> name.startsWith("A"))
        .map(String::toUpperCase)
        .toList();
```

Обычный stream одноразовый: после терминальной операции повторно использовать тот же объект stream нельзя.

---

### 27.2. Промежуточные операции

Промежуточные операции возвращают новый stream и обычно ленивы — вычисления запускаются только при терминальной операции.

Частые операции:

- `filter()` — фильтрация;
- `map()` — преобразование;
- `flatMap()` — разворачивание вложенных потоков;
- `distinct()` — удаление дубликатов по `equals()`/`hashCode()`;
- `sorted()` — сортировка;
- `limit()` / `skip()` — ограничение/пропуск.

```java
Stream<String> pipeline = names.stream()
        .filter(name -> name.length() > 3)
        .map(String::toUpperCase);
```

До терминальной операции элементы могут ещё не обрабатываться.

---

### 27.3. Терминальные операции

Они запускают обработку и завершают stream pipeline:

- `toList()` / `collect()`;
- `forEach()`;
- `count()`;
- `findFirst()` / `findAny()`;
- `anyMatch()` / `allMatch()` / `noneMatch()`;
- `min()` / `max()`;
- `reduce()`.

```java
long count = names.stream()
        .filter(name -> name.startsWith("A"))
        .count();
```

---

### 27.4. `Optional`

Операции поиска могут не найти значение, поэтому `findFirst()`, `findAny()`, `min()` и `max()` возвращают `Optional`:

```java
Optional<String> first = names.stream()
        .filter(name -> name.startsWith("D"))
        .findFirst();

first.ifPresent(System.out::println);
```

Не стоит без проверки вызывать `optional.get()`. Обычно используют `orElse`, `orElseGet`, `orElseThrow`, `ifPresent`.

---

### 27.5. Изменяет ли Stream исходную коллекцию

Операции вроде `filter()` и `map()` сами по себе исходную коллекцию не меняют:

```java
List<String> upper = names.stream()
        .map(String::toUpperCase)
        .toList();
```

Но side effects внутри `forEach`, `peek` или lambda технически возможны. Поэтому точнее говорить: Stream API рассчитан прежде всего на декларативные операции без изменения источника, но сам язык не делает side effects невозможными.

---

### 27.6. `map` и `flatMap`

`map` преобразует один элемент в один результат:

```java
List<Integer> lengths = names.stream()
        .map(String::length)
        .toList();
```

`flatMap` полезен, когда каждый элемент даёт несколько вложенных элементов:

```java
List<String> words = lines.stream()
        .flatMap(line -> Arrays.stream(line.split(" ")))
        .toList();
```

---

### 27.7. `reduce`

`reduce` сворачивает поток в одно значение:

```java
int sum = numbers.stream()
        .reduce(0, Integer::sum);
```

Для примитивных чисел часто удобнее специализированные потоки:

```java
int sum = numbers.stream()
        .mapToInt(Integer::intValue)
        .sum();
```

---

### 27.8. Parallel stream

```java
names.parallelStream()
```

Параллельный stream не является автоматическим ускорением. Он добавляет стоимость распараллеливания и требует особенно внимательно относиться к side effects и thread safety.

---

### 27.9. Что сказать на скрининге

Stream API строит ленивый pipeline обработки данных. Промежуточные операции (`filter`, `map`, `sorted`) формируют pipeline, терминальная операция (`toList`, `count`, `findFirst`) запускает вычисление. Stream не является коллекцией и обычно используется один раз.

---

## Связанные заметки

- [[00 - Индекс|Индекс]]
- [[26 - Аннотации в Java|Предыдущая заметка]]
- [[28 - JDBC|Следующая заметка]]
