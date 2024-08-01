# Всё о yml-файлах

## seeds

**Внутри папки seeds** создаём подпапку или сразу пишем файл. Назвать файл можно любым именем, формат - yml. Например, 

    integration_tests/seeds/project_seeds.yml

Документация: [здесь](https://docs.getdbt.com/reference/seed-properties)

Пример из документации:

    version: 2
    
    seeds:
      - name: <string>
        [description](/reference/resource-properties/description): <markdown_string>
        [docs](/reference/resource-configs/docs):
          show: true | false
          node_color: <color_id> # Use name (such as node_color: purple) or hex code with quotes (such as node_color: "#cd7f32")
        [config](/reference/resource-properties/config):
          [<seed_config>](/reference/seed-configs): <config_value>
        [tests](/reference/resource-properties/data-tests):
          - <test>
          - ... # declare additional tests
        columns:
          - name: <column name>
            [description](/reference/resource-properties/description): <markdown_string>
            [meta](/reference/resource-configs/meta): {<dictionary>}
            [quote](/reference/resource-properties/quote): true | false
            [tags](/reference/resource-configs/tags): [<string>]
            [tests](/reference/resource-properties/data-tests):
              - <test>
              - ... # declare additional tests
    
          - name: ... # declare properties of additional columns
    
      - name: ... # declare properties of additional seeds
      -   
## macros

**Внутри папки macros** создаём подпапку или сразу пишем файл. Назвать файл можно любым именем, формат - yml. Например, 

    integration_tests/macros/project_macros.yml
    
Документация: [здесь](https://docs.getdbt.com/reference/macro-properties)

Пример из документации:

    version: 2
    
    macros:
      - name: <macro name>
        [description](/reference/resource-properties/description): <markdown_string>
        [docs](/reference/resource-configs/docs):
          show: true | false
        arguments:
          - name: <arg name>
            [type](/reference/resource-properties/argument-type): <string>
            [description](/reference/resource-properties/description): <markdown_string>
          - ... # declare properties of additional arguments
    
      - name: ... # declare properties of additional macros




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
