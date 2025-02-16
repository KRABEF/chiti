### Задание:

c) Разверните Docker контейнер используя образ из локального Registry.

- 1. Имя контейнера web
- 2. Контейнер должно работать на порту 80
- 3. Обеспечьте запуск контейнера после перезагрузки компьютера

### Вариант реализации:

### SRV2-DT:

- Запускаем **docker**-контейнер:
    - С именем **web** из образа **localhost:5000/web:01**
    - Контейнер будет слушать сетевые запросы на порту **80**, а параметр **--restart=always** позволит автоматически запускаться контейнеру после перезагрузки сервера

```
docker run -d -p 80:80 --restart=always --name web localhost:5000/web:1.0
```

- Проверяем:
    - Запущенный docker-контейнер:

![](https://sysahelper.ru/pluginfile.php/843/mod_page/content/4/image.png)

- - Доступ до веб-сервера и приложения

![](https://sysahelper.ru/pluginfile.php/843/mod_page/content/4/image%20%281%29.png)

- - Или:

![](https://sysahelper.ru/pluginfile.php/843/mod_page/content/4/image%20%282%29.png)