# Всё о yml-файлах

## seeds

Внутри папки seeds создаём подпапку или сразу пишем файл. Назвать файл можно любым именем, формат - yml.
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

## sources

## models

## exposures

exposures отдельно разбирала [здесь](https://github.com/Malakhova-Natalya/Snippets/tree/main/dbt/dbt_exposures)