## API - Airbyte

Есть задача перенести шаблоны подключений Airbyte из одного проекта в другой/другие.Сделать это нужно по API, т.к. шаблон можно создать один, и затем добавлять его на все новые проекты + по API можно передавать шаблон, не имея ещё доступов/токенов/аутентификации.

**Разбор элементов работающего запроса**:

![cover](https://github.com/Malakhova-Natalya/Snippets/blob/main/API/Airbyte/API%20Airbyte%20разбор%20запроса.png)

**Примеры кода (анонимная версия)**:

Метод sources/get:
    curl --request POST -u "airbyte:PASSWORD" \
    --header 'accept: application/json' --header 'content-type: application/json' \
    --url https://airbyte-SOMEWHERE.adventum.ru/api/v1/sources/get \
    --data '{"sourceId": "SOURCE_ID"}'

Метод source_definitions/list:

    curl --request POST -u "airbyte:PASSWORD" \
    --header 'accept: application/json' --header 'content-type: application/json' \
    --url https://airbyte-SOMEWHERE.adventum.ru/api/v1/source_definitions/list \
    --data '{"workspaceId": "WORKSPACE_ID", "includeTombstone": false}'

    
