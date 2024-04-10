## Скачать и запустить dbt-проект с гитхаба
### Последовательность действий

**Шаги**:
- заходим на гитхаб проекта - Code -> SSH -> копируем ссылку на репозиторий
    
![cover](https://github.com/Malakhova-Natalya/Simple_scenarios/blob/main/start_dbt_project_from_github/01%20git%20clone.png)

- заходим, например, в Visual Studio Code -> Terminal Ubuntu-22.04 (WSL)
- переходим по адресу, куда будем скачивать репозиторий. Например, у меня это /home/natalia/
- выполняем команду git clone <скопированная ссылка на репозиторий>
- далее при попытке запуска проекта (любой команды dbt, например dbt deps) мы можем увидеть ошибку доступа: Permission denied
 
![cover](https://github.com/Malakhova-Natalya/Simple_scenarios/blob/main/start_dbt_project_from_github/02%20permission%20denied.png)
- задаём доступ рекурсивно с помощью команды:

 
         sudo chmod <три цифры уровня доступа> -R /home/natalia/<название скачанной папки с dbt-проектом>

  
![cover](https://github.com/Malakhova-Natalya/Simple_scenarios/blob/main/start_dbt_project_from_github/03%20sudo%20chmod%20755%20-R.png)

Три цифры потому что задаём доступы для: Owner, Group, Others

Read is equivalent to “4.”
Write is equivalent to “2.”
Execute is equivalent to “1.”

Комбинации этих прав дают следующие цифры от 0 до 7:

0: No permission
1: Execute
2: Write
3: Write and Execute
4: Read
5: Read and Execute
6: Read and Write
7: Read, Write, and Execute
  
Подробнее про варианты уровней доступа можно прочитать [здесь](https://www.maketecheasier.com/file-permissions-what-does-chmod-777-means/)

- если далее при попытке запустить проект встречаем ошибки, например, такие: (что в итоге даёт could not run dbt)
  
![cover](https://github.com/Malakhova-Natalya/Simple_scenarios/blob/main/start_dbt_project_from_github/04%20errors.png)

- то вызываем команду
              dbt debug

![cover](https://github.com/Malakhova-Natalya/Simple_scenarios/blob/main/start_dbt_project_from_github/05%20dbt%20debug.png)

  с помощью этой команды можно понять, где что-то не сходится. 
  
  В данном случае не сходятся профили в файлах profiles.yml и dbt_project.yml, и мы можем увидеть расположение этих файлов

- в данном примере с помощью команды cat мы посмотрим содержание файла profiles.yml и отредактируем значение профиля в dbt_project.yml, чтобы всё сходилось

![cover](https://github.com/Malakhova-Natalya/Simple_scenarios/blob/main/start_dbt_project_from_github/06%20cat%20dbt_local.png)

- если в dbt-проекте всё стало верно, то команда debug, проверяющая отсутствие ошибок,
  
          dbt debug
  отработает успешно

![cover](https://github.com/Malakhova-Natalya/Simple_scenarios/blob/main/start_dbt_project_from_github/07%20dbt%20debug.png)

- далее нужно установить зависимости при помощи команды
  
          dbt deps

![cover](https://github.com/Malakhova-Natalya/Simple_scenarios/blob/main/start_dbt_project_from_github/08%20dbt%20deps.png)

если с проектом всё в порядке, она тоже отработает успешно

- далее можно запускать проект, например так

          dbt run
но это слишком экстремально, лучше запускать отдельную часть проекта, а не весь целиком :-)

![cover](https://github.com/Malakhova-Natalya/Simple_scenarios/blob/main/start_dbt_project_from_github/09%20dbt%20run.png)
