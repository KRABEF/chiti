https://sysahelper.ru/mod/page/view.php?id=468
### Задание:

b) Для подключения R-HQ к провайдеру необходимо должен использовать последний адрес из сети 172.16.5.0/28.

c) Провайдер использует первый адрес из каждой сети

### Вариант реализации:

### R-HQ:

- Выполняем создание **интерфейса** с именем **isp** и назначаем IPv4-адрес согласно таблице адресации в сторону **ISP**:

```
r-hq#configure terminal
r-hq(config)#interface isp
r-hq(config-if)#ip address 172.16.5.14/28
r-hq(config-if)#exit
r-hq(config)#
```

- Переходим в режим конфигурирования порта **te0** - создаём **service-instance** с именем **te0/isp:**
    - также указываем что будет обрабатывать не тегированный трафик (**untagged**);
    - и указываем в какой интерфейс нужно отправить обработанные кадры (**isp**);

```
r-hq(config)#port te0
r-hq(config-port)#service-instance te0/isp
r-hq(config-service-instance)#encapsulation untagged
r-hq(config-service-instance)#connect ip interface isp
r-hq(config-service-instance)#exit
r-hq(config-port)#exit
r-hq(config)#
```

- Задаём IP-адрес шлюза по умолчанию:

```
r-hq(config)#ip route 0.0.0.0/0 172.16.5.1
r-hq(config)#write
r-hq(config)#
```

- - Проверяем:

![](https://sysahelper.ru/pluginfile.php/819/mod_page/content/4/image.png)