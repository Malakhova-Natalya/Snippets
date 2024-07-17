## Запуски parse, compile, run с --vars или что делать, если проект поломался

Предположим, в вашем dbt-проекте есть какие-либо изменения, использование параметров при запуске или что-то ещё, что в какой-то момент даёт вам ошибку 

      Unable to do partial parsing because config vars, config profile, or config target have changed

В моей рабочей жизни причиной такого стало использование запусков dbt run с добавкой -- vars.
