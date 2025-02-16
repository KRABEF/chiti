https://sysahelper.ru/mod/page/view.php?id=551
### Задача:

- Подключение Terraform к провайдеру OpenStack
- Подключение openstack-cli к Кибер Инфраструктура

_Исходя из требований задания, реализуем в правильных директориях правильные файлы (наименование)_

Имеем пользовательские данные для доступа к Кибер Инфраструктура (в данном конкретном случае):

- Кибер Инфраструктура доступна по доменному имени - **cyber-infra.local.prof**;
- Имя домена (в контексте Кибер Инфраструктура) - **Region2025**;
- Пользователь - **user01**;
- Пароль пользователя - **user01P@ssw0rd**;

### Вариант реализации:

### ControlVM:

- Создадим файл конфигурации зеркала Terraform
    - Файл должен иметь имя **.terraformrc**
    - Файл должен быть расположен в домашнем каталоге пользователя

```
vim ~/.terraformrc
```

- - Помещаем в данный файл следущее содержимое:

```
provider_installation {
    network_mirror {
        url = "https://terraform-mirror.mcs.mail.ru"
        include = ["registry.terraform.io/*/*"]
    }
    direct {
        exclude = ["registry.terraform.io/*/*"]
    }
}
```

- Из под пользователя **altlinux** создаём директорию **bin** в домашнем каталоге пользователя и переходим в неё:

```
mkdir ~/bin && cd ~/bin
```

- Создадим файл **cloud.conf** в котором опишем необходимые переменные (будут использоваться как переменные окружения) для работы **terraform** и **openstack-cli**  с Кибер Инфраструктура:

```
vim cloud.conf
```

- - Помещаем в данный файл следущее содержимое:

```
# Terraform
export TF_VAR_OS_AUTH_URL=https://cyber-infra.local.prof:5000/v3
export TF_VAR_OS_PROJECT_NAME=Project1
export TF_VAR_OS_USERNAME=user01
export TF_VAR_OS_PASSWORD=user01P@ssw0rd

# openstacl-cli
export OS_AUTH_URL=https://cyber-infra.local.prof:5000/v3
export OS_IDENTITY_API_VERSION=3
export OS_AUTH_TYPE=password
export OS_PROJECT_DOMAIN_NAME=Region2025
export OS_USER_DOMAIN_NAME=Region2025
export OS_PROJECT_NAME=Project1
export OS_USERNAME=user01
export OS_PASSWORD=user01P@ssw0rd
```

- Применяем переменные окружения указанные в файле
    - Стоит учесть данное действие про конечном формирование финального скрипта **cloudinit.sh** (будет рассмотрено далее)

```
source cloud.conf
```

- Проверяем возможность взаимодействовать чере **openstack-cli** с Кибер Инфраструктура:
    - Например выведем список серверов (Виртуальных Машин)

![](https://sysahelper.ru/pluginfile.php/915/mod_page/content/5/image.png)

- Создадим файл **provider.tf** и опишем параметры для подключения к провайдеру **openstack** для работы с Кибер Инфраструктура используя данного провайдера:

```
vim provider.tf
```

- - Помещаем в данный файл следущее содержимое:

```
terraform {
  required_providers {
    openstack = {
      source  = "terraform-provider-openstack/openstack"
      version = "2.1.0"
    }
  }
}

provider "openstack" {
  auth_url    = var.OS_AUTH_URL
  tenant_name = var.OS_PROJECT_NAME
  user_name   = var.OS_USERNAME
  password    = var.OS_PASSWORD
  insecure    = true
}
```

- Создадим файл **variables.tf** и опишем переменные для подключения к провайдеру **openstack**:
    - При таком подходе переменные будут браться из значений определённых в переменных окружения, которые в свою очередь были указаны в файле **cloud.conf**, т.к. по условиям задания эксперты могут только отредактировать его (например для запуска в другом проекте):

```
vim variables.tf
```

- - Помещаем в данный файл следущее содержимое:

```
variable "OS_AUTH_URL" {
	type = string
	sensitive = true
}

variable "OS_PROJECT_NAME" {
	type = string
	sensitive = true
}

variable "OS_USERNAME" {
	type = string
	sensitive = true
}

variable "OS_PASSWORD" {
	type = string
	sensitive = true
}
```

- Инициализируем текущий каталог для работы с **terraform** и провайдером **openstack**:

```
terraform init
```

- - Результат:

![](https://sysahelper.ru/pluginfile.php/915/mod_page/content/5/image%20%281%29.png)