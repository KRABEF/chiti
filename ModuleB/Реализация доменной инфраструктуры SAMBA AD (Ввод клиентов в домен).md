### Задание:

1. Клиентами домена являются ADMIN-DT, CLI-DT, ADMIN-HQ, CLI-HQ.

### Вариант реализации:

### ADMIN-HQ:

- Вводим в домен:

![](https://sysahelper.ru/pluginfile.php/861/mod_page/content/5/image%20%283%29.png)

![](https://sysahelper.ru/pluginfile.php/861/mod_page/content/5/image%20%284%29.png)

![](https://sysahelper.ru/pluginfile.php/861/mod_page/content/5/image%20%285%29.png)

#### 8 - Перезагрузить виртуальную машину

### CLI-HQ:

- Вводим в домен аналогично **ADMIN-HQ**:

![](https://sysahelper.ru/pluginfile.php/861/mod_page/content/5/image%20%286%29.png)

### ADMIN-DT:

- Вводим в домен аналогично **ADMIN-HQ**:

![](https://sysahelper.ru/pluginfile.php/861/mod_page/content/5/image%20%287%29.png)

### CLI-DT:

- Вводим в домен аналогично **ADMIN-HQ**:

![](https://sysahelper.ru/pluginfile.php/861/mod_page/content/5/image%20%288%29.png)

### SRV1-HQ:

- Проверяем наличие клиентов в домене:

```
samba-tool computer list
```

- - Результат:

![](https://sysahelper.ru/pluginfile.php/861/mod_page/content/5/image%20%289%29.png)

- Перемещаем клинетов в подразредения:
    - **ADMIN-DT** в подразделение **ADMIN**:

```
samba-tool computer move ADMIN-DT 'OU=ADMIN,DC=au,DC=team'
```

- - **ADMIN-HQ** в подразделение **ADMIN**:

```
samba-tool computer move ADMIN-HQ 'OU=ADMIN,DC=au,DC=team'
```

- - **CLI-DT** в подразделение **CLI**:

```
samba-tool computer move CLI-DT 'OU=CLI,DC=au,DC=team'
```

- - **CLI-HQ** в подразделение **CLI**:

```
samba-tool computer move CLI-HQ 'OU=CLI,DC=au,DC=team'
```

- Проверяем:

![](https://sysahelper.ru/pluginfile.php/861/mod_page/content/5/image%20%2810%29.png)