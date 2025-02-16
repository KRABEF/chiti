### Задание:

a) Сконфигурируйте основной доменный контроллер на SRV1-HQ

- 1. Используйте модуль BIND9_DLZ

### Вариант реализации:

### SRV1-HQ:

- Установим необходимый пакет:

```
apt-get install task-samba-dc -y
```

- ⁠ Настройка BIND9 для работы с Samba AD:  
    - Отключаем chroot:

```
control bind-chroot disabled
```

- - Отключаем KRB5RCACHETYPE:

```
grep -q KRB5RCACHETYPE /etc/sysconfig/bind || echo 'KRB5RCACHETYPE="none"' >> /etc/sysconfig/bind
```

- - Подключаем плагин BIND_DLZ:

```
grep -q 'bind-dns' /etc/bind/named.conf || echo 'include "/var/lib/samba/bind-dns/named.conf";' >> /etc/bind/named.conf
```

- - Отредактируем файл **/etc/bind/options.conf**:

```
vim /etc/bind/options.conf
```

- - - В раздел **options** необходимо добавить строки:

![](https://sysahelper.ru/pluginfile.php/832/mod_page/content/2/image.png)

- - - В раздел **logging** необходимо добавить строку:

![](https://sysahelper.ru/pluginfile.php/832/mod_page/content/2/image%20%281%29.png)

- Выполняем остановку службы **bind**:

```
systemctl stop bind
```

- Необходимо очистить базы и конфигурацию Samba:

```
rm -f /etc/samba/smb.conf
rm -rf /var/lib/samba
rm -rf /var/cache/samba
mkdir -p /var/lib/samba/sysvol
```

- Запускаем интерактивную установку контроллера домена:

```
samba-tool domain provision
```

- - Результат:
        - Все параметры за исключением **DNS backend** должны подставляться автоматически корректными

![](https://sysahelper.ru/pluginfile.php/832/mod_page/content/2/image%20%282%29.png)

- - Результат:

![](https://sysahelper.ru/pluginfile.php/832/mod_page/content/2/image%20%283%29.png)

- Запускаем службы **samba** и **bind**:

```
systemctl enable --now samba
```

```
systemctl start bind
```

- Настраиваем Kerberos:

```
cp /var/lib/samba/private/krb5.conf /etc/krb5.conf
```

- Проверяем:

![](https://sysahelper.ru/pluginfile.php/832/mod_page/content/2/image%20%284%29.png)