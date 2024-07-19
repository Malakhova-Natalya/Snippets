## Как создать новый dbt-проект

Документация dbt: 
- [Quickstart for dbt Core from a manual install](https://docs.getdbt.com/guides/manual-install?step=1)
- [About dbt Core and installation](https://docs.getdbt.com/docs/core/installation-overview)

В моём случае у меня уже есть активные dbt-проекты по работе. Я использую Visual Studio Code и запускаю команды в CLI. Всё, что мне нужно - это теперь не использовать готовый dbt-проект, а с нуля инициировать новый + установить в него (через dbt_packages) etlcraft (это проект, который мы с разрабатываем на работе).

Поскольку ничего нового устанавливать не надо, обращусь к первой указанной здесь документации - Quickstart for dbt Core.

Следуя инструкции, создаю репозиторий для своего нового проекта. Например, назову его dbt-tutorial, и сделаю этот репозиторий открытым (публичным), выглядит это, например, вот так:

![cover](https://github.com/Malakhova-Natalya/Snippets/blob/main/dbt/dbt_init_new_project/01_create_new_repository.png)

всё, что здесь нужно - заполнить несколько разделов (имя репозитория/доступ public или private/нужен ли README файл и есть ещё пара настроек по желанию, их можно оставлять по умолчанию) и в конце нажать create repository.

После этого можно посмотреть на этот пустой новый репозиторий:

![cover](https://github.com/Malakhova-Natalya/Snippets/blob/main/dbt/dbt_init_new_project/02_empty_new_repository.png)

и теперь скачать его себе на компьютер через Code (зелёненькая кнопка).

Частично это повторение шагов из раздела [Скачать и запустить dbt-проект с гитхаба](https://github.com/Malakhova-Natalya/Snippets/blob/main/dbt/start_dbt_project_from_github/README.md)

Что именно я делаю: Code → SSH → копирую ссылку. У меня это: 

    git@github.com:Malakhova-Natalya/dbt-tutorial.git

Дальнейшие действия:

- захожу в Visual Studio Code -> Terminal Ubuntu-22.04 (WSL)
- перехожу сразу в своего пользователя
  
        sudo su natalia
  
и перехожу "домой" (то есть в стартовую директорию для пользователя)

          cd ~
          
- убеждаюсь, что я нахожусь по адресу, куда буду скачивать репозиторий. Например, у меня это /home/natalia/ (эта самая стартовая директория для пользователя)
  
      pwd

убеждаюсь, что команда pwd выдаёт результат /home/natalia

- выполняю команду
- 
        git clone git@github.com:Malakhova-Natalya/dbt-tutorial.git

  
