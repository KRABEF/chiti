### Задание:

1. Создайте 30 пользователей user1-user30 с паролем P@ssw0rd.

2. Пользователи user1-user10 должны входить в состав группы group1.

3. Пользователи user11-user20 должны входить в состав группы group2.

4. Пользователи user21-user30 должны входить в состав группы group3.

5. Создайте подразделения CLI и ADMIN

- i. Поместите клиентов в подразделения в зависимости от их роли.

### Вариант реализации:

### SRV1-HQ:

- Создаём группы **group1, group2** и **group3**:

```
samba-tool group add group1
```

```
samba-tool group add group2
```

```
samba-tool group add group3
```

- - Проверяем:

![](https://sysahelper.ru/pluginfile.php/833/mod_page/content/3/image.png)

- Создаём пользователей **user1-user30** с паролем **P@ssw0rd**:
    - Создаём пользователей **user1-user10** - и добавляем в группу **group1**:

```
for i in {1..10}; do
  samba-tool user add user$i P@ssw0rd;
  samba-tool user setexpiry user$i --noexpiry;
  samba-tool group addmembers "group1" user$i;
done
```

- - Создаём пользователей **user11-user20** - и добавляем в группу **group2**:

```
for i in {11..20}; do
  samba-tool user add user$i P@ssw0rd;
  samba-tool user setexpiry user$i --noexpiry;
  samba-tool group addmembers "group2" user$i;
done
```

- - Создаём пользователей **user21-user30** - и добавляем в группу **group3**:

```
for i in {21..30}; do
  samba-tool user add user$i P@ssw0rd;
  samba-tool user setexpiry user$i --noexpiry;
  samba-tool group addmembers "group3" user$i;
done
```

- Проверяем:

![](https://sysahelper.ru/pluginfile.php/833/mod_page/content/3/image%20%281%29.png)

- Создим подразделения **CLI** и **ADMIN**:

```
samba-tool ou add 'OU=CLI'
samba-tool ou add 'OU=ADMIN'
```

- Проверяем:

![](https://sysahelper.ru/pluginfile.php/833/mod_page/content/3/image%20%282%29.png)