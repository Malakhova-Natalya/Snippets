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

