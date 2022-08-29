# План автоматизации

## Перечень автоматизируемых сценариев

### Валидные данные:
- номер карты: 16 цифр.
- месяц: 2 цифры в диапазоне 01-12 (не ранее текущего месяца текущего года).
- год: 2 цифры (не ранее текущего года и не позже 5 лет с текущего года).
- владелец: буквы латинского алфавита, имя и фамилия.
- CVC: 3 цифры.

### Позитивные сценарии:
**1.** Заполнить форму покупки тура валидными данными, оплата по дебетовой карте (номер карты: 4444 4444 4444 4441)

***Ожидаемый результат.*** Заявка одобрена банком, сообщение: "Успешно! Операция одобрена Банком". В СУБД payment_entity сохраняется информация о том, каким способом был совершён платёж и успешно ли он был совершён, статус "APPROVED".

**2.** Заполнить форму покупки тура валидными данными, покупка в кредит (номер карты: 4444 4444 4444 4441)

***Ожидаемый результат.*** Заявка одобрена банком, сообщение: "Успешно! Операция одобрена Банком". В СУБД credit_request_entity сохраняется информация о том, каким способом был совершён платёж и успешно ли он был совершён, статус "APPROVED".

**3.** Заполнить форму покупки тура валидными данными, оплата по дебетовой карте (номер карты: 4444 4444 4444 4442)

***Ожидаемый результат.*** Всплывающее окно "Ошибка! Банк отказал в проведении операции". В СУБД payment_entity сохраняется информация о том, каким способом был совершён платёж и успешно ли он был совершён, статус "DECLINED".

**4.** Заполнить форму покупки тура валидными данными, оплата по кредитной карте (номер карты: 4444 4444 4444 4442)

***Ожидаемый результат.*** Всплывающее окно "Ошибка! Банк отказал в проведении операции". В СУБД credit_request_entity сохраняется информация о том, каким способом был совершён платёж и успешно ли он был совершён, статус "DECLINED".

### Негативные сценарии:

**1.** Отправка пустой формы покупки тура, оплата по дебетовой карте

***Ожидаемый результат.*** Все поля подсвечены красным цветом, сообщения об ошибке: «Неверный формат», «Поле обязательно для заполнения».

**2.** Отправка пустой формы покупки тура, оплата по кредитной карте

***Ожидаемый результат.*** Все поля подсвечены красным цветом, сообщения об ошибке: «Неверный формат», «Поле обязательно для заполнения».

**3.** Оплата по дебетовой карте с истекшим сроком действия (месяц, предшествующий текущему; текущий год)

***Ожидаемый результат.*** Поле "Месяц" подсвечено красным, сообщение об ошибке "Неверно указан срок действия карты".

**4.** Оплата по кредитной карте с истекшим сроком действия (месяц, предшествующий текущему; текущий год)

***Ожидаемый результат.*** Поле "Месяц" подсвечено красным, сообщение об ошибке "Неверно указан срок действия карты".

**5.** Оплата по дебетовой карте с истекшим сроком действия (введен год ранее текущего)

***Ожидаемый результат.*** Поле "Год" подсвечено красным, сообщение об ошибке "Истёк срок действия карты".

**6.** Оплата по кредитной карте с истекшим сроком действия (введен год ранее текущего)

***Ожидаемый результат.*** Поле "Год" подсвечено красным, сообщение об ошибке "Истёк срок действия карты".

**7.** Валидация поля "Номер карты": пустое поле, ввод букв, ввод некорректных символов, ввод менее 16 цифр, ввод более 16 цифр, ввод только пробелов, ввод несуществующего номера карты, ввод нулей.

***Ожидаемый результат.*** Номер карты невалидный. Поле «Номер карты» подсвечивается красным цветом, сообщение об ошибке: «Неверный формат». Ввод некорректных символов недоступен.

**8.** Валидация поля "Месяц": пустое поле, ввод букв, ввод некорректных символов, ввод менее 2 цифр, ввод более 2 цифр, ввод цифр не входящих в валидный диапазон (01-12), ввод только пробелов, ввод нулей.

***Ожидаемый результат.*** Значение поля "Месяц" невалидно. Поле «Месяц» подсвечивается красным цветом, сообщение об ошибке: «Неверно указан срок действия карты». Ввод некорректных символов недоступен.

**9.** Валидация поля "Год": пустое поле, ввод букв, ввод некорректных символов, ввод менее 2 цифр, ввод более 2 цифр, ввод цифр не входящих в валидный диапазон (текущий год + 5 лет), ввод только пробелов, ввод нулей.

***Ожидаемый результат.*** Значение поля "Год" невалидно. Поле «Год» подсвечивается красным цветом, сообщение об ошибке: «Неверно указан срок действия карты». Ввод некорректных символов недоступен.

**10.** Валидация поля "Владелец": ввод букв на кириллице, некорректные символы, имя из 1 буквы, цифры в имени, ввод только пробелов, ввод только имени, ввод только фамилии.

***Ожидаемый результат.*** Значение поля "Владелец" невалидно. Поле «Владелец» подсвечивается красным цветом, сообщение об ошибке: «Поле обязательно для заполнения». Ввод некорректных символов недоступен.

**11.** Валидация поля "CVC/CVV": пустое поле, ввод букв, ввод некорректных символов, ввод менее 3 цифр, ввод более 3 цифр, ввод только пробелов, ввод нулей.

***Ожидаемый результат.*** Значение поля "CVC/CVV" невалидно. Поле «CVC/CVV» подсвечивается красным цветом, сообщение об ошибке: «Неверный формат». Ввод некорректных символов недоступен.


## Перечень используемых инструментов с обоснованием выбора

-	*Java* - язык программирования для написания автотестов.
-	*IntelliJ IDEA* - интеллектуальная среда разработки, которая понимает код.
-	*Gradle* - система автоматической сборки.
-	*JUnit 5* - библиотека для тестирования.
-	*Faker* - генерация рандомных данных (имя, номер телефона,электронный адрес).
-	*Selenide* - фреймворк для автоматизированного тестирования.
-	*Allure* - фреймворк для генерации отчета.
-	*Git* - распределённая система управлениями версиями.
-	*GitHub* - используется для хранения кода, в том числе автотестов.
-	*Docker* - система контейнерезации. Позволит подключить и использовать данные из БД.
-	*DBeaver* - инструмент для работы с базами данных.

## Перечень и описание возможных рисков при автоматизации

-   Проблемы с запуском приложения, подключением БД.
-	Отсутствие id у элементов формы.
-	Отсутствие документации к тестируемому приложению.

## Интервальная оценка с учётом рисков (в часах)

Необходимое время на тестирование составляет с учетом рисков - 116 часов:

- подготовка окружения, развертывание БД - 8 часов;
- написание автотестов и отладка - 96 часов;
- формирование отчетов - 12 часов;

## План сдачи работ

- 26.07 - сдача дипломному руководителю работу на проверку без составления отчетов

