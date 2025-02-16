https://sysahelper.ru/mod/page/view.php?id=550
### Задача:

- Установить Terraform на ControlVM
- Установить openstack-cli на Control VM

### Вариант реализации:

#### ControlVM:

- Устанавляваем **wget** и **unzip**:

```
sudo apt-get update && sudo apt-get install -y wget unzip
```

- Скачиваем архив с **Terraform**:
    - С Зеркала Яндекса:

```
wget https://hashicorp-releases.yandexcloud.net/terraform/1.10.3/terraform_1.10.3_linux_arm64.zip
```

- - С Зеркала VK Cloud

```
wget https://hashicorp-releases.mcs.mail.ru/terraform/1.10.3/terraform_1.10.3_linux_amd64.zip
```

- Распаковываем его в каталог **/usr/local/bin**:

```
sudo unzip  terraform_1.10.3_linux_amd64.zip -d /usr/local/bin/
```

- Проверяем:

![](https://sysahelper.ru/pluginfile.php/914/mod_page/content/4/image%20%281%29.png)

- Установим **openstack-cli**:

```
sudo apt-get install -y python3-module-openstackclient python3-module-pip
```

- Проверяем:

![](https://sysahelper.ru/pluginfile.php/914/mod_page/content/4/image%20%282%29.png)

- Установим необходимые модули для работы:
    - **python-octaviaclient** - компонент для работы с облачными балансировщиками нагрузки;
    - **python-cinderclient** - компонент для работы блочного хранилища и расширений;
    - **python-novaclient** - компонент для работы облачных вычислений (ВМ) и расширений;

```
pip3 install python-octaviaclient
```

```
pip3 install python-cinderclient
```

```
pip3 install python-novaclient
```