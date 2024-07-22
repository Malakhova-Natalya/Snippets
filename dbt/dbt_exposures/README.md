# Exposures - что это такое и как их создать

## Exposures - что это такое

Exposures (экспозиции) - это описание в yaml-файле каких-либо внешних экспозиций, связанных с вашим dbt-проектом. Например, это дашборды, приложение или ML. Я планирую создать описание главных дашбордов рабочего проекта и описать в этой инструкции последовательность шагов для этого.

## Exposures - как их создать

Судя по [документации](https://docs.getdbt.com/reference/configs-and-properties#which-properties-are-not-also-configs), exposures относятся к таким properties (то есть свойствам dbt-проекта), которые невозможно использовать в config() блоках или в dbt_project.yml файле. 

Поэтому, чтобы использовать exposures, 

### шаг 1

нам **надо завести отдельную папку внутри папки models**. Судя по [документации exposures](https://docs.getdbt.com/reference/exposure-properties), такую папку можно заводить на произвольной "глубине" внутри папки models и называть как угодно. 

Я предпочту создать папку сразу внутри папки models, не зарываясь в глубины. Назову папку, например, for_docs_generate. 

### шаг 2

Далее внутри models/for_docs_generate **нужно создать yaml-файл с описаниями exposures**. Его можно назвать как угодно. Поскольку exposures можно прописывать в одном yaml-файле вместе с sources и models, я назову свой файл как-то нейтрально, например, project_properties.yml .

**Примечание:**

Кстати, отличия между расширениями «.yaml» и «.yml» отсутствуют. Они представляют собой два варианта одного и того же формата файла. Выбор между ними зависит от личных предпочтений и стандартов вашей команды или проекта.

Судя по той же документации, содержимое yaml-файла должно быть заполнено по такому шаблону:

    version: 2

    exposures:
    - name: <string_with_underscores>
      description: <markdown_string>
      type: {dashboard, notebook, analysis, ml, application}
      url: <string>
      maturity: {high, medium, low}  # Indicates level of confidence or stability in the exposure
      tags: [<string>]
      meta: {<dictionary>}
      owner:
        name: <string>
        email: <string>
    
      depends_on:
        - ref('model')
        - ref('seed')
        - source('name', 'table')
        - metric('metric_name')
      
      label: "Human-Friendly Name for this Exposure!"
      config:
        enabled: true | false

    - name: ... # declare properties of additional exposures

Также в документации приведён конкретный пример для учебного проекта jaffle_shop:

        version: 2

        exposures:

          - name: weekly_jaffle_metrics
            label: Jaffles by the Week              # optional
            type: dashboard                         # required
            maturity: high                          # optional
            url: https://bi.tool/dashboards/1       # optional
            description: >                          # optional
              Did someone say "exponential growth"?

            depends_on:                             # expected
              - ref('fct_orders')
              - ref('dim_customers')
              - source('gsheets', 'goals')
              - metric('count_orders')

            owner:
              name: Callum McData
              email: data@jaffleshop.com


      
          - name: jaffle_recommender
            maturity: medium
            type: ml
            url: https://jupyter.org/mycoolalg
            description: >
              Deep learning to power personalized "Discover Sandwiches Weekly"
    
            depends_on:
              - ref('fct_orders')
      
            owner:
              name: Data Science Drew
              email: data@jaffleshop.com

      
          - name: jaffle_wrapped
            type: application
            description: Tell users about their favorite jaffles of the year
            depends_on: [ ref('fct_orders') ]
            owner: { email: summer-intern@jaffleshop.com }

После того, как exposures прописаны (и без ошибок, например, нужно писать только существующие модели в ref() ) - можно генерировать документацию (надо быть в папке, внутри которой есть папка models):

        dbt docs generate

И отобразить её:

        dbt docs serve --port 8001
