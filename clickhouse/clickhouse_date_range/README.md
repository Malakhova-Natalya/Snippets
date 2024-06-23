## ClickHouse: сгенерировать строки с датами, имея дату начала и дату конца периода

**Варианты решения**: [здесь](https://github.com/Malakhova-Natalya/Snippets/blob/main/clickhouse/clickhouse_date_range/01%20-%20генерация%20строк%20с%20датами.txt)

**Дополнительно**: кроме решения именно для ClickHouse, **можно обратиться к макросу dbt**

1. создаём модель, например, dim_calendar.sql
2. прописываем в ней config, например:

   		{{ config(
			materialized='table',
			order_by='date_day'
			) 
   		}}

4. вызывыем макрос, указывая дату начала и дату конца календаря по своему усмотрению:

	 	{{ dbt_date.get_date_dimension('2020-01-01', '2025-01-01') }}

В результате будет создана таблица со множеством колонок. В каждой колонке - измерения, связанные с календарем, например, сама дата, день вперед, день назад, название месяца и тд и тп



## ClickHouse: сравнить запросы

Например, у нас есть два решения одной задачи и мы хотим сравнить запросы. Один запрос использует LAST_VALUE и FILTER:

	SELECT socSource, socDate, followers, LAST_VALUE(followers) FILTER (WHERE followers !=0)
			      OVER (PARTITION BY socSource
			      ORDER BY socSource, socDate
			      ROWS BETWEEN UNBOUNDED PRECEDING AND CURRENT ROW) AS followers_fixed
	FROM my_table

Другой - LAST_VALUE и CASE WHEN внутри него:

	SELECT socSource, socDate, followers, LAST_VALUE(CASE WHEN followers!=0 THEN followers 
				ELSE COALESCE(replaceAll(followers, 0, NULL)) END) 
		   OVER(PARTITION BY socSource 
				ORDER BY socSource, socDate 
				ROWS BETWEEN UNBOUNDED PRECEDING AND CURRENT ROW) AS followers_fixed
	FROM my_table

Для сравнения запросов можно их сначала записать в DBeaver, затем запустить по очереди, подготовить технический запрос к system.query_log. И далее исполнить например такой запрос:


	SELECT query_start_time_microseconds,  query , 
	query_duration_ms, read_rows, read_bytes, 
	result_rows, result_bytes,  memory_usage,
	formatReadableSize(read_bytes) AS read_bytes
	FROM system.query_log 
	WHERE type='QueryFinish' AND query like 'SELECT socSource, socDate, followers, LAST_VALUE(followers) FILTER%' OR
					query like 'SELECT socSource, socDate, followers, LAST_VALUE(CASE WHEN%'
	ORDER BY query_start_time_microseconds DESC
	LIMIT 2

 Здесь в SELECT запрошены интересующие данные, а в WHERE отобраны интересующие запросы согласно началу кода запроса. А в ORDER BY и LIMIT мы дополнительно себе помогаем и выводим информацию именно о двух последних запросах, чтобы не отбирать всё подряд и не путаться.
