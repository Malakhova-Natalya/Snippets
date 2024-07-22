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

Далее внутри models/for_docs_generate **нужно создать yaml-файл с описаниями exposures**. Его можно назвать как угодно. Поскольку exposures можно прописывать в одном yaml-файле вместе с sources и models, я назову свой файл как-то нейтрально, например, project_properties.

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


