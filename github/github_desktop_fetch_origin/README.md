## GitHub Desktop завис на Fetch origin - что можно сделать?

Один простой лайфхак.

Открываем GitHub Desktop → переходим в раздел Repository → Repository settings

![cover](https://github.com/Malakhova-Natalya/Snippets/blob/main/github/github_desktop_fetch_origin/01_Repository_settings.png)

в разделе Remote в имеющейся строке вместо git вставляем https ссылку на ваш репозиторий

![cover](https://github.com/Malakhova-Natalya/Snippets/blob/main/github/github_desktop_fetch_origin/02_Repository_settings_remote.png)

Откуда эти значения?

значение с git@github - это один способ связывания репозитория - по SSH 

![cover](https://github.com/Malakhova-Natalya/Snippets/blob/main/github/github_desktop_fetch_origin/03_Code_SSH.png)

а значение с https - это другой способ - по HTTPS (и оно работает, видимо, получше)

![cover](https://github.com/Malakhova-Natalya/Snippets/blob/main/github/github_desktop_fetch_origin/04_Code_HTTPS.png)
