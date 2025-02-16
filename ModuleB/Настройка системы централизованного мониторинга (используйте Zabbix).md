### Задание:

a) В качестве сервера системы централизованного мониторинга используйте SRV3-DT

b) В качестве системы централизованного мониторинга используйте Zabbix

- 1. В качестве сервера баз данных используйте PostgreSQL
    - i. Имя базы данных: zabbix
    - ii. Пользователь базы данных: zabbix
    - iii. Пароль пользователя базы данных: zabbixpwd
- 2. В качестве веб-сервера используйте Apache

c) Система централизованного мониторинга должна быть доступна для внутренних пользователей по адресу http://<IP адрес SRV3-DT>/zabbix

- 1. Администратором системы мониторинга должен быть пользователь Admin с паролем P@ssw0rd
- 2. Часовой пояс по умолчанию должен быть Europe/Moscow

### Вариант реализации:

### SRV3-DT:

- Устанавливаем **необходимые** пакеты:

```
apt-get update && apt-get install -y postgresql16-server zabbix-server-pgsql
```

- Создаём системные базы данных для корректной работы PostgreSQL:

```
/etc/init.d/postgresql initdb
```

- Включаем и добавляем в автозагрузку службу **postgresql**:

```
systemctl enable --now postgresql
```

- Создаём пользоавтеля **zabbix** в базе данных **PostgreSQL**:

```
su - postgres -s /bin/sh -c 'createuser --no-superuser --no-createdb --no-createrole --encrypted --pwprompt zabbix'
```

- - После запуска данной команды - задаём в качестве пароля для пользователя **zabbix** - пароль **zabbixpwd** и подтверждаем его:

![](https://sysahelper.ru/pluginfile.php/844/mod_page/content/2/image%20%281%29.png)

- Создаём базу данных с именем **zabbix**:

```
su - postgres -s /bin/sh -c 'createdb -O zabbix zabbix'
```

- Выполняем перезагрузку службы **postgresql**:

```
systemctl restart postgresql
```

- Добавляем в базу данные для веб-интерфейса:

```
su - postgres -s /bin/sh -c 'psql -U zabbix -f /usr/share/doc/zabbix-common-database-pgsql-*/schema.sql zabbix'
```

```
su - postgres -s /bin/sh -c 'psql -U zabbix -f /usr/share/doc/zabbix-common-database-pgsql-*/images.sql zabbix'
```

```
su - postgres -s /bin/sh -c 'psql -U zabbix -f /usr/share/doc/zabbix-common-database-pgsql-*/data.sql zabbix'
```

- Устанавливаем пакеты для веб-сервера **apache2**:

```
apt-get install -y apache2 apache2-mod_php8.2
```

- Включаем и добавляем в автозагрузку службу отвечающую за веб-сервер **apache2**:

```
systemctl enable --now httpd2
```

- Установим **PHP** и необходимые модули для корректной работы:

```
apt-get install -y php8.2 php8.2-{mbstring,sockets,gd,xmlreader,pgsql,ldap,openssl}
```

- Меняем некоторые опции **php** в файле **/etc/php/8.2/apache2-mod_php/php.ini**: 

```
vim /etc/php/8.2/apache2-mod_php/php.ini
```

- - Находим следующие параметры и приводим их к следующему виду:

![](https://sysahelper.ru/pluginfile.php/844/mod_page/content/2/image%20%282%29.png)

![](https://sysahelper.ru/pluginfile.php/844/mod_page/content/2/image%20%283%29.png)

- Перезапускаем службу отвечающую за веб-сервер **apache2**:

```
systemctl restrart httpd2
```

- Вносим изменения в конфигурационный файл **/etc/zabbix/zabbix_server.conf:**

```
vim /etc/zabbix/zabbix_server.conf
```

- - Добавляем следующие изменения:

![](https://sysahelper.ru/pluginfile.php/844/mod_page/content/2/image%20%284%29.png)

- Добавим **Zabbix-сервер** в автозапуск и запустить его:

```
systemctl enable --now zabbix_pgsql
```

- Установим пакет с веб-интерфейсом Zabbix:

```
apt-get install zabbix-phpfrontend-{apache2,php8.2} -y
```

- Включаем аддоны в **apache2**: 

```
ln -s /etc/httpd2/conf/addon.d/A.zabbix.conf /etc/httpd2/conf/extra-enabled/
```

- Изменяем права доступа к конфигурационному каталогу веб-интерфейса, чтобы веб-установщик мог записать конфигурационный файл: 

```
chown apache2:apache2 /var/www/webapps/zabbix/ui/conf
```

- Перезапускаем службу отвечающую за веб-сервер **apache2**:

```
systemctl restrart httpd2
```

- Далее установка производится средствами веб-интерфейса:
    - Например с **ADMIN-DT** в браузере перейти на страницу установки Zabbix сервера **http://<IP адрес SRV3-DT>/zabbix**: 

![](https://sysahelper.ru/pluginfile.php/844/mod_page/content/2/image%20%285%29.png)

![](https://sysahelper.ru/pluginfile.php/844/mod_page/content/2/image%20%286%29.png)

1. В качестве базы данных выбираем - **PostgreSQL**;
2. В качестве сервера базы данных указываем **IP** - адрес или имя **localhost;**
3. Указываем имя созданной базы данных **zabbix;**
4. Указываем имя созданного пользователя **zabbix;**
5. Указываем пароль для пользователя zabbix  **zabbixpwd**;
6. Нажимаем **Next step**

![](https://sysahelper.ru/pluginfile.php/844/mod_page/content/2/image%20%287%29.png)

- При необходимости задаём имя серверу и нажимаем **Next step**

![](https://sysahelper.ru/pluginfile.php/844/mod_page/content/2/image%20%288%29.png)

- Проверяем введённые ранее параметры и нажимаем **Next step**

![](https://sysahelper.ru/pluginfile.php/844/mod_page/content/2/image%20%289%29.png)

- Нажимаем **Finish**

![](https://sysahelper.ru/pluginfile.php/844/mod_page/content/2/image%20%2810%29.png)

- Выполняем вход из под пользователя по умолчанию: **Admin** с паролем: **zabbix**

![](https://sysahelper.ru/pluginfile.php/844/mod_page/content/2/image%20%2812%29.png)

- В качестве пароля для пользователя **Admin -** необходимо установить **P@ssw0rd:**
    - переходим в настройки аутентификации и снимаем галочку, которая запрещает использование слабых паролей

![](https://sysahelper.ru/pluginfile.php/844/mod_page/content/2/image%20%2813%29.png)

- Задаём новый пароль **P@ssw0rd** - для пользователя **Admin**

![](https://sysahelper.ru/pluginfile.php/844/mod_page/content/2/image%20%2814%29.png)

- Результат:

![](https://sysahelper.ru/pluginfile.php/844/mod_page/content/2/image%20%2815%29.png)