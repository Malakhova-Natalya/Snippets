## Exposures - что это такое и как их создать

Судя по [документации](https://docs.getdbt.com/reference/configs-and-properties#which-properties-are-not-also-configs), exposures относятся к таким properties (то есть свойствам dbt-проекта), которые невозможно использовать в config() блоках или в dbt_project.yml файле. 

**Поэтому, чтобы использовать exposures, нам надо завести для них отдельную папку внутри папки models**. Судя по [документации exposures](https://docs.getdbt.com/reference/exposure-properties), такую папку можно заводить на произвольной "глубине" внутри папки models. 

Я предпочту создать папку сразу внутри папки models, не зарываясь в глубины. Назову папку, например, for_docs_generate.
