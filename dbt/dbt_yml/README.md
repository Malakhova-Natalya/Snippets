# Всё о yml-файлах

В yml-файлах описываются источники, макросы, модели, - всё то, что можно увидеть в документации. Все описания происходят в yml-файлах. Разберём их все от сырых данных до внешних экспозиций.

В документации можно задавать цвета кодом.

Палитры и их код можно смотреть [здесь](https://www.color-hex.com/color-palettes/)

Например, возьму такие цвета:  "Коричневые тона" [отсюда](https://colorscheme.ru/html-colors.html)


## seeds

**Внутри папки seeds** создаём подпапку или сразу пишем файл. Назвать файл можно любым именем, формат - yml. Например, 

    integration_tests/seeds/project_seeds.yml

Документация: [здесь](https://docs.getdbt.com/reference/seed-properties)

Работающий пример:

    version: 2
    
    seeds:
      - name: datacraft_clientname_raw__stream_adjust_default_accountid_cohorts
        description: adjust_default_accountid_cohorts
        docs:
          show: true 
          node_color: "#873a39" 
        columns:
          - name: _airbyte_raw_id
            description: техническое поле Airbyte - id строки
            tags: ['adjust']
          - name: _airbyte_data 
            description: поле с данными, все колонки внутри одной json-строки
            tags: ['adjust']
          - name: _airbyte_extracted_at 
            description: техническое поле Airbyte - время получения данных
            tags: ['adjust']
    
    #  - name: ... # declare properties of additional seeds


## macros

**Внутри папки macros** создаём подпапку или сразу пишем файл. Назвать файл можно любым именем, формат - yml. Например, 

    integration_tests/macros/project_macros.yml
    
Документация: [здесь](https://docs.getdbt.com/reference/macro-properties)

На примере макроса:

    {% macro cents_to_dollars(column_name, scale=2) %}
        ({{ column_name }} / 100)::numeric(16, {{ scale }})
    {% endmacro %}

Будет такой yml_файл:

    version: 2
    
    macros:
      - name: cents_to_dollars
        arguments:
          - name: column_name
            type: column name or expression
            description: "The name of a column, or an expression — anything that can be `select`-ed as a column"
    
          - name: scale
            type: integer
            description: "The number of decimal places to round to. Default is 2."


## sources, models, exposures

**Внутри папки models** создаём подпапку или сразу пишем файл. Назвать файл можно любым именем, формат - yml. Например, 

    integration_tests/models/project_properties.yml

Документация: [здесь](https://docs.getdbt.com/reference/configs-and-properties#which-properties-are-not-also-configs)

Пример из документации:

    version: 2
    
    sources:
      - name: raw_jaffle_shop
        description: A replica of the postgres database used to power the jaffle_shop app.
        tables:
          - name: customers
            columns:
              - name: id
                description: Primary key of the table
                tests:
                  - unique
                  - not_null
    
          - name: orders
            columns:
              - name: id
                description: Primary key of the table
                tests:
                  - unique
                  - not_null
    
              - name: user_id
                description: Foreign key to customers
    
              - name: status
                tests:
                  - accepted_values:
                      values: ['placed', 'shipped', 'completed', 'return_pending', 'returned']
    
    
    models:
      - name: stg_jaffle_shop__customers
        config:
          tags: ['pii']
        columns:
          - name: customer_id
            tests:
              - unique
              - not_null
    
      - name: stg_jaffle_shop__orders
        config:
          materialized: view
        columns:
          - name: order_id
            tests:
              - unique
              - not_null
          - name: status
            tests:
              - accepted_values:
                  values: ['placed', 'shipped', 'completed', 'return_pending', 'returned']
                  config:
                    severity: warn
    



**P.S.** exposures отдельно разбирала [здесь](https://github.com/Malakhova-Natalya/Snippets/tree/main/dbt/dbt_exposures)
