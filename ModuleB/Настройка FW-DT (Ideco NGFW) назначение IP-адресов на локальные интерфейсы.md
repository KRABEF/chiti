https://sysahelper.ru/mod/page/view.php?id=500
### Назначение IP-адресов на локальные интерфейсы

- С **ADMIN-DT** переходим в веб-интерфейс **FW-DT** для дальнейшей настройки локальных интерфейсов:
    - В модуле **Сервисы** переходим в раздел **Сетевые интерфейсы** и первым делом редактируем уже существующее подключение:

![](https://sysahelper.ru/pluginfile.php/855/mod_page/content/5/image%20%285%29.png)

- - Даём понятное имя для локального интерфейса, также помещаем интерфейс в зону безопасности, которую необходимо сначала создать:

![](https://sysahelper.ru/pluginfile.php/855/mod_page/content/5/image%20%286%29.png)

- - В результате получаем следующую конфигурацию для существующего локального интерфейса для подсети Администраторов
        - проверяем и нажимаем **Сохранить**:

![](https://sysahelper.ru/pluginfile.php/855/mod_page/content/5/image%20%287%29.png)

- - - Результат:

![](https://sysahelper.ru/pluginfile.php/855/mod_page/content/5/image%20%288%29.png)

- - Добавляем **Локальный Ethernet** для Серверной подсети (vlan220):

![](https://sysahelper.ru/pluginfile.php/855/mod_page/content/5/image%20%289%29.png)

- - Выбираем уже использующийцся физический интерфейс для реализации на его основе подинтерфейса:

![](https://sysahelper.ru/pluginfile.php/855/mod_page/content/5/image%20%2810%29.png)

- - Заполняем необходимые поля в соответствие с таблицей адресации и нажимаем **Добавить**:

![](https://sysahelper.ru/pluginfile.php/855/mod_page/content/5/image%20%2811%29.png)

- - - Результат:

![](https://sysahelper.ru/pluginfile.php/855/mod_page/content/5/image%20%2812%29.png)

- - Аналогичным образом добавляем подинтерфейс для Клиентской подсети (vlan110)
        - Результат:

![](https://sysahelper.ru/pluginfile.php/855/mod_page/content/5/image%20%2813%29.png)

- Также реализуем подключение к маршрутизатору **R-DT**, используя второе физическое подключение
    - Добавляем **Локальный Ethernet**:

![](https://sysahelper.ru/pluginfile.php/855/mod_page/content/5/image%20%2814%29.png)

- - Выбираем ранее не использованный физический интерфейс:

![](https://sysahelper.ru/pluginfile.php/855/mod_page/content/5/image%20%2815%29.png)

- - Заполняем все необходимые поля, за исключением IP-адреса шлюза по умолчанию
        - по условиям задания: _FW-DT должен получать маршрут по умолчанию и другие необходимые маршруты от R-DT через OSPF_

![](https://sysahelper.ru/pluginfile.php/855/mod_page/content/5/image%20%2816%29.png)

- - - Результат:

![](https://sysahelper.ru/pluginfile.php/855/mod_page/content/5/image%20%2817%29.png)