# Metric Flow

Материалы, чтобы разобраться:
- About MetricFlow на docs.getdbt.com: [здесь](https://docs.getdbt.com/docs/build/about-metricflow)
- GitHub - dbt-labs/metricflow: [здесь](https://github.com/dbt-labs/metricflow)
- нашла [ветку для ClickHouse](https://github.com/kolatr-dev/metricflow/tree/feature/support-clickhouse) в [этом обсуждении](https://discourse.getdbt.com/t/metricflow-with-clickhouse-adapter/12857)

## шаг 1. pip install dbt-metricflow

Пытаюсь установить:

    pip install dbt-metricflow
  
получаю ошибку 

    [Errno 13] Permission denied: '/home/natalia/.venv/lib/python3.10/site-packages/text_unidecode'
    Check the permissions.

Использую ранее разобранный [здесь](https://github.com/Malakhova-Natalya/Snippets/blob/main/dbt/start_dbt_project_from_github/README.md) подход:

     sudo chmod <три цифры уровня доступа> -R /home/natalia/<название папки>

В конце концов metricflow устанавливается успешно. Но команда

    mf tutorial

не работает, выдаёт длинную ошибку, суть которой в следующем:

    Could not find adapter type clickhouse!

Действительно, для Clickhouse пока адаптера нет, попробуем ради metricflow действовать через Postgres.

## шаг 2. Postgres

**1.** Поднимаем Postgres в Docker через Docker Desktop по аналогии с разбором [здесь](https://github.com/Malakhova-Natalya/Snippets/blob/main/other/docker_desktop/README.md)

Порт автоматически заполняется как 5432 + задаём имя контейнеру + обязательно в переменных указываем

        POSTGRES_PASSWORD

с непустым значением (иначе будет ошибка и контейнер не будет создан).

Далее можно сделать новое соединение с БД в DBeaver → База данных → Новое подключение - PostreSQL: localhost, внимание - **пользователь - postgres**, пароль -  непустое значение, заданное шагом ранее.

**2.** Что ещё понадобится для работы с postgres? Точно нужно установить:

        pip install dbt-postgres

Возможно, ещё понадобится:

        python -m pip install "dbt-metricflow[postgres]"

**3.** Нужно прописать в .dbt/profiles.yml профиль для подключения к БД postgres. Например, у меня это:

        dbt_local_postgres:
          target: dev
          outputs:
            dev:
              type: postgres
              host: localhost
              user: postgres
              password: 'ваше непустое значение пароля как string'
              port: 5432
              dbname: postgres 
              schema: public
              threads: 4

И этот профиль (в моём случае dbt_local_postgres) нужно заполнить в файле dbt_project.yml - 10-я строка:

        profile: 'dbt_local_postgres'
