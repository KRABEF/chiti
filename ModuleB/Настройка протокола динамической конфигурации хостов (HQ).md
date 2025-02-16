### Задание:

a) На R-HQ настройте протокол динамической конфигурации хостов для клиентов (CLI-HQ)

- 1. Адрес сети – согласно топологии
    - i. Исключите адрес шлюза по умолчанию из диапазона выдаваемых адресов
- 2. Адрес шлюза по умолчанию – в соответствии с топологией
    - i. Шлюзом для сети HQ является маршрутизатор R-HQ
- 3. DNS-суффикс – au.team
- 4. Настройте клиентов на получение динамических адресов.

### Вариант реализации:

### R-HQ:

- Задаём POOL адресов с именем **CLI-HQ**, затем задаём диапазон IP-адресов, который будет раздаваться DHCP сервером:
    - в данном случае раздаваться будет вся клиентская подсеть заисключением IP-адреса маршрутизатора R-HQ

```
r-hq#configure terminal
r-hq(config)#ip pool CLI-HQ 192.168.11.2-192.168.11.62
r-hq(config)#
```

- Для настройки DHCP-сервера необходимо в режиме конфигурации ввести команду **dhcp-server <NUMBER>**
    - где **NUMBER** – номер сервера в системе маршрутизатора:

```
r-hq(config)#dhcp-server 1
r-hq(config-dhcp-server)#
```

- Привязываем ранее созданный POOL раздаваемых адресов с именем **CLI-HQ**, а также указанием номера сервера в системе маршрутизатора **1**:

```
r-hq(config-dhcp-server)#pool CLI-HQ 1
r-hq(config-dhcp-server-pool)#
```

- Задаём основные параметры для раздачи DHCP сервером:

```
r-hq(config-dhcp-server-pool)#mask 26
r-hq(config-dhcp-server-pool)#gateway 192.168.11.1
r-hq(config-dhcp-server-pool)#dns 192.168.11.66,192.168.33.66
r-hq(config-dhcp-server-pool)#domain-name au.team
r-hq(config-dhcp-server-pool)#exit
r-hq(config-dhcp-server)#exit
r-hq(config)#
```

- После настройки сервера необходимо указать, на каком интерфейсе маршрутизатор будет принимать пакеты DHCP Discover и отвечать на них предложением с IP-настройками:
    - в данном случае подинтерфейс с именем **vl110** смотрит в сторону клиентской подсети (vlan110)

```
r-hq(config)#interface vl110
r-hq(config-if)#dhcp-server 1
r-hq(config-if)#exit
r-hq(config)#write
r-hq(config)#
```

### CLI-HQ:

- Настраиваем клиента на получение динамических адресов:

![](https://sysahelper.ru/pluginfile.php/823/mod_page/content/3/image.png)

- Проверяем:

![](https://sysahelper.ru/pluginfile.php/823/mod_page/content/3/image%20%281%29.png)