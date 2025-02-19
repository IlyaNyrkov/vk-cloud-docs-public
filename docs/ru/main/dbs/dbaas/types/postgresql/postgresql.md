## Описание

PostgreSQL — это объектно-реляционная система управления базами данных (ОРСУБД, ORDBMS), основанная на POSTGRES.

PostgreSQL — СУБД с открытым исходным кодом. Она поддерживает большую часть стандарта SQL и предлагает множество современных функций:

- сложные запросы
- внешние ключи
- триггеры
- изменяемые представления
- транзакционная целостность
- многоверсионность

Кроме того, можно сильно расширять возможности PostgreSQL, например, создавая свои сущности:

- типы данных
- функции
- операторы
- агрегатные функции
- методы индексирования
- процедурные языки

## Запуск инстанса

Инстансы СУБД запускаются в личном кабинете, [в разделе «Базы данных»](https://mcs.mail.ru/app/services/databases/) кнопкой "Создать базу данных" или "Добавить".

![](./assets/1597318730698-1597318730698.png)

На следующем шаге нужно выбрать тип базы данных для запуска, а также шаблон конфигурации - "Single", "Master-Slave" или "Кластер".

![](./assets/1603324948973-1603324948973.png)

Доступность шаблонов конфигурации зависит от типа выбранной СУБД.

- Single — единичный инстанс СУБД без реплики. Рекомендуется использовать исключительно для целей разработки и тестирования.
- Master-slave — два инстанса СУБД в разных ЦОД с репликацией в режиме master-slave (active-passive). Используйте для промышленной эксплуатации.
- Кластер — кластер с синхронной репликацией данных. Используйте при наличии повышенных требований к надежности и отказоустойчивости системы.

На втором шаге нужно выбрать параметры инстанса:

<table><tbody><tr><td>Тип виртуальной машины</td><td>Количество оперативной памяти и число виртуальных процессоров.</td></tr><tr><td>Размер диска</td><td>По умолчанию подставляется объем из выбранной конфигурации. Но при желании можно увеличить или уменьшить размер диска.</td></tr><tr><td>Имя инстанса</td><td>Можно оставить имя по умолчанию или ввести своё название латинскими символами.</td></tr><tr><td>Зона доступности</td><td>Географическое расположение серверов (рекомендуем DP1 или MS1).</td></tr><tr><td>Сеть</td><td>Можно оставить по умолчанию или выбрать свою приватную сеть.</td></tr><tr><td>Назначить внешний IP</td><td>Для организации доступа к базе данных из Интернет через плавающий IP-адрес нужно включить опцию "Назначить внешний IP".</td></tr><tr><td>Создать реплику</td><td>Переключатель для создания read-only реплики для нового инстанса.</td></tr><tr><td>Ключ для доступа по SSH</td><td>Можно выбрать из уже созданных ранее или создатьн новый. Он понадобится для подключения к серверу.</td></tr></tbody></table>

На следующем шаге нужно задать параметры инициализации базы данных - уникальную комбинацию логин+пароль и имя СУБД.

![](./assets/1603325403389-1603325403389.png)

Инстанс будет создаваться в течение нескольких минут и после создания отобразится информация о способах подключения к нему.

![](./assets/1603325738547-1603325738547.png)

## Подключение к инстансу

Увидеть способы подключения можно при клике на название инстанса в списке, или наведя курсор на значок информации.

![](./assets/1597319083077-1597319083077.png)

В разделе «Параметры подключения» приведены примеры кода из популярных языков, с помощью которых можно управлять содержимым базы данных.

![](./assets/1597319118659-1597319118659.png)

При использовании примеров следует заменить макросы `<DATABASE>`,`<USERNAME>`,`<PASSWORD>` на актуальные значения:

- `<DATABASE>` — название базы данных, указанное при создании.
- `<USERNAME>` — имя пользователя (указывается при создании).
- `<PASSWORD>` — пароль пользователя (указывается при создании).

Дополнительную информацию можно найти в документации по используемому коннектору.

## Действия с инстансом

При наведении курсора в списке инстансов на три точки (контекстное меню) появляются доступные действия для конкретного инстанса:

![](./assets/1597319167692-1597319167692.png)
