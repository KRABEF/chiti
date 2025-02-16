https://sysahelper.ru/mod/page/view.php?id=567
### Задание:

c) Развертывание WordPress с использованием Docker Compose

- 3.1. Создание файла wordpress.yml:
    - i. В домашней директории пользователя создайте файл  wordpress.yml, описывающий стек контейнеров для WordPress и MySQL.
    - 3.2. Конфигурация стека Docker Compose:
        - i. Определите два сервиса: 
            - **wordpress:** 
                
                - Используйте образ **wordpress:latest**.
                - Свяжите с сетью wordpress-network.
                - Прокиньте порт 80 для доступа к WordPress извне.
                - Настройте необходимые переменные окружения (WORDPRESS_DB_HOST, WORDPRESS_DB_USER, WORDPRESS_DB_PASSWORD, WORDPRESS_DB_NAME и тд.).
            - **mysql:** 
                - Используйте образ **mysql:5.7**.
                - Свяжите с сетью wordpress-network.
                - Создайте volume для хранения данных базы данных.
                - Настройте необходимые переменные окружения (MYSQL_DATABASE,MYSQL_USER, MYSQL_PASSWORD, MYSQL_ROOT_PASSWORD и тд.).
- 3.3. Запуск стека:
    - i. Запустите Docker Compose с файлом wordpress.yml.
    - ii. Убедитесь, что WordPress доступен по указанному порту и готов к настройке.

### Вариант реализации:

### ControlVM:

- В домашней директории пользователя **altlinux** из под пользователя **altlinux** создаём файл **wordpress.yml**:

```
vim ~/wordpress.yml
```

- - Помещаем в него следующее содержимое:
        - Реализуем функционал согласно требованиям задания:

```
version: '3.1'

services:
  wordpress:
    image: wordpress:latest
    container_name: wordpress
    networks:
      - wordpress-network
    ports:
      - "80:80"
    environment:
      WORDPRESS_DB_HOST: mysql:3306
      WORDPRESS_DB_NAME: wordpress
      WORDPRESS_DB_USER: wordpress
      WORDPRESS_DB_PASSWORD: wordpress
    depends_on:
      - mysql
    volumes:
      - wordpress_data:/var/www/html

  mysql:
    image: mysql:5.7
    container_name: mysql
    networks:
      - wordpress-network
    environment:
      MYSQL_ROOT_PASSWORD: toor
      MYSQL_DATABASE: wordpress
      MYSQL_USER: wordpress
      MYSQL_PASSWORD: wordpress
    volumes:
      - mysql_data:/var/lib/mysql

networks:
  wordpress-network:
    driver: bridge
    name: wordpress-network

volumes:
  wordpress_data:
  mysql_data:
```

- Устанавливаем **docker-compose-v2**:

```
sudo apt-get install -y docker-compose-v2
```

- Выполняем запуск стека контейнеров для **WordPress** и **MySQL**:
    - Запуск команды выполняется из домашнего каталога пользователя **altlinux**;

```
sudo docker compose -f wordpress.yml up -d
```

- - Результат:

![](https://sysahelper.ru/pluginfile.php/932/mod_page/content/1/image.png)

- Проверяем запущенные контейнеры:

![](https://sysahelper.ru/pluginfile.php/932/mod_page/content/1/image%20%281%29.png)

- Проверяем что есть сеть с именем **wordpress-network**:

![](https://sysahelper.ru/pluginfile.php/932/mod_page/content/1/image%20%282%29.png)

- Проверяем доступ к веб-интерфейсу настройки **WordPress**:
    - Обращаясь на Плавающий-IP адрес **ControlVM** на порт **80**;

![](https://sysahelper.ru/pluginfile.php/932/mod_page/content/1/image%20%283%29.png)