---
title: "23. Page Object Model и Page Factory"
aliases:
  - "Page Object Model и Page Factory"
  - "23. Page Object Model и Page Factory"
tags:
  - screening/fs
  - java
  - qa/autotests
  - tools
section: "Инструменты для автотестов"
status: evergreen
updated: 2026-07-21
---
## 23. Page Object Model и Page Factory

> [!abstract] Коротко
> Page Object Model — паттерн проектирования автотестов, где каждая страница или крупный блок интерфейса описывается отдельным классом.


**Page Object Model** — паттерн проектирования автотестов, где каждая страница или крупный блок интерфейса описывается отдельным классом.

Главная идея: тест не должен напрямую знать все локаторы и детали страницы. Он должен работать с понятными методами page object.

---

### 23.1. Зачем нужен Page Object Model

Page Object Model помогает:

* убрать локаторы из тестов;
* не дублировать код работы со страницей;
* сделать тесты читаемыми;
* проще поддерживать UI-тесты при изменениях верстки;
* разделить тестовую логику и логику взаимодействия со страницей.

Без Page Object тест может выглядеть так:

```java
driver.findElement(By.id("username")).sendKeys("danil");
driver.findElement(By.id("password")).sendKeys("password");
driver.findElement(By.cssSelector("button[type='submit']")).click();
```

С Page Object:

```java
loginPage.login("danil", "password");
```

---

### 23.2. Структура Page Object

Обычно Page Object содержит:

* локаторы элементов;
* методы действий;
* методы проверок;
* иногда переходы на другие страницы.

Пример:

```java
import org.openqa.selenium.By;
import org.openqa.selenium.WebDriver;

public class LoginPage {

    private final WebDriver driver;

    private final By usernameInput = By.id("username");
    private final By passwordInput = By.id("password");
    private final By submitButton = By.cssSelector("button[type='submit']");

    public LoginPage(WebDriver driver) {
        this.driver = driver;
    }

    public void login(String username, String password) {
        driver.findElement(usernameInput).sendKeys(username);
        driver.findElement(passwordInput).sendKeys(password);
        driver.findElement(submitButton).click();
    }
}
```

Тест:

```java
LoginPage loginPage = new LoginPage(driver);

loginPage.login("danil", "password");
```

---

### 23.3. Хорошие правила Page Object

* Один Page Object описывает одну страницу или логический блок.
* Локаторы хранятся внутри Page Object, а не в тесте.
* Методы называются по бизнес-действиям: `login()`, `openProfile()`, `addProductToCart()`.
* Page Object не должен содержать assertion-логику, если в проекте принято держать проверки в тестах.
* Если действие ведет на новую страницу, метод может возвращать новый Page Object.

Пример:

```java
public ProfilePage login(String username, String password) {
    driver.findElement(usernameInput).sendKeys(username);
    driver.findElement(passwordInput).sendKeys(password);
    driver.findElement(submitButton).click();

    return new ProfilePage(driver);
}
```

---

### 23.4. Page Factory

**Page Factory** — механизм Selenium, который позволяет инициализировать элементы страницы через аннотацию `@FindBy`.

Вместо `By`-локаторов можно писать поля:

```java
@FindBy(id = "username")
private WebElement usernameInput;
```

Элементы инициализируются через:

```java
PageFactory.initElements(driver, this);
```

---

### 23.5. Пример Page Factory

```java
import org.openqa.selenium.WebDriver;
import org.openqa.selenium.WebElement;
import org.openqa.selenium.support.FindBy;
import org.openqa.selenium.support.PageFactory;

public class LoginPage {

    @FindBy(id = "username")
    private WebElement usernameInput;

    @FindBy(id = "password")
    private WebElement passwordInput;

    @FindBy(css = "button[type='submit']")
    private WebElement submitButton;

    public LoginPage(WebDriver driver) {
        PageFactory.initElements(driver, this);
    }

    public void login(String username, String password) {
        usernameInput.sendKeys(username);
        passwordInput.sendKeys(password);
        submitButton.click();
    }
}
```

---

### 23.6. Page Object Model vs Page Factory

| Подход | Что это |
| --- | --- |
| Page Object Model | Паттерн организации UI-тестов |
| Page Factory | Способ инициализации элементов в Selenium |

То есть Page Factory не заменяет Page Object Model. Page Factory можно использовать внутри Page Object.

---

### 23.7. Page Object Model в Selenide

В Selenide Page Object обычно проще, потому что есть `$`, `SelenideElement` и встроенные ожидания.

```java
import com.codeborne.selenide.SelenideElement;

import static com.codeborne.selenide.Selenide.$;

public class LoginPage {

    private final SelenideElement usernameInput = $("#username");
    private final SelenideElement passwordInput = $("#password");
    private final SelenideElement submitButton = $("button[type='submit']");

    public void login(String username, String password) {
        usernameInput.setValue(username);
        passwordInput.setValue(password);
        submitButton.click();
    }
}
```

---

### 23.8. Что важно сказать на скрининге

Page Object Model — это паттерн для организации UI-автотестов: локаторы и действия страницы выносятся в отдельный класс. Page Factory — это механизм Selenium с `@FindBy` и `PageFactory.initElements`, который помогает инициализировать элементы внутри Page Object.
---

## Связанные заметки

- [[00 - Индекс|Индекс]]
- [[22 - Maven|Предыдущая заметка]]
- [[24 - XPath, id, class и CSS-селекторы|Следующая заметка]]
