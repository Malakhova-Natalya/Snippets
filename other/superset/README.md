## Superset

### Как добавить фильтр даты внутрь виртуальной таблицы

Предположим, у нас есть задача сделать график big number, который показывал бы сумму подписчиков на текущий день (последнюю дату). При этом на график должны действовать фильтры по дате и по источнику (соцсети).

То есть, например, у нас есть такие данные:

    date           Source  factFollowers   factFollowers_fixed
    2024-07-01     TG      55917	       55917
    2024-07-02     TG      0	       55917
    2024-07-03     TG      0	       55917
    2024-07-01     VK      503390	       503390
    2024-07-02     VK      503384	       503384
    2024-07-03     VK      503372	       503372

И нам нужно отобрать из них и просуммировать такие данные:

    date           Source  factFollowers   factFollowers_fixed
    2024-07-03     TG      0	       55917
    2024-07-03     VK      503372	       503372

Как нам указать, что нас интересует последняя дата?

Например, можно использовать ROW_NUMBER() :

        SELECT date, Source, factFollowers, factFollowers_fixed, 
        ROW_NUMBER () OVER (PARTITION BY Source ORDER BY date DESC) AS _rn
        FROM my_table
        WHERE date >= toDate('2024-07-01') AND date < toDate('2024-07-04')
        ORDER BY 2, 1

И получить:

    date           Source  factFollowers   factFollowers_fixed     _rn
    2024-07-01     TG      55917	       55917                   3
    2024-07-02     TG      0	       55917                   2
    2024-07-03     TG      0	       55917                   1
    2024-07-01     VK      503390	       503390                  3
    2024-07-02     VK      503384	       503384                  2
    2024-07-03     VK      503372	       503372                  1
        
Чтобы можно было по условию _rn=1 отобрать нужные строки:

    date           Source  factFollowers   factFollowers_fixed     _rn
    2024-07-03     TG      0	       55917                   1
    2024-07-03     VK      503372	       503372                  1

Итоговый запрос в таком случае выглядел бы, например, так:

    SELECT SUM(factFollowers_fixed) FILTER(WHERE _rn=1) as _max_factFollowers 
    FROM (
        SELECT date, Source, factFollowers, factFollowers_fixed, 
        ROW_NUMBER () OVER (PARTITION BY Source ORDER BY date DESC) AS _rn
        FROM my_table
    ) AS virtual_table

Но если мы будем использовать такое в Суперсете, то график будет показывать 0 для всех других дат, кроме последней. Это происходит потому что при добавлении фильтра с датами  выбранный диапазон  указывается уже после расчёта ROW_NUMBER() и наше условие с _rn=1 перестаёт работать:

    SELECT SUM(factFollowers_fixed) FILTER(WHERE _rn=1) as _max_factFollowers 
    FROM (
        SELECT date, Source, factFollowers, factFollowers_fixed, 
        ROW_NUMBER () OVER (PARTITION BY Source ORDER BY date DESC) AS _rn
        FROM my_table
    ) AS virtual_table
    WHERE date >= toDate('2024-07-01') AND date < toDate('2024-07-04') 

    
Как же нам сделать так, чтобы фильтр даты был внутри виртуальной таблицы? То есть чтобы в итоге запрос выглядел, например, вот так:

    SELECT SUM(factFollowers_fixed) FILTER(WHERE _rn=1) as _max_factFollowers 
    FROM (
        SELECT date, Source, factFollowers, factFollowers_fixed, 
        ROW_NUMBER () OVER (PARTITION BY Source ORDER BY date DESC) AS _rn
        FROM my_table
        WHERE date >= toDate('2024-07-01') AND date < toDate('2024-07-04') 
    ) AS virtual_table

Ответ прост: можно использовать jinja-фильтры!

То есть нашу виртуальную таблицу мы записываем не так:

        SELECT date, Source, factFollowers, factFollowers_fixed, 
        ROW_NUMBER () OVER (PARTITION BY Source ORDER BY date DESC) AS _rn
        FROM my_table

а вот так:

        SELECT date, Source, factFollowers, factFollowers_fixed, 
        ROW_NUMBER () OVER (PARTITION BY Source ORDER BY date DESC) AS _rn
        FROM my_table
        WHERE date >= toDate('{{ from_dttm }}') and date < toDate('{{ to_dttm }}')

тогда при работе фильтра с датой поля from_dttm и to_dttm заполняются выбранными значениями (например, '2024-07-01' и '2024-07-04'), и в целом запрос начинает работать как нужно:

    SELECT SUM(factFollowers_fixed) FILTER(WHERE _rn=1) as _max_factFollowers 
    FROM (
        SELECT date, Source, factFollowers, factFollowers_fixed, 
        ROW_NUMBER () OVER (PARTITION BY Source ORDER BY date DESC) AS _rn
        FROM my_table
        WHERE date >= toDate('2024-07-01') AND date < toDate('2024-07-04') 
    ) AS virtual_table
