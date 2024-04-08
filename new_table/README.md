## Добавить новую таблицу в базу данных
### Сценарий 1: url
**Описание задачи**: У нас есть файл в Excel, а в нём данные, которые нужно добавить в БД. Например, реестр городов и областей. Как нам добавить эти данные в БД?
Первый способ - через ссылку на csv файл. 

**Шаги**:
  - сохраняем нужные данные в формате csv
  - добавляем файл csv, например, на гитхаб
  - открываем этот файл - он должен выглядеть, например, так:

    ![cover](https://github.com/Malakhova-Natalya/Simple_scenarios/blob/main/new_table/01%20-%20csv.png) 
  - в верхнем правом углу переходим в режим сырых данных - нажимаем Raw
  - в результате получим что-то в таком духе:
    ![cover](https://github.com/Malakhova-Natalya/Simple_scenarios/blob/main/new_table/02%20-%20csv_raw.png)
  - отсюда надо скопировать ссылку из адресной строки наверху (всю целиком, вместе с .csv на конце, если оно есть)
  - далее переходим в DBeaver -> Редактор SQL -> Новый редактор SQL (для нужной БД)
  - пишем и последовательно выполняем скрипт из двух частей: создание таблицы, наполнение её данными
	Например, вот так: [скрипт создания и наполнения таблицы](https://github.com/Malakhova-Natalya/Simple_scenarios/blob/main/new_table/03%20-%20script.txt)

Однако, если это рабочая задача, у БД могут быть ограничения прав. Поэтому может быть ошибка, например: 
SQL Error [497] [07000]: Code: 497. DB::Exception: <...>: Not enough privileges. To execute this query it's necessary to have grant CREATE TEMPORARY TABLE, URL ON *.*. (ACCESS_DENIED)
(Тут ошибка с тем, что у пользователя нет прав именно на загрузку из url)

### Сценарий 2: импорт данных через DBeaver

**Описание**: обойти ограничения прав помогает этот [способ](https://forum.goodt.me/t/zagruzka-csv-fajlov-v-postgres-s-pomoshhyu-dbeaver-shag-za-shagom/165)

**Шаги**:
  - БД -> схема -> правой кнопкой мыши "Таблицы" -> "Импорт данных"

![cover](https://github.com/Malakhova-Natalya/Simple_scenarios/blob/main/new_table/04%20-%20DBeaver%20импорт%20данных.png)
  - исходный формат - csv

![cover](https://github.com/Malakhova-Natalya/Simple_scenarios/blob/main/new_table/05%20-%20DBeaver%20импорт%20данных%2001%20исходный%20формат.png)
  - выбрать входной файл

![cover](https://github.com/Malakhova-Natalya/Simple_scenarios/blob/main/new_table/06%20-%20DBeaver%20импорт%20данных%2002%20входные%20файлы.png)
  - соответствие столбцов - можно по умолчанию, можно изменить формат данных в столбцах в Configure
 
![cover](https://github.com/Malakhova-Natalya/Simple_scenarios/blob/main/new_table/07%20-%20DBeaver%20импорт%20данных%2003%20соответствие%20столбцов.png)
  - настройки загрузки данных - можно по умолчанию

![cover](https://github.com/Malakhova-Natalya/Simple_scenarios/blob/main/new_table/08%20-%20DBeaver%20импорт%20данных%2004%20настройки%20загрузки%20данных.png)

После этого данные будут загружены в БД.

### Сценарий 3: добавление данных напрямую через CREATE TABLE ... INSERT INTO ... VALUES

**Описание**: этот способ удобен для небольших тестовых данных

**Шаги**:


CREATE TABLE dt
(
    tm DateTime('Europe/Moscow'),
    
    dt_str String
)
ENGINE = Log;


Наполним ее данными:


INSERT INTO dt VALUES (1580518861, 'Wed, 01 Jan 2020 01:01:01 GMT'), 

                      ('2020-02-01 01:01:01', 'Sat, 01 Feb 2020 01:01:01 GMT');
