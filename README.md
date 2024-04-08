# Simple_scenarios
Простые сценарии: учебные/рабочие задачи, ситуации и пошаговые подходы к их решению

## Добавить новую таблицу в базу данных
### Сценарий 1: url
**Описание задачи**: У нас есть файл в Excel, а в нём данные, которые нужно добавить в БД. Например, реестр городов и областей. Как нам добавить эти данные в БД?
Первый способ - через ссылку на csv файл. 

**Шаги**:
  - сохраняем нужные данные в формате csv
  - добавляем файл csv, например, на гитхаб
  - открываем этот файл - он должен выглядеть, например, так:

    ![cover](https://github.com/Malakhova-Natalya/Simple_scenarios/blob/main/01%20-%20csv.png)
  - в верхнем правом углу переходим в режим сырых данных - нажимаем Raw
  - в результате получим что-то в таком духе:
    ![cover](https://github.com/Malakhova-Natalya/Simple_scenarios/blob/main/02%20-%20csv_raw.png)
  - отсюда надо скопировать ссылку из адресной строки наверху (всю целиком, вместе с .csv на конце, если оно есть)
  - далее переходим в DBeaver -> Редактор SQL -> Новый редактор SQL (для нужной БД)
  - пишем и последовательно выполняем скрипт из двух частей: создание таблицы, наполнение её данными
Например, вот так:

    CREATE TABLE default.`regions_and_cities` 
(
	`city_rus` String, 
	`region_rus` LowCardinality(String), 
	`city_eng` String, 
	`region_eng` LowCardinality(String),
	`region_ISO_code` LowCardinality(String)
)
ENGINE = MergeTree()
ORDER BY city_rus;


INSERT INTO default.regions_and_cities
SELECT * FROM url(
'https://raw.githubusercontent.com/Malakhova-Natalya/My_hints/main/regions_and_cities.csv', 
CSVWithNames, 
'city_rus String, 
region_rus LowCardinality(String), 
city_eng String, 
region_eng LowCardinality(String),
region_ISO_code LowCardinality(String)'
);
