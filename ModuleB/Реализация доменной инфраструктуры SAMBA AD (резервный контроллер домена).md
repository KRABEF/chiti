### Задание:

f) В качестве резервного контроллера домена используйте SRV1-DT.

1. Используйте модуль BIND9_DLZ

### Вариант реализации:

### SRV1-DT:

- Реализуем резервный контроллер домена с модулем **BIND9_DLZ**:
    - Установим необходимый пакет:

```
apt-get install task-samba-dc -y
```

- - Останавливаем конфликтующие службы **krb5kdc** и **slapd**, а также **bind**:

```
for service in smb nmb krb5kdc slapd bind; do 
  systemctl disable $service; 
  systemctl stop $service; 
done
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

![](https://sysahelper.ru/pluginfile.php/834/mod_page/content/4/image.png)

- - - В раздел **logging** необходимо добавить строку:

![](https://sysahelper.ru/pluginfile.php/834/mod_page/content/4/image%20%281%29.png)

- - Установим следующие параметры в файле конфигурации клиента Kerberos:

```
vim /etc/krb5.conf
```

- - - Содержимое:

![](https://sysahelper.ru/pluginfile.php/834/mod_page/content/4/image%20%282%29.png)

- - Запросим билет Kerberos администратора домена: 

```
kinit administrator@AU.TEAM
```

- - - Проверяем:

![](https://sysahelper.ru/pluginfile.php/834/mod_page/content/4/image%20%283%29.png)

_P.S. предварительно на FW-DT должен быть отключён перехват пользовательских DNS запросов_

- Необходимо очистить базы и конфигурацию Samba:

```
rm -f /etc/samba/smb.conf
rm -rf /var/lib/samba
rm -rf /var/cache/samba
mkdir -p /var/lib/samba/sysvol
```

- Вводим **SRV1-DT** в домен **au.team** в качестве контроллера домена: 

```
samba-tool domain join au.team DC -Uadministrator --realm=au.team --dns-backend=BIND9_DLZ
```

- Включаем и добавляем в автозагрузку службы **samba** и **bind**:

```
systemctl enable --now samba
```

```
systemctl enable --now bind
```

- Выполняем процедуру репликации - с первого контроллера домена на второй:

```
samba-tool drs replicate srv1-dt.au.team srv1-hq.au.team dc=au,dc=team -Uadministrator
```

- Выполняем процедуру репликации на первый контроллер домена со второго:

```
samba-tool drs replicate srv1-hq.au.team srv1-dt.au.team dc=au,dc=team -Uadministrator
```

- - Результат:

![](https://sysahelper.ru/pluginfile.php/834/mod_page/content/4/image%20%284%29.png)

- Проверяем репликацию:

![](https://sysahelper.ru/pluginfile.php/834/mod_page/content/4/image%20%285%29.png)