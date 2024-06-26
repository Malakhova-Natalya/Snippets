## Как разбираться с редкими/непонятными ошибками dbt (на примере ошибки Enum8)
Если при запуске моделей dbt появляется ошибка такого рода:

![cover](https://github.com/Malakhova-Natalya/Snippets/blob/main/dbt/dbt_error_Enum8/dbt%20-%20error%20Enum8.png)

проверьте, в каком терминале вы работаете - возможно, в powershell + у вас не включено облачное окружение Python?

Как избежать этой ошибки:

1. Заходим в Ubuntu
2. Активируем облачное окружение python

Вот пример моей инструкции из реального проекта:
![cover](https://github.com/Malakhova-Natalya/Snippets/blob/main/dbt/dbt_error_Enum8/ubuntu%20%2B%20python.png)

Если ошибка не проходит, возможно, дело в **версии ПО**, например, ClickHouse.
Проверить версию ClickHouse можно перейдя в → DBeaver → SQL-запрос в интересующей БД:

    SELECT version()

Так вы узнаете свою версию ClickHouse, например у меня это 22.8.4.7 на проекте по работе и 24.1.4.20 на localhost.

В разных версиях команда 

    SELECT table_type FROM information_schema.tables

может выдать результат в разном формате. Например, для моих версий ClickHouse это формат String:

![cover](https://github.com/Malakhova-Natalya/Snippets/blob/main/dbt/dbt_error_Enum8/table_type.png)

а для других версий это может быть Enum8, при чтении которого в некоторых макросах могут появляться проблемы.

Как это исправить? (если вы не хотите менять БД на новую версию)

Можно исправить код нужных макросов в dbt-проекте.

Например, на моей практике возникла проблема с макросом get_table_types_sql. Находим такой макрос: 

    dbt_packages > dbt_utils > macros > sql > get_table_types_sql

и меняем ему формат вывода на String например вот так:

![cover](https://github.com/Malakhova-Natalya/Snippets/blob/main/dbt/dbt_error_Enum8/get_table_types_sql.png)

        {% macro default__get_table_types_sql() %}
            case toString(table_type)
                when 'BASE TABLE' then 'table'
                when 'EXTERNAL TABLE' then 'external'
                when 'MATERIALIZED VIEW' then 'materializedview'
                else lower(toString(table_type))
            end as {{ adapter.quote('table_type') }}
        {% endmacro %}

## Как найти и исправить другие ошибки

Если модель падает, и непонятно, чем именно вызвана ошибка, можно применить следующий подход:

на протяжении кода расставить между некоторыми разделами (например, между блоками, где вызываются макросы) строчки такого рода:

        
        {{ log(111, true) }}
        ...
        {{ log(222, true) }}
        ...
        {{ log(333, true) }}

потом сохранить, запустить модель - и до фатальной ошибки вы увидите некоторые из этих значений. Например, 111 и 222. А то, которое не увидите - например, 333 - значит до него дело не дошло. Значит фатальное место было до той строки. Так вы сузите круг подозреваемых и сможете разобраться в источнике проблемы.

Что здесь важно по синтаксису: внутри log должно быть сначала число, потом true. Например, можно написать 1, 10, 11, 100, 111 а вот 01 или 001 написать нельзя, так не сработает (нуля перед числом не должно быть).

Если нужно посмотреть какую-то переменную, например table_pattern, то можно вывести её так:

        {{ log(table_pattern, true) }}

А можно вывести свой текст:

        {{ log('hello', true) }}
