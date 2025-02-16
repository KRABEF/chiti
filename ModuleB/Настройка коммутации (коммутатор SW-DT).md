### Задание:

b) Настройте коммутатор SW-DT

- 1. В качестве коммутатора используйте соответствующий виртуальный коммутатор.

c) Для каждого офиса устройства должны находиться в соответствующих VLAN

- 1. Клиенты - vlan110,
- 2. Сервера – в vlan220,
- 3. Администраторы – в vlan330.

### Вариант реализации:

- В качестве коммутатора используется виртуальный коммутатор на уровне Альт Виртуализаци PVE (на котором развёрнут стенд) **vmbr112**:  
    - подразумевается что для **vmbr112** в текущем случае выставлен чек-бокс:

![](https://sysahelper.ru/pluginfile.php/817/mod_page/content/2/image.png)

- Реализуем необходимые порты "доступа (access)":
    - **SRV1-DT** (vlan 220 - Сервера):

![](https://sysahelper.ru/pluginfile.php/817/mod_page/content/2/image%20%281%29.png)

- - **SRV2-DT** (vlan 220 - Сервера):

![](https://sysahelper.ru/pluginfile.php/817/mod_page/content/2/image%20%282%29.png)

- - **SRV3-DT** (vlan 220 - Сервера):

![](https://sysahelper.ru/pluginfile.php/817/mod_page/content/2/image%20%283%29.png)

- - **CLI-DT** (vlan 110 - Клиента):

![](https://sysahelper.ru/pluginfile.php/817/mod_page/content/2/image%20%284%29.png)