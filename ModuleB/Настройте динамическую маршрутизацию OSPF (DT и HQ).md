### Задание:

a) Между офисами DT и HQ

- 1. Маршрутизаторы должны быть защищены от вброса маршрутов с любых интерфейсов, кроме тех, на которых обмен маршрутами явно требуется.
- 2. Обеспечьте защиту протокола маршрутизации посредством парольной защиты
    - i. Используйте пароль P@ssw0rd

### Вариант реализации:

### R-HQ:

- Настраиваем OSPFv2:
    - Задаём router-id;
    - Переводим все интерфейсы в пассивный режим;
    - Объявляем сети;
    - На туннельном интерфейсе отключаем пассивный режим, чтобы можно было установить соседство:

```
r-hq#configure terminal
r-hq(config)#router ospf 1
r-hq(config-router)#ospf router-id 10.10.10.2
r-hq(config-router)#passive-interface default
r-hq(config-router)#network 10.10.10.0  0.0.0.3 area 0
r-hq(config-router)#network 192.168.11.0 0.0.0.63 area 0
r-hq(config-router)#network 192.168.11.64 0.0.0.15 area 0
r-hq(config-router)#network 192.168.11.80 0.0.0.7 area 0
r-hq(config-router)#no passive-interface tunnel.0
r-hq(config-router)#exit
r-hq(config)#exit
r-hq#write
r-hq#
```

- Обеспечиваем защиту протокола маршрутизации посредством парольной защиты:

```
r-hq#configure terminal
r-hq(config)#interface tunnel.0
r-hq(config-if-tunnel)#ip ospf authentication message-digest
r-hq(config-if-tunnel)#ip ospf message-digest-key 1 md5 P@ssw0rd
r-hq(config-if-tunnel)#exit
r-hq(config)#write
r-hq(config)#
```

### R-DT:

- Настраиваем OSPFv2:
    - Задаём router-id
    - Переводим все интерфейсы в пассивный режим, т.к. сказано в задании
    - Объявляем сети
    - На туннельном и в сторону FW-DT интерфейсах отключаем пассивный режим, чтобы можно было установить соседство

```
r-dt#configure terminal
r-dt(config)#router ospf 1
r-dt(config-router)#ospf router-id 10.10.10.1
r-dt(config-router)#passive-interface default
r-dt(config-router)#network 10.10.10.0  0.0.0.3 area 0
r-dt(config-router)#network 192.168.33.88 0.0.0.3 area 0
r-dt(config-router)#no passive-interface tunnel.0
r-dt(config-router)#no passive-interface int1
r-dt(config-router)#exit
r-dt(config)#exit
r-dt#write
r-dt#
```

- Обеспечиваем защиту протокола маршрутизации посредством парольной защиты:

```
r-dt#configure terminal
r-dt(config)#interface tunnel.0
r-dt(config-if-tunnel)#ip ospf authentication message-digest
r-dt(config-if-tunnel)#ip ospf message-digest-key 1 md5 P@ssw0rd
r-dt(config-if-tunnel)#exit
r-dt(config)#write
r-dt(config)#
```

- Проверяем:

- - Утсановленное соседство и таблицы маршрутизации:
        - **R-HQ:**

**![](https://sysahelper.ru/pluginfile.php/825/mod_page/content/3/image.png)**

- - - **R-DT:**

![](https://sysahelper.ru/pluginfile.php/825/mod_page/content/3/image%20%281%29.png)