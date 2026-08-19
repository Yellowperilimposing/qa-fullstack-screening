---
title: 22. Maven
aliases:
  - Maven
  - 22. Maven
tags:
  - screening/fs
  - java
  - qa/autotests
  - tools
section: Инструменты для автотестов
status: evergreen
updated: 2026-07-21
---
## 22. Maven

> [!abstract] Коротко
> Maven — инструмент для сборки Java-проектов и управления зависимостями.


**Maven** — инструмент для сборки Java-проектов и управления зависимостями.

Maven помогает:

* подключать библиотеки;
* запускать тесты;
* собирать проект;
* управлять версиями зависимостей;
* хранить настройки проекта в одном файле `pom.xml`.

---

### 22.1. Что такое pom.xml

`pom.xml` — главный конфигурационный файл Maven-проекта.

В нем обычно указывают:

* координаты проекта;
* зависимости;
* плагины;
* версии Java;
* настройки сборки;
* профили.

Минимальный пример:

```xml
<project>
    <modelVersion>4.0.0</modelVersion>

    <groupId>ru.example</groupId>
    <artifactId>demo-tests</artifactId>
    <version>1.0-SNAPSHOT</version>
</project>
```

---

### 22.2. Координаты Maven-проекта

| Поле | Что означает |
| --- | --- |
| `groupId` | Группа или организация проекта |
| `artifactId` | Имя артефакта или модуля |
| `version` | Версия проекта |

Пример:

```xml
<groupId>ru.danil</groupId>
<artifactId>ui-tests</artifactId>
<version>1.0-SNAPSHOT</version>
```

---

### 22.3. Зависимости

Зависимости подключаются в блоке `dependencies`.

Пример JUnit:

```xml
<dependencies>
    <dependency>
        <groupId>org.junit.jupiter</groupId>
        <artifactId>junit-jupiter</artifactId>
        <version>5.10.2</version>
        <scope>test</scope>
    </dependency>
</dependencies>
```

`scope` показывает, где используется зависимость.

| Scope | Для чего |
| --- | --- |
| `compile` | Нужна при компиляции и запуске |
| `test` | Нужна только для тестов |
| `provided` | Нужна при компиляции, но будет предоставлена окружением |
| `runtime` | Нужна только при запуске |

---

### 22.4. Жизненный цикл Maven

Maven работает через фазы жизненного цикла.

Основные фазы:

| Фаза | Что делает |
| --- | --- |
| `validate` | Проверяет корректность проекта |
| `compile` | Компилирует основной код |
| `test` | Запускает тесты |
| `package` | Собирает артефакт, например `.jar` |
| `verify` | Выполняет дополнительные проверки |
| `install` | Кладет артефакт в локальный репозиторий |
| `deploy` | Публикует артефакт в удаленный репозиторий |

Если запустить позднюю фазу, Maven выполнит и предыдущие.

Например:

```bash
mvn test
```

выполнит подготовку, компиляцию и запуск тестов.

---

### 22.5. Частые команды

| Команда | Что делает |
| --- | --- |
| `mvn clean` | Удаляет папку `target` |
| `mvn compile` | Компилирует проект |
| `mvn test` | Запускает тесты |
| `mvn package` | Собирает проект |
| `mvn clean test` | Очищает проект и запускает тесты |
| `mvn clean install` | Собирает и кладет артефакт в локальный репозиторий |

---

### 22.6. Плагины

Плагины расширяют возможности Maven.

Частые плагины:

| Плагин | Для чего |
| --- | --- |
| `maven-compiler-plugin` | Настраивает версию Java для компиляции |
| `maven-surefire-plugin` | Запускает unit-тесты |
| `maven-failsafe-plugin` | Запускает integration-тесты |

Пример настройки Java:

```xml
<build>
    <plugins>
        <plugin>
            <groupId>org.apache.maven.plugins</groupId>
            <artifactId>maven-compiler-plugin</artifactId>
            <version>3.11.0</version>
            <configuration>
                <source>17</source>
                <target>17</target>
            </configuration>
        </plugin>
    </plugins>
</build>
```

---

### 22.7. Что важно сказать на скрининге

Maven — это инструмент сборки и управления зависимостями. Главный файл Maven-проекта — `pom.xml`. Зависимости подключаются через `dependencies`, сборка идет по жизненному циклу, а тесты чаще всего запускают командой `mvn test` или `mvn clean test`.
---

## Связанные заметки

- [[00 - Индекс|Индекс]]
- [[21 - Appium|Предыдущая заметка]]
- [[23 - Page Object Model и Page Factory|Следующая заметка]]
