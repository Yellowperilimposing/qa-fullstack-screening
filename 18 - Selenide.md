---
title: "18. Selenide"
aliases:
  - "Selenide"
  - "18. Selenide"
tags:
  - screening/fs
  - java
  - qa/autotests
  - tools
section: "Инструменты для автотестов"
status: evergreen
updated: 2026-07-21
---
## 18. Selenide

> [!abstract] Коротко
> Selenide — Java-библиотека поверх Selenium WebDriver для UI-автотестов.


**Selenide** — Java-библиотека поверх Selenium WebDriver для UI-автотестов.

Selenide упрощает работу с браузером:

* сам ждет появления элементов;
* дает короткий синтаксис для поиска элементов;
* умеет проверять состояния элементов;
* автоматически делает скриншоты при падении тестов;
* уменьшает количество ручных ожиданий.

---

### 18.1. Жизненный цикл UI-теста на Selenide

Обычно тест на Selenide идет так:

1. Настроить браузер и базовый URL.
2. Открыть страницу через `open()`.
3. Найти элементы через `$` или `$$`.
4. Выполнить действия: `click()`, `setValue()`, `selectOption()`.
5. Проверить результат через `shouldHave`, `shouldBe`, `should`.
6. Закрыть браузер после теста, если это настроено в проекте.

---

### 18.2. Основные методы и проверки

| Метод | Для чего нужен |
| --- | --- |
| `open(url)` | Открывает страницу |
| `$(selector)` | Ищет один элемент |
| `$$(selector)` | Ищет коллекцию элементов |
| `click()` | Кликает по элементу |
| `setValue(text)` | Вводит текст |
| `shouldBe(visible)` | Проверяет состояние элемента |
| `shouldHave(text(...))` | Проверяет текст или другое свойство |
| `exists()` | Проверяет, существует ли элемент |
| `isDisplayed()` | Проверяет, отображается ли элемент сейчас |

---

### 18.3. Частые conditions

| Condition | Что проверяет |
| --- | --- |
| `visible` | Элемент виден |
| `hidden` | Элемент скрыт |
| `enabled` | Элемент доступен |
| `disabled` | Элемент недоступен |
| `text("...")` | Элемент содержит текст |
| `exactText("...")` | Текст полностью совпадает |
| `value("...")` | Значение поля совпадает |
| `exist` | Элемент существует в DOM |

---

### 18.4. Ожидание по умолчанию в Selenide

В Selenide есть ожидание по умолчанию.

По умолчанию Selenide ждет элемент примерно **4 секунды**. Это значение хранится в настройке:

```java
Configuration.timeout
```

Можно изменить timeout:

```java
import com.codeborne.selenide.Configuration;

Configuration.timeout = 10000; // 10 секунд
```

Это значит, что проверки вида `shouldBe`, `shouldHave`, `should` будут ждать выполнение условия до 10 секунд.

Важно: Selenide не делает одну моментальную проверку. Он повторяет проверку, пока условие не выполнится или пока не закончится timeout.

---

### 18.5. Метод should в Selenide

`should` — метод для проверки состояния элемента.

Он принимает condition и ждет, пока условие выполнится.

Примеры:

```java
$("#login").shouldBe(visible);
$("#login").shouldHave(text("Войти"));
$("#login").should(exist);
```

Разница в стиле:

| Метод | Когда удобно использовать |
| --- | --- |
| `shouldBe(condition)` | Когда проверяем состояние: visible, enabled, disabled |
| `shouldHave(condition)` | Когда проверяем содержимое или значение: text, value, cssClass |
| `should(condition)` | Универсальная форма проверки condition |

По смыслу все эти методы запускают ожидание и проверку условия.

---

### 18.6. Символы для XPath в Selenide

Для XPath в Selenide часто используют `$x()`.

```java
$x("//button[text()='Войти']").click();
```

Основные XPath-обозначения:

| Символ | Что означает |
| --- | --- |
| `//` | Искать элемент в любом месте DOM |
| `/` | Переход на следующий уровень вложенности |
| `@` | Обращение к атрибуту |
| `*` | Любой тег |
| `[]` | Условие поиска |
| `text()` | Текст элемента |
| `contains()` | Проверка, что значение содержит текст |

Примеры:

```xpath
//*[@id='login']
```

Любой элемент с `id="login"`.

```xpath
//input[@name='email']
```

`input` с атрибутом `name="email"`.

```xpath
//button[contains(text(), 'Войти')]
```

Кнопка, текст которой содержит `Войти`.

---

### 18.7. Виды ожиданий в Selenide

В Selenide основные ожидания встроены в проверки.

| Вид ожидания | Пример | Что делает |
| --- | --- | --- |
| Ожидание видимости | `shouldBe(visible)` | Ждет, пока элемент станет видимым |
| Ожидание исчезновения | `shouldBe(hidden)` | Ждет, пока элемент исчезнет или станет скрытым |
| Ожидание существования | `should(exist)` | Ждет, пока элемент появится в DOM |
| Ожидание текста | `shouldHave(text("Done"))` | Ждет нужный текст |
| Ожидание значения | `shouldHave(value("test"))` | Ждет нужное значение поля |
| Ожидание доступности | `shouldBe(enabled)` | Ждет, пока элемент станет доступным |

Можно задать timeout для конкретной проверки:

```java
$(".status").shouldHave(text("Готово"), Duration.ofSeconds(10));
```

В Selenide обычно не пишут `Thread.sleep()`, потому что ожидания уже встроены в `should`, `shouldBe`, `shouldHave`.

---

### 18.8. Пример

```java
import org.junit.jupiter.api.Test;

import static com.codeborne.selenide.Condition.text;
import static com.codeborne.selenide.Selenide.$;
import static com.codeborne.selenide.Selenide.open;

class LoginTest {

    @Test
    void shouldLogin() {
        open("/login");

        $("#username").setValue("danil");
        $("#password").setValue("password");
        $("button[type='submit']").click();

        $(".profile").shouldHave(text("Danil"));
    }
}
```

---

### 18.9. Что важно сказать на скрининге

Selenide — это удобная обертка над Selenium для UI-автотестов. Главное отличие — встроенные ожидания и лаконичные проверки состояния элементов через `should`, `shouldBe` и `shouldHave`. По умолчанию Selenide ждет условие около 4 секунд, timeout можно изменить через `Configuration.timeout`. Для XPath используется `$x()`, а для CSS-селекторов — `$()` и `$$()`.
---

## Связанные заметки

- [[00 - Индекс|Индекс]]
- [[17 - JUnit|Предыдущая заметка]]
- [[19 - AssertJ|Следующая заметка]]
