### Задание:

a) Создайте локальный Docker Registry.

### Вариант реализации:

### SRV2-DT:

- Установим пакет для работы с **Docker**:

```
apt-get update && apt-get install -y docker-engine
```

- Запускаем и добавляем в автозагрузку службу **docker**:

```
systemctl enable --now docker.service
```

- Создаём и запускаем локальный **Docker Registry**:
    - Поднимает **контейнер Docker** с именем **DockerRegistry** из образа **registry:2** 
    - Контейнер будет слушать сетевые запросы **на порту 5000**
    - Параметр **--restart=always** позволит автоматически запускаться контейнеру после перезагрузки сервера.

```
docker run -d -p 5000:5000 --restart=always --name DockerRegistry registry:2
```

- Проверяем:

![](https://sysahelper.ru/pluginfile.php/841/mod_page/content/2/image.png)