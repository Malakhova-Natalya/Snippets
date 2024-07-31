# Metric Flow

Материалы, чтобы разобраться:
- About MetricFlow на docs.getdbt.com: [здесь](https://docs.getdbt.com/docs/build/about-metricflow)
- GitHub - dbt-labs/metricflow: [здесь](https://github.com/dbt-labs/metricflow)
- нашла [ветку для ClickHouse](https://github.com/kolatr-dev/metricflow/tree/feature/support-clickhouse) в [этом обсуждении](https://discourse.getdbt.com/t/metricflow-with-clickhouse-adapter/12857)

пытаюсь установить

  pip install dbt-metricflow
  
получаю ошибку 

  [Errno 13] Permission denied: '/home/natalia/.venv/lib/python3.10/site-packages/text_unidecode'
  Check the permissions.

     sudo chmod 777 -R /home/natalia/.venv/lib/python3.10/site-packages/text_unidecode

    sudo chmod 777 -R /home/natalia/.venv/bin
