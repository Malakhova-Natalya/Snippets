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
