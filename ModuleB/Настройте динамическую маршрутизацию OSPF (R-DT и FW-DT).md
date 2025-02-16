### Задание:

b) Между R-DT и FW-DT

- 1. R-DT должен узнавать о сетях, подключенных к FW-DT по OSPF.
- 2. FW-DT должен получать маршрут по умолчанию и другие необходимые маршруты от R-DT через OSPF.
- 3. R-DT должен быть защищен от вброса маршрутов с любых интерфейсов, кроме тех, на которых обмен маршрутами явно требуется.

### Вариант реализации:

### R-DT:

- FW-DT должен получать маршрут по умолчанию и другие необходимые маршруты от R-DT через OSPF:

```
r-dt#configure terminal
r-dt(config)#router ospf 1
r-dt(config-router)#default-information originate 
r-dt(config-router)#exit
r-dt(config)#  write
r-dt(config)#
```

### FW-DT:

- В веб-интерфейсе **FW-DT** с **ADMIN-DT** переходим в модуль **Сервисы** в раздел **OSPF** на вкладку **ДОПОЛНИТЕЛЬНО** и снимаем чек-боксы с функционала, который не требуется по заданию:

![](https://sysahelper.ru/pluginfile.php/826/mod_page/content/3/image.png)

- Для настройки OSPF переходим на вкладку **ОСНОВНЫЕ** и нажимаем **Добавить**:

![](https://sysahelper.ru/pluginfile.php/826/mod_page/content/3/image%20%281%29.png)

- Настраиваем OSPF на локальном интерфейсе:

![](https://sysahelper.ru/pluginfile.php/826/mod_page/content/3/image%20%282%29.png)

- Аналогично и для всех остальных интерфейсов, в результате должны получить следующее:

![](https://sysahelper.ru/pluginfile.php/826/mod_page/content/3/image%20%283%29.png)

- Включаем OSPF:

![](https://sysahelper.ru/pluginfile.php/826/mod_page/content/3/image%20%284%29.png)

- Проверяем:

![](https://sysahelper.ru/pluginfile.php/826/mod_page/content/3/image%20%285%29.png)

P.S. но если приглядеться на маршрут по умолчанию, который получен по OSPF, то можно увидеть **inactive**

- Также результат попытки выхода в сеть Интернет получается следующий:

![](https://sysahelper.ru/pluginfile.php/826/mod_page/content/3/image%20%287%29.png)

- Полная таблица маршрутизации на FW-DT выглядит следующим образом:

![](https://sysahelper.ru/pluginfile.php/826/mod_page/content/3/image%20%288%29.png)

- А в результате вывода команды **show runnin-config**:
    - присутствует информация, которая не была целенаправлена задана через веб-интерфейс FW-DT

![](https://sysahelper.ru/pluginfile.php/826/mod_page/content/3/image%20%289%29.png)

- Тогда переходим в конфигурационный файл **/etc/frr/frr.conf** (например используя текстовый редактор **vi**) и удаляем данный блок
- После чего необходимо перезагрузить службу **frr:**

```
systemctl restart frr
```

- - Проверяем:

![](https://sysahelper.ru/pluginfile.php/826/mod_page/content/3/image%20%2811%29.png)