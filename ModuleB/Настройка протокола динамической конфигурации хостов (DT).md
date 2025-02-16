### Задание:

a) На R-DT настройте протокол динамической конфигурации хостов для клиентов (CLI-DT)

- 1. Адрес сети – согласно топологии
    - i. Исключите адрес шлюза по умолчанию из диапазона выдаваемых адресов
- 2. Адрес шлюза по умолчанию – в соответствии с топологией
    - i. Шлюзом для сети DT является межсетевой экран FW-DT
- 3. DNS-суффикс – au.team
- 4. Настройте клиентов на получение динамических адресов.

### Вариант реализации:

### R-DT:

Аналогично R-HQ, подробный процесс настройки DHCP - сервера рассмотрет в [Настройка протокола динамической конфигурации хостов (HQ)](https://sysahelper.ru/mod/page/view.php?id=472)

- Таким образом настройка DHCP - сервера выглядит следующим образом:

```
r-dt#configure terminal
r-dt(config)#ip pool CLI-DT 192.168.33.2-192.168.33.62
r-dt(config)#dhcp-server 1
r-dt(config-dhcp-server)#pool CLI-DT 1
r-dt(config-dhcp-server-pool)#mask 26
r-dt(config-dhcp-server-pool)#gateway 192.168.33.1
r-dt(config-dhcp-server-pool)#dns 192.168.33.66,192.168.11.66
r-dt(config-dhcp-server-pool)#domain-name au.team
r-dt(config-dhcp-server-pool)#exit
r-dt(config-dhcp-server)#exit
r-dt(config)#interface int1
r-dt(config-if)#dhcp-server 1
r-dt(config-if)#exit
r-dt(config)#write
r-dt(config)#
```

### FW-DT:

- Настраиваем **DHCP Relay**:
    - В веб-интерфейсе **FW-DT** с **ADMIN-DT** переходим в модуле **Сервисы** в раздел **DHCP-сервер** и нажимаем **Добавить**:

![](https://sysahelper.ru/pluginfile.php/822/mod_page/content/3/image.png)

- - Выбираем интерфейс, который будет участвовать в раздаче IP-адресов, затем введим IP-адрес DHCP-сервера:

![](https://sysahelper.ru/pluginfile.php/822/mod_page/content/3/image%20%281%29.png)

- - Активируем службу для работы DHCP-Relay:

![](https://sysahelper.ru/pluginfile.php/822/mod_page/content/3/image%20%282%29.png)

- - Результат:

![](https://sysahelper.ru/pluginfile.php/822/mod_page/content/3/image%20%283%29.png)

### CLI-DT:

- Настраиваем клиента на получение динамических адресов:

![](https://sysahelper.ru/pluginfile.php/822/mod_page/content/3/image%20%284%29.png)

- Проверяем:

![](https://sysahelper.ru/pluginfile.php/822/mod_page/content/3/image%20%285%29.png)

P.S. на текущий момент доступа в сеть Интернет из офиса DT быть не должно:

1. Не реализована авторизация на FW-DT;
2. FW-DT не имеет IP-адреса шлюза по умолчанию, т.к. не было настройки OSPF ещё.