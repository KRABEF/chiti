https://sysahelper.ru/mod/page/view.php?id=467

### Задание:

a) Для подключения R-DT к провайдеру необходимо использовать последний адрес из сети 172.16.4.0/28.

c) Провайдер использует первый адрес из каждой сети

### Вариант реализации:

### R-DT:

- Выполняем создание **интерфейса** с именем **isp** и назначаем IPv4-адрес согласно таблице адресации в сторону **ISP**:

```
r-dt#configure terminal
r-dt(config)#interface isp
r-dt(config-if)#ip address 172.16.4.14/28
r-dt(config-if)#exit
r-dt(config)#
```

- Переходим в режим конфигурирования порта **te0** - создаём **service-instance** с именем **te0/isp:**
    - также указываем что будет обрабатывать не тегированный трафик (**untagged**);
    - и указываем в какой интерфейс нужно отправить обработанные кадры (**isp**);

```
r-dt(config)#port te0
r-dt(config-port)#service-instance te0/isp
r-dt(config-service-instance)#encapsulation untagged
r-dt(config-service-instance)#connect ip interface isp
r-dt(config-service-instance)#exit
r-dt(config-port)#exit
r-dt(config)#
```

- Задаём IP-адрес шлюза по умолчанию:

```
r-dt(config)#ip route 0.0.0.0/0 172.16.4.1
r-dt(config)#write
r-dt(config)#
```

- - Проверяем:

![](https://sysahelper.ru/pluginfile.php/818/mod_page/content/2/image.png)