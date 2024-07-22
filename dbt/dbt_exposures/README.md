## Exposures - что это такое и как их создать

Судя по [документации](https://docs.getdbt.com/reference/configs-and-properties#which-properties-are-not-also-configs), exposures относятся к таким properties (то есть свойствам dbt-проекта), которые невозможно использовать в config() блоках или в dbt_project.yml файле. 

Поэтому, чтобы использовать exposures, нам надо завести для них отдельную папку внутри папки models.
