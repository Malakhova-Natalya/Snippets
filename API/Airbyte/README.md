## API - Airbyte

Есть задача перенести шаблоны подключений Airbyte из одного проекта в другой/другие.Сделать это нужно по API, т.к. шаблон можно создать один, и затем добавлять его на все новые проекты + по API можно передавать шаблон, не имея ещё доступов/токенов/аутентификации.

**Разбор элементов работающего запроса**:

![cover](https://github.com/Malakhova-Natalya/Snippets/blob/main/API/Airbyte/API%20Airbyte%20разбор%20запроса.png)

Документация Airbyte по API: https://airbyte-public-api-docs.s3.us-east-2.amazonaws.com/rapidoc-api-docs.html

**Начало работы**:

Например, можно работать в Visual Studio Code. Открываем Terminal --> New Terminal --> Ubuntu (wsl)

Для более плотной работы разработчики советуют использовать postman/insomnia - кратко что это описано [здесь](https://github.com/Malakhova-Natalya/Snippets/tree/main/API).


**Примеры кода (анонимная версия)**:

**Метод sources/get**:

    curl --request POST -u "airbyte:PASSWORD" \
    --header 'accept: application/json' --header 'content-type: application/json' \
    --url https://airbyte-SOMEWHERE.adventum.ru/api/v1/sources/get \
    --data '{"sourceId": "SOURCE_ID"}'

Кстати запросы можно писать в блокноте и затем вставлять их в командную строку. В таком случае иногда могут мешаться ненужные пробелы. Поэтому можно вставлять запросы без разбиения \ , в одну строку - например запрос выше будет выглядеть вот так:

    curl --request POST -u "airbyte:PASSWORD" --header 'accept: application/json' --header 'content-type: application/json' --url https://airbyte-SOMEWHERE.adventum.ru/api/v1/sources/get --data '{"sourceId": "SOURCE_ID"}'

выдаёт например такой результат:

    {"sourceDefinitionId":"SOURCE_DEFINITION_ID","sourceId":"SOURCE_ID","workspaceId":"WORKSPACE_ID","connectionConfiguration":{"reports":[{"name":"custom_report","fields":["Date","CampaignId","CampaignName","CampaignType","AdId","Cost","Impressions","Clicks"],"goal_ids":[],"filters_json":"[]","additional_fields":[],"attribution_models":[]}],"date_range":{"load_today":true,"date_range_type":"last_days","last_days_count":5},"credentials":{"auth_type":"credentials_craft_auth","credentials_craft_host":"https://credentialscraft-SOMEWHERE.adventum.ru","credentials_craft_token":"**********","credentials_craft_token_id":TOKEN_NUMBER},"client_login":"adventum-CLIENT","adimages_use_simple_loader":true},"name":"yd_default_","sourceName":"yd"}

Этот результат даёт нам:
- sourceDefinitionId
- sourceId
- workspaceId
- connectionConfiguration
- name - название connection 
- sourceName - название connector

С помощью такого запроса мы можем получить информацию о каком-либо connection, который, например, мы хотим перенести с одного проекта на другой. Эта информация пригодится при создании connection на другом проекте - там при помощи запроса с методом create мы передадим полученную здесь информацию (возьмём sourceDefinitionId, connectionConfiguration) ???

**Метод source_definitions/list**:

    curl --request POST -u "airbyte:PASSWORD" \
    --header 'accept: application/json' --header 'content-type: application/json' \
    --url https://airbyte-SOMEWHERE.adventum.ru/api/v1/source_definitions/list \
    --data '{"workspaceId": "WORKSPACE_ID", "includeTombstone": false}'

выдаёт оооочень много данных, пример результата здесь приводить не буду. Для нашей задачи можно обойтись и без этого метода.


**Метод sources/create**:

    curl --request POST -u "airbyte:PASSWORD" \
    --header 'accept: application/json' --header 'content-type: application/json' \
    --url https://airbyte-SOMEWHERE.adventum.ru/api/v1/sources/create \
    <SOMETHING},"name":"NEW_NAME"}'

этот метод создаёт новый шаблон с именем NEW_NAME
