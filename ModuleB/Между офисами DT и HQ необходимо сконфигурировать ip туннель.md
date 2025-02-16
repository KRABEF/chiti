### Задание:

1) Между офисами DT и HQ необходимо сконфигурировать ip туннель

- a) Используйте GRE

### Вариант реализации:

### R-DT:

- Настраиваем **GRE** туннель в сторону **R-HQ**,
    - где: i**nterface tunnel.<номер>** - номер это произвольное число

```
r-dt#configure terminal
r-dt(config)#  interface tunnel.0
r-dt(config-if-tunnel)#description "Connect_HQ-R"
r-dt(config-if-tunnel)#ip add 10.10.10.1/30
r-dt(config-if-tunnel)#ip mtu 1476
r-dt(config-if-tunnel)#ip tunnel 172.16.4.14 172.16.5.14 mode gre
r-dt(config-if-tunnel)#end
r-dt#write
r-dt#
```

- Проверяем:

![](https://sysahelper.ru/pluginfile.php/824/mod_page/content/2/image.png)

### R-HQ:

- Настраиваем **GRE** - туннель в сторону **R-DT**,
    - где: **interface tunnel.<номер>** - номер это произвольное число

```
r-hq#configure terminal
r-hq(config)#  interface tunnel.0
r-hq(config-if-tunnel)#description "Connect_DT-R"
r-hq(config-if-tunnel)#ip add 10.10.10.2/30
r-hq(config-if-tunnel)#ip mtu 1476
r-hq(config-if-tunnel)#ip tunnel 172.16.5.14 172.16.4.14 mode gre
r-hq(config-if-tunnel)#end
r-hq#write
r-hq#
```

- Проверяем:

![](https://sysahelper.ru/pluginfile.php/824/mod_page/content/2/image%20%281%29.png)