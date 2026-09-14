---
title: "4. Условия: if, else, switch"
aliases:
  - "Условия: if, else, switch"
  - "4. Условия: if, else, switch"
tags:
  - screening/fs
  - java
  - java/basics
section: "Основы Java"
status: evergreen
updated: 2026-09-14
---
## 4. Условия: if, else, switch

> [!abstract] Коротко
> Для ветвления в Java используют `if/else`, `switch` и тернарный оператор `?:`.

### 4.1. `if`, `else if`, `else`

```java
int score = 75;

if (score >= 90) {
    System.out.println("Отлично");
} else if (score >= 70) {
    System.out.println("Хорошо");
} else {
    System.out.println("Нужно подтянуть");
}
```

Условие в `if` должно иметь тип `boolean`.

---

### 4.2. Тернарный оператор

Подходит, когда нужно выбрать одно из двух значений:

```java
String message = age >= 18 ? "Можно" : "Нельзя";
```

Для сложной логики читаемее использовать обычный `if/else`.

---

### 4.3. Классический `switch`

`switch` удобен, когда одно выражение сравнивается с набором вариантов:

```java
switch (day) {
    case 1:
        System.out.println("Понедельник");
        break;
    case 2:
        System.out.println("Вторник");
        break;
    default:
        System.out.println("Неизвестный день");
}
```

В классической форме без `break` выполнение продолжится в следующем `case` — это называется fall-through.

---

### 4.4. Современный `switch` с `->`

В современной Java можно использовать стрелочную форму, где нет случайного fall-through:

```java
switch (status) {
    case "ACTIVE" -> System.out.println("Активен");
    case "BLOCKED" -> System.out.println("Заблокирован");
    default -> System.out.println("Неизвестный статус");
}
```

Несколько значений можно объединять:

```java
switch (day) {
    case 6, 7 -> System.out.println("Выходной");
    default -> System.out.println("Рабочий день");
}
```

---

### 4.5. `switch` как выражение

`switch` может возвращать значение:

```java
String type = switch (status) {
    case "ACTIVE" -> "Рабочий";
    case "BLOCKED" -> "Заблокированный";
    default -> "Неизвестный";
};
```

Если для `case` нужен блок из нескольких инструкций, значение возвращают через `yield`:

```java
int result = switch (operation) {
    case 1 -> a + b;
    case 2 -> {
        System.out.println("Вычитание");
        yield a - b;
    }
    default -> 0;
};
```

---

### 4.6. Что важно помнить

- `if/else` лучше подходит для диапазонов и произвольных boolean-условий.
- `switch` удобен для набора конкретных вариантов одного выражения.
- В стрелочной форме `case ... ->` `break` обычно не нужен.
- `switch`-expression должен дать результат для всех возможных веток либо завершиться исключением.

---

## Связанные заметки

- [[00 - Индекс|Индекс]]
- [[03 - Операторы и выражения|Предыдущая заметка]]
- [[05 - Циклы|Следующая заметка]]
