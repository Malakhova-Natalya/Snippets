
Если при запуске моделей dbt появляется ошибка такого рода как:

![cover](https://github.com/Malakhova-Natalya/Snippets/blob/main/dbt_error_Enum8/dbt%20-%20error%20Enum8.png)

проверьте, в каком терминале вы работаете - возможно, в powershell + у вас не включено облачное окружение Python?

Как избежать этой ошибки:

1. Заходим в Ubuntu
2. Активируем облачное окружение python

Вот пример моей инструкции из реального проекта:
![cover](https://github.com/Malakhova-Natalya/Snippets/blob/main/dbt_error_Enum8/ubuntu%20%2B%20python.png)

Если ошибка не проходит, возможно, дело в версии ПО, например, ClickHouse.
Проверить версию ClickHouse можно перейдя в → DBeaver → SQL-запрос в интересующей БД:

    SELECT version()

Так вы узнаете свою версию ClickHouse, например у меня это 22.8.4.7 на проекте по работе и 24.1.4.20 на localhost.

В разных версиях команда 

    SELECT table_type FROM information_schema.tables

может выдать результат в разном формате. Например, для моих версий ClickHouse это формат String:

![cover]()
а для других версий это может быть Enum8
