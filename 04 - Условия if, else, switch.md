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
updated: 2026-07-21
---
## 4. Условия: if, else, switch

> [!abstract] Коротко
> Условие позволяет программе принимать решения.


### 4.1. Что такое условие?

Условие позволяет программе принимать решения.

Например:

```text
Если возраст больше или равен 18 → доступ разрешён
Иначе → доступ запрещён
```

В Java это записывается через `if`.

---

### 4.2. if

```java
int age = 20;

if (age >= 18) {
    System.out.println("Доступ разрешён");
}
```

Код внутри `{}` выполнится только если условие истинное.

---

### 4.3. if-else

```java
int age = 16;

if (age >= 18) {
    System.out.println("Доступ разрешён");
} else {
    System.out.println("Доступ запрещён");
}
```

Если условие `true`, выполнится блок `if`.

Если условие `false`, выполнится блок `else`.

---

### 4.4. if-else if-else

Используется, когда условий несколько.

```java
int score = 75;

if (score >= 90) {
    System.out.println("Отлично");
} else if (score >= 70) {
    System.out.println("Хорошо");
} else if (score >= 50) {
    System.out.println("Удовлетворительно");
} else {
    System.out.println("Плохо");
}
```

---

### 4.5. Тернарный оператор в условиях

Тернарный оператор можно использовать вместо простого `if-else`.

```java
int age = 18;

String message = age >= 18 ? "Можно" : "Нельзя";

System.out.println(message);
```

Но не стоит злоупотреблять тернарным оператором, если условие сложное. В таких случаях лучше использовать обычный `if-else`.

---

### 4.6. switch-case

`switch` используется, когда нужно сравнить одну переменную с несколькими возможными значениями.

Пример:

```java
int day = 3;

switch (day) {
    case 1:
        System.out.println("Понедельник");
        break;
    case 2:
        System.out.println("Вторник");
        break;
    case 3:
        System.out.println("Среда");
        break;
    default:
        System.out.println("Неизвестный день");
}
```

Здесь:

* `switch (day)` — проверяем значение переменной `day`;
* `case 1` — если значение равно `1`;
* `break` — завершает выполнение `switch`;
* `default` — выполняется, если ни один `case` не подошёл.

---

### 4.7. Почему важен break?

Если не написать `break`, Java продолжит выполнять следующие `case`.

Пример:

```java
int day = 1;

switch (day) {
    case 1:
        System.out.println("Понедельник");
    case 2:
        System.out.println("Вторник");
    default:
        System.out.println("Неизвестный день");
}
```

Результат:

```text
Понедельник
Вторник
Неизвестный день
```

Поэтому в классическом `switch` обычно пишут `break`.

---

## Связанные заметки

- [[00 - Индекс|Индекс]]
- [[03 - Операторы и выражения|Предыдущая заметка]]
- [[05 - Циклы|Следующая заметка]]
