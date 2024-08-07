## Как скрыть некоторые папки dbt-проекта с гитхаба?

Очень просто. Для этого нам нужен файл .gitignore. Этот файл можно создать, если его ещё нет, в корневой папке вашего проекта. (Создать можно, например, в Visual Studio Code).

Стандартное наполнение .gitignore для dbt-проекта это:

    # dbt
    dbt_packages/
    logs/
    target/

Причём такой файл можно создать не только в корневой папке, а и в той, где вы запускаете модели. Например, я запускаю в папке integration_tests, значит я могу поместить ещё один файл .gitignore и туда. Заполнить можно также - названиями папок, которые находятся по адресу размещения файла .gitignore, и которые вы хотите скрыть.

После того как вы добавили какие-то файлы/папки в .gitignore они не исчезнут сами из гитхаба (если они до этого уже были там). Надо удалить их, и потом добавить всё снова. Перед этим убедитесь, что всё нужное вы добавили на гитхаб.

Итак, удалить всё:

    git rm -rf --cached .

Добавить заново всё:

    git add .

Эти команды удалят все файлы из репозитория и добавят их обратно, но уже соблюдая правила, прописанные в вашем .gitignore.

### Детали

в файле .gitignore система ищет значения по регулярным выражениям, а некоторые названия могут пересекаться/совпадать. Чтобы исключить нежелательное поведение, можно написать, например, так:

    # dbt
    dbt_packages/
    logs/
    target/
    !target_for_demonstration/

Восклицательный знак означает, что эту папку надо исключить. Например, target будет игнорироваться гитхабом, а target_for_demonstration не будет игнорироваться - то есть будет показан на гитхабе.
