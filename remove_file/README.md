## Удалить файл, который не удаляется
### Сценарий 1: удаление через командную строку
**Описание задачи**: Допустим, у вас есть папка с файлами, в которой никак не удаляется один файл. При ручном удалении выскакивает ошибка, которую кажется невозможно обойти. Это «Ошибка 0x80070003: Системе не удается найти указанный путь.»

![cover](https://github.com/Malakhova-Natalya/Simple_scenarios/blob/main/remove_file/02.png)


**Шаги**:
  - запоминаем адрес ненужной папки и её название
   
    ![cover](https://github.com/Malakhova-Natalya/Simple_scenarios/blob/main/remove_file/01.png)
  - удаляем через командную строку:
   
    ![cover](https://github.com/Malakhova-Natalya/Simple_scenarios/blob/main/remove_file/03.png)
    - открываем, например,  Visual Studio Code – > Terminal Ubuntu-22.04 (WSL)
      (потому что в данном случае файл лежит на Ubuntu)
    - переходим по адресу, где лежит папка, которую будем удалять
   
      
         например, нужно удалить /home/natalia/old_dbt-etlcraft-main_old?
      
         значит переходим в /home/natalia
    - выполняем команду rm -rf <имя папки>
      
         rm от слова remove, значит удалить
      
         -rf значит рекурсивно и то что внутри есть ещё папки
    - готово, вы великолепны! И командная строка тоже)
     
