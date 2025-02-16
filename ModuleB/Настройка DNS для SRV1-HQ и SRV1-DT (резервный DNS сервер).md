### Задание:

b) Сконфигурируйте SRV1-DT, как резервный DNS сервер

d) В качестве DNS сервера пересылки используйте любой общедоступный DNS сервер

### Вариант реализации:

### SRV1-DT:

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

- - Вносим следующие изменения:
        - Основные параметры описаны в [Настройка DNS для SRV1-HQ и SRV1-DT (основной DNS сервер)](https://sysahelper.ru/mod/page/view.php?id=476)

![](https://sysahelper.ru/pluginfile.php/828/mod_page/content/3/image.png)

- Чтобы bind работал в режиме SLAVE, нужно настроить control:

```
control bind-slave enabled
```

- Задаём настройки для внутреннего DNS:

```
cat <<EOF > /etc/net/ifaces/ens19/resolv.conf
  search au.team
  nameserver 192.168.33.66
  nameserver 192.168.11.66
EOF
```

- Перезагружаем службу **network**:

```
systemctl restart network
```

- Включаем и добавляем в автозагрузку службу **bind**:

```
systemctl enable --now bind
```

- Проверяем:

![](https://sysahelper.ru/pluginfile.php/828/mod_page/content/3/image%20%281%29.png)