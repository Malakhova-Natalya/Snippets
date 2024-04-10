## Скачать и запустить dbt-проект с гитхаба
### Последовательность действий

**Шаги**:
- заходим на гитхаб проекта - Code -> SSH -> копируем ссылку на репозиторий
    
![cover](https://github.com/Malakhova-Natalya/Simple_scenarios/blob/main/start_dbt_project_from_github/01%20git%20clone.png)

- заходим в Visual Studio Code -> Terminal Ubuntu-22.04 (WSL)
- переходим по адресу, куда будем скачивать репозиторий. Например, у меня это /home/natalia/
- выполняем команду git clone <скопированная ссылка на репозиторий>
- далее при попытке запуска проекта (любой команды dbt) мы можем увидеть ошибку доступа: Permission denied
 
  ![cover](https://github.com/Malakhova-Natalya/Simple_scenarios/blob/main/start_dbt_project_from_github/02%20permission%20denied.png)
- задаём доступ рекурсивно с помощью команды:

 
         sudo chmod <три цифры уровня доступа> -R /home/natalia/<название скачанной папки с dbt-проектом>

  
 ![cover](https://github.com/Malakhova-Natalya/Simple_scenarios/blob/main/start_dbt_project_from_github/03%20sudo%20chmod%20755%20-R.png)
  
Подробнее про варианты уровней доступа можно прочитать [здесь](https://www.maketecheasier.com/file-permissions-what-does-chmod-777-means/)
