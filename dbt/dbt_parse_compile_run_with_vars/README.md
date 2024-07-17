## Запуски parse, compile, run с --vars или что делать, если проект поломался

Предположим, в вашем dbt-проекте есть какие-либо изменения, использование параметров при запуске или что-то ещё, что в какой-то момент даёт вам ошибку 

      Unable to do partial parsing because config vars, config profile, or config target have changed

В моей рабочей жизни причиной такого стало использование запусков dbt run с добавкой -- vars. Причём в команде может быть несколько подробностей: с какого места запустить модели, запустить с последующими моделями, обновить ли все модели, какие переменные подставить, какую модель исключить...

      dbt run -m models/1_silos/1_normalize/+ --full-refresh --vars '{"limit0": true,  "metadata_name": "metadata_1" }' --exclude "normalize_appsflyer_events_default_in_app_events"

Такое изобилие - это, конечно, хорошо. Но иногда может настать момент ошибки Unable to do partial parsing. Её коварство в том, что после себя она выдает конкретную модель и её проблему. Но чинить там что-то бесполезно - потому что дело никогда не заключается в этой модели. Ошибка - в парсинге. Как же это починить?

Ответ прост: нам помогут последовательные запуски 
- **parse**
- **compile**
- **run**  -

  
**и все с одинаковыми vars** (и при этом желательно run с full-refresh)

Например, в итоге нас интересует команда:

      dbt run -m models/1_silos/1_normalize/+ --full-refresh --vars '{"limit0": true,  "metadata_name": "metadata_1" }'

Но она падает с ошибкой Unable to do partial parsing и выдаёт нам - я так это называю - ложный след. Как только мы видим Unable to do partial parsing - мы обращаемся к parse и compile.

Запускаем  команду parse с интересующими нас в дальнейшем --vars:

      dbt parse --vars '{"limit0": true, "metadata_name": "metadata_1" }'

После неё запускаем compile - и опять вместе с интересующими vars:

      dbt compile --vars '{"limit0": true, "metadata_name": "metadata_1" }'

И после этого уже команду run (с теми же самыми vars):

      dbt run -m models/1_silos/1_normalize/+ --full-refresh --vars '{"limit0": true,  "metadata_name": "metadata_1" }'

**Детали:**

насколько я поняла, для команды parse нет особых подробностей, она запускается на весь проект,  к ней нельзя добавить никаких особых деталей, например, --exclude.

А вот для команд compile и run сработает, например, и такое:

      dbt compile  --vars '{"limit0": true, "metadata_name": "metadata_1" }' --exclude "normalize_appsflyer_events_default_in_app_events"


