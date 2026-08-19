---
title: "21. Appium"
aliases:
  - "Appium"
  - "21. Appium"
tags:
  - screening/fs
  - java
  - qa/autotests
  - tools
section: "Инструменты для автотестов"
status: evergreen
updated: 2026-07-21
---
## 21. Appium

> [!abstract] Коротко
> Appium — инструмент для автоматизации мобильных приложений.


**Appium** — инструмент для автоматизации мобильных приложений.

С помощью Appium тестируют:

* Android-приложения;
* iOS-приложения;
* мобильные браузеры;
* гибридные приложения.

Appium использует WebDriver-подход: тест отправляет команды на Appium Server, а сервер управляет приложением на устройстве или эмуляторе.

---

### 21.1. Жизненный цикл Appium-теста

Типичный жизненный цикл:

1. Запустить Appium Server.
2. Подготовить устройство, эмулятор или симулятор.
3. Описать capabilities: платформа, устройство, приложение.
4. Создать `AppiumDriver`.
5. Найти элементы на экране.
6. Выполнить действия: tap, input, swipe.
7. Проверить результат.
8. Закрыть сессию через `driver.quit()`.

---

### 21.2. Основные capabilities

| Capability | Для чего нужен |
| --- | --- |
| `platformName` | Платформа: Android или iOS |
| `deviceName` | Имя устройства |
| `app` | Путь к приложению |
| `appPackage` | Package Android-приложения |
| `appActivity` | Activity Android-приложения |
| `automationName` | Движок автоматизации, например UiAutomator2 |
| `noReset` | Не сбрасывать состояние приложения между тестами |

---

### 21.3. Основные действия

| Действие | Что делает |
| --- | --- |
| `findElement()` | Ищет элемент |
| `click()` | Нажимает на элемент |
| `sendKeys()` | Вводит текст |
| `getText()` | Получает текст |
| `isDisplayed()` | Проверяет видимость |
| `driver.quit()` | Закрывает Appium-сессию |

---

### 21.4. Пример

```java
import io.appium.java_client.AppiumDriver;
import io.appium.java_client.android.AndroidDriver;
import org.openqa.selenium.By;
import org.openqa.selenium.remote.DesiredCapabilities;

import java.net.URL;

public class LoginMobileTest {

    public void shouldLogin() throws Exception {
        DesiredCapabilities capabilities = new DesiredCapabilities();
        capabilities.setCapability("platformName", "Android");
        capabilities.setCapability("deviceName", "emulator-5554");
        capabilities.setCapability("automationName", "UiAutomator2");
        capabilities.setCapability("appPackage", "com.example.app");
        capabilities.setCapability("appActivity", ".MainActivity");

        AppiumDriver driver = new AndroidDriver(
                new URL("http://127.0.0.1:4723"),
                capabilities
        );

        driver.findElement(By.id("login")).sendKeys("danil");
        driver.findElement(By.id("password")).sendKeys("password");
        driver.findElement(By.id("submit")).click();

        driver.quit();
    }
}
```

---

### 21.5. Что важно сказать на скрининге

Appium нужен для мобильных автотестов. Тест создает сессию с устройством через capabilities, управляет приложением через driver и обязательно закрывает сессию после выполнения.
---

## Связанные заметки

- [[00 - Индекс|Индекс]]
- [[20 - Lombok|Предыдущая заметка]]
- [[22 - Maven|Следующая заметка]]
