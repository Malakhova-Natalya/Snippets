# Как создать новый dbt-проект

## Шаг 1. git clone + dbt init
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

Частично, кстати, это повторение шагов из раздела [Скачать и запустить dbt-проект с гитхаба](https://github.com/Malakhova-Natalya/Snippets/blob/main/dbt/start_dbt_project_from_github/README.md)

Но на всякий случай законспектирую ещё раз здесь, что именно я делаю: Code → SSH → копирую ссылку. У меня это: 

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
 
        git clone git@github.com:Malakhova-Natalya/dbt-tutorial.git

Все эти команды выглядят, например, вот так:

![cover](https://github.com/Malakhova-Natalya/Snippets/blob/main/dbt/dbt_init_new_project/03_terminal.png)

После выполнения команды git clone на компьютере появляется папка репозитория dbt-tutorial. В этом можно убедиться, перейдя по адресу, куда проводилось скачивание. Пока папка практически пустая - в ней только привязка к гитхабу - скрытая папка .git и файл с описанием репозитория README.md.

![cover](https://github.com/Malakhova-Natalya/Snippets/blob/main/dbt/dbt_init_new_project/04_folder_on_computer.png)

Далее открываю эту папку - dbt-tutorial - в Visual Studio Code и подготавливаю Terminal к работе:

![cover](https://github.com/Malakhova-Natalya/Snippets/blob/main/dbt/dbt_init_new_project/05_vscode_terminal.png)

Проверяю версию dbt:

    dbt --version

Инициирую новый dbt-проект, используя команду init:

    dbt init my_dbt_project

Тут dbt может спросить, какую базу данных планируется использовать, предлагает доступные варианты и просит ввести номер варианта:

    Which database would you like to use?
    [1] clickhouse

    (Don't see the one you want? https://docs.getdbt.com/docs/available-adapters)

    Enter a number:

(выбор у меня небольшой, ввожу 1). Dbt создаёт проект и предлагает несколько ссылок, где можно посмотреть документацию.

После завершения установки в Visual Studio Code появляется новое содержимое:

![cover](https://github.com/Malakhova-Natalya/Snippets/blob/main/dbt/dbt_init_new_project/06_dbt_init.png)

Итак, новый dbt-проект инициирован, осталось добавить ему packages (меня интересует etlcraft).

## Шаг 2. dbt_project.yml ↔ .dbt/profiles.yml

В проекте в файле dbt_project.yml есть профиль: строка 

    profile: 'my_dbt_project'
    
У меня она автоматически заполнена как 'my_dbt_project' из-за названия проекта при инициализации. 

    # Name your project! Project names should contain only lowercase characters
    # and underscores. A good package name should reflect your organization's
    # name or the intended use of these models
    name: 'my_dbt_project'
    version: '1.0.0'
    config-version: 2
    
    # This setting configures which "profile" dbt uses for this project.
    profile: 'my_dbt_project'
        
Но вообще-то значение профиля надо привести в соответствие с файлом, расположенным в папке .dbt (например у меня она находится по адресу "\\wsl.localhost\Ubuntu-22.04\home\natalia\.dbt").
Если в соответствие профиль не привести, то при попытке запуска будет ошибка 

    ERROR: Runtime Error
      Could not find profile named 'my_dbt_project'

Итак, в папке .dbt есть файл profiles.yml. У меня один профиль (значение записано в первой строке), и я для этого проекта тоже буду использовать его - dbt_local. Так что в dbt_project.yml  в строке profile меняю значение:

        profile: 'dbt_local'

Теперь, когда в dbt_project.yml и в .dbt/profile одно значение профиля, тестовые модели отрабатывают успешно.

![cover](https://github.com/Malakhova-Natalya/Snippets/blob/main/dbt/dbt_init_new_project/07_profile.png)

## Шаг 3. packages

Теперь установим packages. Для этого надо на том же уровне, где находится dbt_project.yml, создать ещё один файл - packages.yml. В нём записываются packages, которые вы хотите использовать. Выглядит это, например, вот так:
    
![cover](https://github.com/Malakhova-Natalya/Snippets/blob/main/dbt/dbt_init_new_project/08_packages.png)

Список доступных packages можно посмотреть на [Package hub](https://hub.getdbt.com/).

Таким образом можно скачать известные packages. Например, можно установить такие:
    
    packages:
      - package: dbt-labs/dbt_utils
        version: 1.1.1
      - package: yu-iskw/dbt_unittest
        version: 0.3.3    

А что, если вы хотите использовать свою разработку? Можно действовать через git:

    packages:
      - git: "https://github.com/adventum/dbt-etlcraft.git" 
        revision: v1.0 

Можно использовать revision, чтобы скачать определённую версию проекта. Если не указывать - скачается текущее состояние проекта:

    packages:
      - git: "https://github.com/adventum/dbt-etlcraft.git" 

После того, как заполнили файл packages.yml,  убеждаемся, что находимся по адресу своего проекта (в моём примере это dbt-tutorial/my_dbt_project - папка, сразу внутри которой лежит файл packages.yml) и вызываем команду

    dbt deps

Указанные packages скачаются в папку dbt_packages (надо немного подождать, может быть, 1-2 минуты, и содержимое появится).

![cover](https://github.com/Malakhova-Natalya/Snippets/blob/main/dbt/dbt_init_new_project/09_etlcraft.png)

Хотя я указывала только etlcraft, у меня появились ещё dbt_utils и dbt_unittest, потому что они вызываются внутри etlcraft и указаны в его packages-файле.


Если вы что-то скачали, и хотите это изменить, нужно сначала всё удалить:

    dbt clean

Подождать ,проверить, что всё удалилось (1-2 минуты), заполнить packages как вы хотите, и потом снова его запустить командой

    dbt deps

    
