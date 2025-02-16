### Задание:

a) Управление доменом с помощью ADMC осуществляться с ADMIN-HQ

c) Для подразделения ADMIN реализуйте подключение общей папки SAMBA с использованием доменных политик.

### Вариант реализации:

### ADMIN-HQ:

- В оснастке ADMC - переходим в раздел **Объекты групповой политики** - выбираем подразделение **ADMIN** и нажимаем **Создать политику и связать с этом подразделением**:

![](https://sysahelper.ru/pluginfile.php/837/mod_page/content/4/image.png)

- Выбираем созданную групповую политику и нажимаем **Изменить**:

![](https://sysahelper.ru/pluginfile.php/837/mod_page/content/4/image%20%281%29.png)

- - В соответствие с требованиями задания - реализуем необходимый функционал:

![](https://sysahelper.ru/pluginfile.php/837/mod_page/content/4/image%20%285%29.png)

- - - Результат:

![](https://sysahelper.ru/pluginfile.php/837/mod_page/content/4/image%20%286%29.png)

- Проверяем:
    - перезагружаем **ADMIN-DT** и **ADMIN-HQ** - должен подключиться автоматически сетевой диск:

![](https://sysahelper.ru/pluginfile.php/837/mod_page/content/4/image%20%287%29.png)

![](https://sysahelper.ru/pluginfile.php/837/mod_page/content/4/image%20%288%29.png)