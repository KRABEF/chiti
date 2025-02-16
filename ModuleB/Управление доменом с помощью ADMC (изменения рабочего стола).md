### Задание:

a) Управление доменом с помощью ADMC осуществляться с ADMIN-HQ

b) Для подразделения CLI настройте политику изменения рабочего стола на картинку компании, а также запретите использование пользователям изменение сетевых настроек и изменение графических параметров рабочего стола.

### Вариант реализации:

### ADMIN-HQ | ADMIN-DT | CLI-HQ | CLI-DT:

- Необходимо установить пакет **gpupdate**:

```
apt-get update && apt-get install -y gpupdate
```

- Включить модуль групповых политик:

```
gpupdate-setup enable
```

### ADMIN-HQ:

- Установим пакет **admc**:

```
apt-get update && apt-get install -y admc
```

- Проверяем:
    - Получаем билет Kerberos (из под обычного пользователя):

```
kinit administrator@AU.TEAM
```

- - Запускаем оснастку ADMC:

![](https://sysahelper.ru/pluginfile.php/836/mod_page/content/2/image.png)

- Установим пакет **gpui**, для редактирования настроек клиентской конфигурации:

```
apt-get install -y gpui
```

- В оснастке ADMC - переходим в раздел **Объекты групповой политики** - выбираем подразделение **CLI** и нажимаем **Создать политику и связать с этом подразделением**:

![](https://sysahelper.ru/pluginfile.php/836/mod_page/content/2/image%20%281%29.png)

![](https://sysahelper.ru/pluginfile.php/836/mod_page/content/2/image%20%282%29.png)

- Выбираем созданную групповую политику и нажимаем **Изменить**:

![](https://sysahelper.ru/pluginfile.php/836/mod_page/content/2/image%20%283%29.png)

- - В соответствие с требованиями задания - реализуем необходимый функционал:
        - Задаём картинку компании для рабочего стола и запрещаем её менять

![](https://sysahelper.ru/pluginfile.php/836/mod_page/content/2/image%20%284%29.png)

- - - Запрещаем изменение сетевых настроек

![](https://sysahelper.ru/pluginfile.php/836/mod_page/content/2/image%20%285%29.png)

![](https://sysahelper.ru/pluginfile.php/836/mod_page/content/2/image%20%286%29.png)

![](https://sysahelper.ru/pluginfile.php/836/mod_page/content/2/image%20%288%29.png)

- - опционально пройтись по всему списку и выставить **Включено**, с вариантом ограничений **No**:

![](https://sysahelper.ru/pluginfile.php/836/mod_page/content/2/image%20%289%29.png)

- Проверяем:
    - перезагружаем **CLI-HQ** и **CLI-DT**:
        - картинка фона рабочего стола должна быть установлена:

![](https://sysahelper.ru/pluginfile.php/836/mod_page/content/2/image%20%2810%29.png)

- - При попытке отключить сетевой интерфейс:

![](https://sysahelper.ru/pluginfile.php/836/mod_page/content/2/image%20%2811%29.png)