## Как изменить schema в dbt-проекте
### Где указано значение schema

Данная информация указана в файле profiles.yml в папке .dbt

То есть, например, для пользователя natalia полный путь может выглядеть так:
\\wsl.localhost\Ubuntu-22.04\home\natalia\\.dbt\profiles.yml

Содержимое этого файла может выглядеть примерно вот так:


    dbt_local:
        outputs:
            dev:
                connect_timeout: 30
                host: localhost
                port: 8123
                schema: test
                threads: 2
                type: clickhouse
                user: default
        target: dev

В самом верхнеуровнем файле dbt_project.yml должно быть указано то же значение профиля, например

        profile: 'dbt_local'

То есть содержимое файла profiles.yml более обобщённо можно обозначить как:

    YOUR_PROFILE_NAME:
        outputs:
            dev:
                connect_timeout: YOUR_NUMBER
                host: YOUR_HOST
                port: YOUR_PORT
                schema: YOUR_SCHEMA_NAME
                threads: YOUR_NUMBER
                type: YOUR_TYPE
                user: YOUR_NUMBER
        target: dev

### Как изменить базовое значение schema

В файле profiles.yml, который мы рассмотрели выше, нужно задать нужное Вам значение в 

                schema: YOUR_SCHEMA_NAME

Остальное dbt сделает сам.

Это значение будет использовано во всём проекте, и от него избавиться будет невозможно. К нему можно только добавлять дополнительные части. Как - об этом ниже.

### Как сделать составное название schema

Рабочая ситуация.

Никто, абсолютно никто: <живёт себе спокойно>


Airbyte: а изменим-ка мы схему, куда будут идти сырые данные!

... Так получилось, что для одного проекта мне надо было каким-то образом создать отдельную схему под названием airbyte_internal, положить туда все сырые данные проекта (это тестовые данные) через dbt seed, и затем сделать так, чтобы dbt находил данные в той схеме airbyte_internal и работал с ними тем же образом, как было раньше (в своей схеме, для примера это у меня схема test).

В случае реального проекта данные из новой версии Airbyte сами будут приходить в новую схему. Создать её достаточно просто (имея нужные права): 

        CREATE DATABASE airbyte_internal
  
#### Последовательность действий, чтобы сделать составное название schema
Рассмотрим для примера, что нам нужно для seeds сделать схему airbyte_internal.

1. home/natalia/.dbt/profiles.yml - прописать
   
                   schema: airbyte

2. integration_tests/dbt_project.yml - прописать

        seeds:
           schema: internal

Оно сгенерируется как надо - в airbyte_internal, потому что по умолчанию макрос [generate_schema_name](https://docs.getdbt.com/docs/build/custom-schemas#how-does-dbt-generate-a-models-schema-name) работает по такой логике:

    {% macro generate_schema_name(custom_schema_name, node) -%}

        {%- set default_schema = target.schema -%}
        {%- if custom_schema_name is none -%}

            {{ default_schema }}

        {%- else -%}

            {{ default_schema }}_{{ custom_schema_name | trim }}

        {%- endif -%}

    {%- endmacro %}

Этот макрос можно положить в папку macros (сразу в неё, без подпапок), и изменить его поведение. Однако избавиться от {{ default_schema }} в начале в любом случае не удастся, в документации есть специальный [Warning](https://docs.getdbt.com/docs/build/custom-schemas#warning-dont-replace-default_schema-in-the-macro)

последствия экспериментов можно удалить в DBeaver вот так: DROP DATABASE test_airbyte_internal
