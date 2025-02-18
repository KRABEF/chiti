### Задание:

a) Реализуйте основной DNS сервер компании на SRV1-HQ

- 1. Для всех устройств обоих офисов необходимо создать записи A и PTR.
- 2. Для всех сервисов предприятия необходимо создать записи CNAME.
- 3. Загрузка записей с SRV1-HQ должна быть разрешена только для SRV1-DT

d) В качестве DNS сервера пересылки используйте любой общедоступный DNS сервер

### Вариант реализации:

Записи типа A, PTR и CNAME - будут создаваться после установки контроллера домена, средствами samba-tool.

Данный процесс рассмотрет в [Добавление всех необходимые записей типа A, PTR и CNAME средствами samba-tool](https://sysahelper.ru/mod/page/view.php?id=502)

### SRV1-HQ:

- Для установки необходимых пакетов, временно установим в качестве DNS публичный адрес:

```
echo "nameserver 77.88.8.8" > /etc/resolv.conf
```

- Установим необходимые пакеты:

```
apt-get update && apt-get install -y bind bind-utils
```

- Правим конфигурационный файл **/etc/bind/options.conf**:

```
vim /etc/bind/options.conf
```

- - вносим следующие изменения:
        - **listen-on** - Позволяет указать сетевые интерфейсы, которые будет прослушивать служба;
        - **listen-on-v6** - Раз IPv6 не используется, тогда не используем;
        - **forwarders** - DNS-сервер, на который будут перенаправляться запросы клиентов;
        - **allow-query** - IP-адреса и подсети от которых будут обрабатываться запросы;
        - **allow-transfer** - Устанавливает возможность передачи зон для slave-серверов.

![](https://sysahelper.ru/pluginfile.php/827/mod_page/content/4/image.png)

- Включаем и добавляем в автозагрузку службу **bind**:

```
systemctl enable --now bind
```

- Проверяем:
    - Настраиваем SRV1-HQ на использование в качестве DNS-сервера самого себя:

```
cat <<EOF > /etc/net/ifaces/ens19/resolv.conf
  search au.team
  nameserver 192.168.11.66
EOF
```

- - Перезагружаем службы **network** и **bind**:

```
systemctl restart network
```

```
systemctl restart bind
```

- - Проверяем доступ в сеть Интернет:

![](https://sysahelper.ru/pluginfile.php/827/mod_page/content/4/image%20%281%29.png)