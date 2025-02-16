### Задание:

b) Напишите Dockerfile для приложения web.

- 1. В качестве базового образа используйте nginx:alpine
- 2. Содержание index.html

```
<html>
    <body>
        <center><h1><b>WEB</b></h1></center>
    </body>
</html>
```

- 3. Соберите образ приложения web и загрузите его в ваш Registry.
    - i. Используйте номер версии 1.0 для вашего приложения
    - ii. Образ должен быть доступен для скачивания и дальнейшего запуска на локальной машине

### Вариант реализации:

### SRV2-DT:

- Напишим **Dockerfile** для приложения **web**:

```
vim Dockerfile
```

- - Содержимое, где:
        - **FROM** - задаёт базовый образ;
        - **COPY** - копирует с локального хоста в контейнер:

![](https://sysahelper.ru/pluginfile.php/842/mod_page/content/3/image.png)

- Создаём файл **index.html**:

```
vim index.html
```

- - Содержимое по требованию задания:

![](https://sysahelper.ru/pluginfile.php/842/mod_page/content/3/image%20%281%29.png)

- Выполняем сборку docker-образа:
    - **-t** - позволяет присвоить имя собираемому образу;
    - "**.**" - говорит о том что **Dockerfile** находится в текущей директории откуда выполняется данная команда и имеет имя именно **Dockerfile:**

```
docker build -t localhost:5000/web:1.0 .
```

- Результат:

![](https://sysahelper.ru/pluginfile.php/842/mod_page/content/3/image%20%282%29.png)

- Проверяем:
    - Наличие собранного образа

![](https://sysahelper.ru/pluginfile.php/842/mod_page/content/3/image%20%283%29.png)

- Загружаем образ собранный из **Dockerfile** в локальной **DockerRegistry**

```
docker push localhost:5000/web:1.0
```

- - Результат:

![](https://sysahelper.ru/pluginfile.php/842/mod_page/content/3/image%20%284%29.png)

- Проверяем:
    - Возможность загрузки из локального Docker Registry:  
        - Сперва удаляем образ **localhost:5000/web:1.0**

![](https://sysahelper.ru/pluginfile.php/842/mod_page/content/3/image%20%285%29.png)

- - - Загружаем образ приложения **web** из локального Docker Registry:

![](https://sysahelper.ru/pluginfile.php/842/mod_page/content/3/image%20%286%29.png)