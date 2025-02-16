### Задание:

b) Все устройства должны синхронизировать своё время с SRV1-HQ.

- 1. Используйте chrony, где это возможно

c) Используйте на всех устройствах московский часовой пояс.

### Вариант реализации:

### SW1-HQ | SW2-HQ | SW3-HQ | SRV1-DT | SRV2-DT | SRV3-DT:

- Редактируем конфигурационный файл  **chrony**:

```
vim /etc/chrony.conf
```

- - Приводим его к следующему виду:

![](https://sysahelper.ru/pluginfile.php/831/mod_page/content/6/image.png)

- Перезагружаем службу **chronyd** для применения изменений:

```
systemctl restart chronyd
```

- Проверяем наличие клиентов на **SRV1-HQ**:

![](https://sysahelper.ru/pluginfile.php/831/mod_page/content/6/image%20%281%29.png)

### ADMIN-HQ | ADMIN-DT | CLI-HQ | CLI-DT:

![](https://sysahelper.ru/pluginfile.php/831/mod_page/content/6/image%20%282%29.png)

- Проверяем наличие клиентов на **SRV1-HQ**:

![](https://sysahelper.ru/pluginfile.php/831/mod_page/content/6/image%20%283%29.png)

### R-HQ | R-DT:

```
r-hq#configure terminal
r-hq(config)#ntp server 192.168.11.66
r-hq(config)#ntp timezone UTC+3
r-hq(config)#write
r-hq(config)#
```

### FW-DT:

![](https://sysahelper.ru/pluginfile.php/831/mod_page/content/6/image%20%284%29.png)

![](https://sysahelper.ru/pluginfile.php/831/mod_page/content/6/image%20%285%29.png)

- Проверяем наличие клиентов на **SRV1-HQ**:

![](https://sysahelper.ru/pluginfile.php/831/mod_page/content/6/image%20%286%29.png)