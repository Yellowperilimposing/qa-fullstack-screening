---
title: "26. Аннотации в Java"
aliases:
  - "Аннотации в Java"
  - "26. Аннотации в Java"
tags:
  - screening/fs
  - java
  - qa/autotests
  - tools
section: "Инструменты для автотестов"
status: evergreen
updated: 2026-09-14
---
## 26. Аннотации в Java

> [!abstract] Коротко
> Аннотация — метаданные о коде. Их могут использовать компилятор, build-tools, annotation processors, фреймворки и runtime-код через reflection.

### 26.1. Что делает аннотация

Аннотация сама по себе не выполняет бизнес-логику. Значение появляется, когда её интерпретирует другой механизм.

Примеры:

```java
@Override
public String toString() {
    return "User";
}
```

```java
@Test
void shouldLogin() {
}
```

```java
@Getter
class User {
    private String name;
}
```

В этих примерах аннотации обрабатываются разными механизмами: компилятором, JUnit и Lombok annotation processor.

---

### 26.2. Объявление своей аннотации

```java
@interface Owner {
    String value();
}
```

Использование:

```java
@Owner("qa-team")
class LoginTest {
}
```

У элементов аннотации можно задавать значения по умолчанию:

```java
@interface Retry {
    int value() default 3;
}
```

---

### 26.3. `@Target`

`@Target` задаёт, где аннотацию разрешено использовать:

```java
@Target({ElementType.TYPE, ElementType.METHOD})
@interface Marker {
}
```

Частые `ElementType`: `TYPE`, `METHOD`, `FIELD`, `PARAMETER`, `CONSTRUCTOR`, `ANNOTATION_TYPE`, `PACKAGE`, `RECORD_COMPONENT`.

---

### 26.4. `@Retention`

`@Retention` определяет, как долго хранится аннотация:

| Policy | Где доступна |
| --- | --- |
| `SOURCE` | только в исходном коде |
| `CLASS` | сохраняется в `.class`, но обычно недоступна через runtime reflection |
| `RUNTIME` | сохраняется и доступна во время выполнения |

Если `@Retention` не указана, используется `RetentionPolicy.CLASS`.

```java
@Retention(RetentionPolicy.RUNTIME)
@Target(ElementType.TYPE)
@interface Description {
    String value();
}
```

---

### 26.5. Другие мета-аннотации

- `@Documented` — включать использование аннотации в Javadoc;
- `@Inherited` — позволяет наследовать аннотацию класса подклассами (с оговорками; применяется к аннотациям типов);
- `@Repeatable` — разрешает повторять одну аннотацию на элементе.

---

### 26.6. Reflection

Аннотацию с `RetentionPolicy.RUNTIME` можно прочитать во время выполнения:

```java
Description annotation = MyClass.class.getAnnotation(Description.class);
if (annotation != null) {
    System.out.println(annotation.value());
}
```

Именно такой подход часто лежит в основе декларативной конфигурации фреймворков.

---

### 26.7. Что сказать на скрининге

Аннотация — это метаинформация о коде. Важно различать саму аннотацию и механизм, который её обрабатывает. `@Target` ограничивает место применения, `@Retention` — время жизни/доступность, а `RUNTIME` позволяет читать аннотацию через reflection.

---

## Связанные заметки

- [[00 - Индекс|Индекс]]
- [[25 - Awaitility|Предыдущая заметка]]
- [[27 - Stream API|Следующая заметка]]
