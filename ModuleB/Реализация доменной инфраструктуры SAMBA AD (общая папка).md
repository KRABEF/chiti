### Задание:

h) Реализуйте общую папку на SRV1-HQ

- 1. Используйте название SAMBA
- 2. Используйте расположение /opt/data

### Вариант реализации:

### SRV1-HQ:

- Создаём директорию для общей папки:

```
mkdir /opt/data
```

- Задаём права на директорию:

```
chmod 777 /opt/data
```

- Реализуем общую папку, вносим следующую информацию в конфигурационный файл **/etc/samba/smb.conf**:

![](https://sysahelper.ru/pluginfile.php/835/mod_page/content/5/image.png)

- Перезагружаем службу **samba**:

```
systemctl restart samba
```

- Проверяем:

![](https://sysahelper.ru/pluginfile.php/835/mod_page/content/5/image%20%281%29.png)