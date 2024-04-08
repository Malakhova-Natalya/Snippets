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
	Например, вот так: [скрипт создания и наполнения таблицы](https://github.com/Malakhova-Natalya/Simple_scenarios/blob/main/03%20-%20script.txt)

Однако, если это рабочая задача, у БД могут быть ограничения прав. Поэтому может быть ошибка, например: 
SQL Error [497] [07000]: Code: 497. DB::Exception: <...>: Not enough privileges. To execute this query it's necessary to have grant CREATE TEMPORARY TABLE, URL ON *.*. (ACCESS_DENIED)
(Тут ошибка с тем, что у пользователя нет прав именно на загрузку из url)

### Сценарий 2: импорт данных через DBeaver
**Описание**: обойти ограничения прав помогает этот способ [ссылка](https://forum.goodt.me/t/zagruzka-csv-fajlov-v-postgres-s-pomoshhyu-dbeaver-shag-za-shagom/165)
**Шаги**:
  - 
![cover](https://github.com/Malakhova-Natalya/Simple_scenarios/blob/main/04%20-%20DBeaver%20импорт%20данных.png)
![cover](https://github.com/Malakhova-Natalya/Simple_scenarios/blob/main/05%20-%20DBeaver%20импорт%20данных%2001%20исходный%20формат.png)
![cover](https://github.com/Malakhova-Natalya/Simple_scenarios/blob/main/06%20-%20DBeaver%20импорт%20данных%2002%20входные%20файлы.png)
![cover](https://github.com/Malakhova-Natalya/Simple_scenarios/blob/main/07%20-%20DBeaver%20импорт%20данных%2003%20соответствие%20столбцов.png)
![cover](https://github.com/Malakhova-Natalya/Simple_scenarios/blob/main/08%20-%20DBeaver%20импорт%20данных%2004%20настройки%20загрузки%20данных.png)

