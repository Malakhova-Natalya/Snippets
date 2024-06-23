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

