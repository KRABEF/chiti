### Задание:

a) Настройте узел управления на базе ADMIN-DT

- 1. Используйте стандартную пакетную версию ansible.

b) Сконфигурируйте инвентарь

- 1. Инвентарь должен располагаться по пути /etc/ansible/inventory.
    - i. Настройте запуск данного инвентаря по умолчанию
- 2. Инвентарь должен содержать три группы устройств:
    - i. Networking (R-DT, R-HQ)
    - ii. Servers (SRV1-HQ, SRV1-DT, SRV2-DT, SRV3-DT)
    - iii. Clients (ADMIN-HQ, ADMIN-DT, CLI-HQ, CLI-DT)

### Вариант реализации:

### ADMIN-DT:

- Установим пакет **ansible**:

```
apt-get update && apt-get install -y ansible sshpass
```

- Назначем необходимые права на директорию **/etc/ansible**:

```
chown -R root:user /etc/ansible
```

```
chmod -R 774 /etc/ansible
```

- Из  под обычного пользователя **user** пероходим для дальнейшей работы в директорию **/etc/ansible**:

```
cd /etc/ansible
```

- Проверяем:

![](https://sysahelper.ru/pluginfile.php/847/mod_page/content/2/image.png)

- Создаём инвентарный файл:

```
vim inventory
```

- - Помещаем в него следующее содержимое:

![](https://sysahelper.ru/pluginfile.php/847/mod_page/content/2/image%20%281%29.png)

- Редактируем конфигурационный файл **ansible.cfg**:

```
vim ansible.cfg
```

- - Вносим следующие изменения:
        - Инвентарь должен располагаться по пути **/etc/ansible/inventory**
        - Отключите проверку **SSH–ключа на хосте**

![](https://sysahelper.ru/pluginfile.php/847/mod_page/content/2/image%20%282%29.png)