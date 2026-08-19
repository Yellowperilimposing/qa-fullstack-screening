---
title: "24. XPath, id, class и CSS-селекторы"
aliases:
  - "XPath, id, class и CSS-селекторы"
  - "24. XPath, id, class и CSS-селекторы"
tags:
  - screening/fs
  - java
  - qa/autotests
  - tools
section: "Инструменты для автотестов"
status: evergreen
updated: 2026-07-21
---
## 24. XPath, id, class и CSS-селекторы

> [!abstract] Коротко
> Эти инструменты нужны, чтобы находить элементы на странице в UI-автотестах.


Эти инструменты нужны, чтобы находить элементы на странице в UI-автотестах.

Например:

* найти поле логина;
* кликнуть по кнопке;
* проверить текст ошибки;
* выбрать элемент из списка.

---

### 24.1. Что такое id

`id` — это уникальный идентификатор HTML-элемента.

Пример HTML:

```html
<input id="email">
```

CSS-селектор по `id`:

```css
#email
```

Selenide:

```java
$("#email").setValue("test@mail.com");
```

Selenium:

```java
driver.findElement(By.id("email")).sendKeys("test@mail.com");
```

`id` обычно самый удобный и стабильный способ найти элемент, если он есть и не генерируется случайно.

---

### 24.2. Что такое class

`class` — это CSS-класс элемента.

Один и тот же класс может быть у многих элементов.

Пример HTML:

```html
<div class="error-message">Неверный пароль</div>
```

CSS-селектор по `class`:

```css
.error-message
```

Selenide:

```java
$(".error-message").shouldHave(text("Неверный пароль"));
```

Selenium:

```java
driver.findElement(By.className("error-message"));
```

Если у элемента несколько классов:

```html
<button class="btn primary active">Войти</button>
```

CSS-селектор:

```css
.btn.primary.active
```

---

### 24.3. Что такое CSS-селектор

**CSS-селектор** — это выражение, которое описывает, какой элемент нужно найти.

CSS-селекторы могут искать по:

* тегу;
* `id`;
* `class`;
* атрибуту;
* вложенности;
* комбинации признаков.

Примеры:

| CSS-селектор | Что ищет |
| --- | --- |
| `button` | Все кнопки |
| `#login` | Элемент с `id="login"` |
| `.error` | Элементы с классом `error` |
| `input[name='email']` | `input` с атрибутом `name="email"` |
| `button[type='submit']` | Кнопку с `type="submit"` |
| `div.error` | `div` с классом `error` |
| `#login-form button` | Кнопку внутри элемента `#login-form` |

Selenide:

```java
$("button[type='submit']").click();
```

Selenium:

```java
driver.findElement(By.cssSelector("button[type='submit']")).click();
```

---

### 24.4. Что такое XPath

**XPath** — это язык для поиска элементов в XML/HTML-документе.

В автотестах XPath используют, когда нужно найти элемент:

* по тексту;
* по атрибуту;
* по структуре DOM;
* рядом с другим элементом;
* когда CSS-селектора недостаточно.

Примеры:

| XPath | Что ищет |
| --- | --- |
| `//*[@id='login']` | Любой элемент с `id="login"` |
| `//input[@name='email']` | `input` с `name="email"` |
| `//button[text()='Войти']` | Кнопку с точным текстом `Войти` |
| `//button[contains(text(), 'Войти')]` | Кнопку, текст которой содержит `Войти` |
| `//div[contains(@class, 'error')]` | `div`, у которого в `class` есть `error` |

Selenide:

```java
$x("//button[text()='Войти']").click();
```

Selenium:

```java
driver.findElement(By.xpath("//button[text()='Войти']")).click();
```

---

### 24.5. CSS-селектор vs XPath

| Что сравниваем | CSS-селектор | XPath |
| --- | --- | --- |
| Поиск по `id` и `class` | Удобно | Можно |
| Поиск по атрибутам | Удобно | Удобно |
| Поиск по тексту | Ограниченно | Удобно |
| Поиск вверх по DOM | Нельзя или неудобно | Можно |
| Читаемость | Обычно проще | Может быть сложнее |
| Скорость | Обычно хороший выбор | Тоже норм, но часто тяжелее читается |

Обычно лучше начинать с CSS-селектора.

XPath использовать, когда:

* нужно искать по тексту;
* нужно идти к родителю или соседнему элементу;
* нет нормального `id`, `class` или стабильного атрибута.

---

### 24.6. Хорошие и плохие локаторы

Хорошо:

```css
#email
[data-testid='login-button']
button[type='submit']
```

```xpath
//button[text()='Войти']
//input[@name='email']
```

Плохо:

```css
div > div > div > button
```

```xpath
/html/body/div[2]/div[1]/form/button
```

Почему плохо:

* сильно зависит от верстки;
* легко ломается при изменении страницы;
* плохо читается.

---

### 24.7. Что важно сказать на скрининге

`id` и `class` — это атрибуты HTML-элементов.

CSS-селектор — способ найти элемент по тегу, `id`, `class`, атрибутам или вложенности.

XPath — язык поиска элементов по DOM-структуре, атрибутам и тексту.

В автотестах Selenium, Selenide и Appium используют CSS-селекторы и XPath как локаторы для поиска элементов.
---

## Связанные заметки

- [[00 - Индекс|Индекс]]
- [[23 - Page Object Model и Page Factory|Предыдущая заметка]]
- [[25 - Awaitility|Следующая заметка]]
