### Задание:

a) При обращении по доменному имени www.au.team, клиента должно перенаправлять на SRV2-DT на контейнер web

b) При обращении по доменному имени zabbix.au.team клиента должно перенаправлять на SRV3-DT на сервис Zabbix

c) Если необходимо, настройте сетевое оборудование для обеспечения работы требуемых сервисов.

### Вариант реализации:

### SRV1-DT:

- Установим пакет **nginx**:

```
apt-get update && apt-get install -y nginx
```

- Создадим конфигурационный файл, в котором опишем необходимые параметры для настройки обратного прокси-сервера:

```
vim /etc/nginx/sites-available.d/proxy.conf
```

- - Содержимое должно быть следующее:

![](https://sysahelper.ru/pluginfile.php/846/mod_page/content/3/image%20%283%29.png)

- Добавляем символьную ссылку на созданный файл:

```
ln -s /etc/nginx/sites-available.d/proxy.conf /etc/nginx/sites-enabled.d/
```

- Проверяем корректность написания конфигурационного файла для обратного прокси-сервера:

![](https://sysahelper.ru/pluginfile.php/846/mod_page/content/3/image%20%281%29.png)

- Включаем и добавляем в автозагрузку службу **nginx**:

```
systemctl enable --now nginx
```

- Проверяем:
    - Доступ по **[http://www.au.team](http://www.au.team/)**:

![](https://sysahelper.ru/pluginfile.php/846/mod_page/content/3/image%20%282%29.png)

- - Доступ по **[http://zabbix.au.team](http://zabbix.au.team/)**:

![](https://sysahelper.ru/pluginfile.php/846/mod_page/content/3/image%20%284%29.png)