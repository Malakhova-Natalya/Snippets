## Запуски parse, compile, run с --vars или что делать, если проект поломался

Предположим, в вашем dbt-проекте есть какие-либо изменения, использование параметров при запуске или что-то ещё, что в какой-то момент даёт вам ошибку 

      Unable to do partial parsing because config vars, config profile, or config target have changed

В моей рабочей жизни причиной такого стало использование запусков dbt run с добавкой -- vars. Причём в команде может быть несколько подробностей: с какого места запустить модели, запустить с последующими моделями, обновить ли все модели, какие переменные подставить, какую модель исключить...

      dbt run -m models/1_silos/1_normalize/+ --full-refresh --vars '{"limit0": true,  "metadata_name": "metadata_1" }' --exclude "normalize_appsflyer_events_default_in_app_events"
