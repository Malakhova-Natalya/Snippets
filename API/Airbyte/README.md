## API - Airbyte

Есть задача перенести шаблоны подключений Airbyte из одного проекта в другой/другие.Сделать это нужно по API, т.к. шаблон можно создать один, и затем добавлять его на все новые проекты + по API можно передавать шаблон, не имея ещё доступов/токенов/аутентификации.

**Разбор элементов работающего запроса**:

![cover](https://github.com/Malakhova-Natalya/Snippets/blob/main/API/Airbyte/API%20Airbyte%20разбор%20запроса.png)

Документация Airbyte по API: https://airbyte-public-api-docs.s3.us-east-2.amazonaws.com/rapidoc-api-docs.html

**Начало работы**:

Например, можно работать в Visual Studio Code. Открываем Terminal --> New Terminal --> Ubuntu (wsl)

Для более плотной работы разработчики советуют использовать postman/insomnia - кратко это записано [здесь](https://github.com/Malakhova-Natalya/Snippets/tree/main/API).


**Примеры кода (анонимная версия)**:

Метод sources/get:

    curl --request POST -u "airbyte:PASSWORD" \
    --header 'accept: application/json' --header 'content-type: application/json' \
    --url https://airbyte-SOMEWHERE.adventum.ru/api/v1/sources/get \
    --data '{"sourceId": "SOURCE_ID"}'

Кстати запросы можно писать в блокноте и затем вставлять их в командную строку. В таком случае иногда могут мешаться ненужные пробелы, поэтому можно вставлять запросы без разбиения \ , например вот так:

        curl --request POST -u "airbyte:PASSWORD" --header 'accept: application/json' --header 'content-type: application/json' --url https://airbyte-SOMEWHERE.adventum.ru/api/v1/sources/get --data '{"sourceId": "SOURCE_ID"}'

Метод source_definitions/list:

    curl --request POST -u "airbyte:PASSWORD" \
    --header 'accept: application/json' --header 'content-type: application/json' \
    --url https://airbyte-SOMEWHERE.adventum.ru/api/v1/source_definitions/list \
    --data '{"workspaceId": "WORKSPACE_ID", "includeTombstone": false}'


Метод sources/create:

    curl --request POST -u "airbyte:PASSWORD" \
    --header 'accept: application/json' --header 'content-type: application/json' \
    --url https://airbyte-SOMEWHERE.adventum.ru/api/v1/sources/create \
    <SOMETHING},"client_login":"SOMETHING","adimages_use_simple_loader":true},"name":"NEW_NAME"}'
