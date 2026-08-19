---
title: "20. Lombok"
aliases:
  - "Lombok"
  - "20. Lombok"
tags:
  - screening/fs
  - java
  - qa/autotests
  - tools
section: "Инструменты для автотестов"
status: evergreen
updated: 2026-07-21
---
## 20. Lombok

> [!abstract] Коротко
> Lombok — библиотека, которая генерирует шаблонный Java-код через аннотации.


**Lombok** — библиотека, которая генерирует шаблонный Java-код через аннотации.

Lombok помогает не писать вручную:

* getters;
* setters;
* constructors;
* `toString()`;
* `equals()` и `hashCode()`;
* builder.

---

### 20.1. Как работает Lombok

Жизненный цикл использования Lombok:

1. Разработчик добавляет аннотацию к классу, полю или конструктору.
2. Во время компиляции Lombok генерирует нужный код.
3. В исходном `.java` файле этого кода не видно.
4. В скомпилированном `.class` файле методы уже есть.

---

### 20.2. Основные аннотации

| Аннотация | Что генерирует |
| --- | --- |
| `@Getter` | Getter-методы |
| `@Setter` | Setter-методы |
| `@ToString` | Метод `toString()` |
| `@EqualsAndHashCode` | Методы `equals()` и `hashCode()` |
| `@NoArgsConstructor` | Конструктор без параметров |
| `@AllArgsConstructor` | Конструктор со всеми полями |
| `@RequiredArgsConstructor` | Конструктор для `final` и `@NonNull` полей |
| `@Data` | Getter, setter, `toString`, `equals`, `hashCode` |
| `@Builder` | Builder для создания объекта |
| `@Slf4j` | Logger `log` |

---

### 20.3. Пример

```java
import lombok.AllArgsConstructor;
import lombok.Getter;
import lombok.Setter;

@Getter
@Setter
@AllArgsConstructor
public class User {
    private String name;
    private int age;
}
```

Можно использовать так:

```java
User user = new User("Danil", 28);

System.out.println(user.getName());
user.setAge(29);
```

---

### 20.4. Пример с Builder

```java
import lombok.Builder;
import lombok.Getter;

@Getter
@Builder
public class User {
    private String name;
    private int age;
}
```

```java
User user = User.builder()
        .name("Danil")
        .age(28)
        .build();
```

---

### 20.5. Что важно сказать на скрининге

Lombok уменьшает boilerplate-код, но важно понимать, какой код он генерирует. Например, `@Data` удобна, но не всегда подходит для сложных сущностей, потому что сразу генерирует много методов.
---

## Связанные заметки

- [[00 - Индекс|Индекс]]
- [[19 - AssertJ|Предыдущая заметка]]
- [[21 - Appium|Следующая заметка]]
