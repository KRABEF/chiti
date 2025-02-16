### Задание:

c) На всех устройства (кроме FW-DT) создайте пользователя sshuser с паролем P@ssw0rd

- 1. Пользователь sshuser должен иметь возможность запуска утилиты sudo без дополнительной аутентификации.
- 2. На маршрутизаторах пользователь sshuser должен обладать максимальными привилегиями.

### Вариант реализации:

### SRV1-DT | SRV2-DT | SRV3-DT | SW1-HQ | SW2-HQ | SW3-HQ    | SRV1-HQ:

- Для создания пользователя **sshuser** используем утилиту [useradd](https://www.opennet.ru/man.shtml?topic=useradd&category=8&russian=0):
    - где:
        - **useradd** - утилита для создания пользователя;
        - **sshuser** - имя пользователя;
        - **-m** - если домашнего каталога пользователя не существует, то он будет создан;
        - **-U** - создаётся одноимённая группа и пользователь автоматически в неё добавляется;
        - **-s /bin/bash** - задаётся командный интерпретатор для пользователя:

```
useradd sshuser -m -U -s /bin/bash
```

- Проверяем созданного пользователя с необходимыми параметрами:
    - для этого необходимо открыть содержимое файла **/etc/passwd** с помощью утилиты **cat** или же текстовым редактором, например: **vim**;
    - или же использовать утилиту [grep](https://ru.wikipedia.org/wiki/Grep) и передать ей в качестве значения имя пользователя:

```
grep sshuser /etc/passwd
```

- - Результат на примере для **SRV1-HQ****:**

![](https://sysahelper.ru/pluginfile.php/814/mod_page/content/2/image.png)

_Аналогично для всех остальных устройств_

- Для назначения пользователя **sshuser** пароля **P@ssw0rd** используем утилиту [passwd](https://www.opennet.ru/man.shtml?topic=passwd&category=1):
    - во время запуска - утилита в интерактивном режиме попросит ввести пароль для пользователя и затем подтвердить его:

```
passwd sshuser
```

- - Результат запуска утилиты:

![](https://sysahelper.ru/pluginfile.php/814/mod_page/content/2/image%20%281%29.png)

- Проверяем:
    - на **SRV-HQ1** - выполняем вход из под пользователя **sshuser** с паролем **P@ssw0rd**:

![](https://sysahelper.ru/pluginfile.php/814/mod_page/content/2/image%20%282%29.png)

Аналогично для всез остальных устройств_:_ SRV1-DT, SRV2-DT, SRV3-DT, SW1-HQ, SW2-HQ, SW3-HQ.

- Реализуем возможность запуска утилиты sudo пользователю sshuser без ввода пароля:
- [Рекумендуется прочитать перед выполнением](https://www.altlinux.org/Sudo)

- Добавляем пользователя **sshuser** в группу **wheel** для этого используем утилиту [usermod](https://www.opennet.ru/man.shtml?topic=usermod&category=8&russian=0)
    - поскольку штатное состояние политики: **wheelonly** (Означает что пользователь из группы wheel имеет право запускать саму команду sudo, но не означает, что он через sudo может выполнить какую-то команду с правами root)
    - где:
        - **usermod** - утилита для изменения и работы с параметрами пользователя;
        - **-aG** - параметр чтобы добавить пользователя в дополнительную группу(ы). Использовать только вместе с параметром **-G;**
        - **wheel** - имя группы;
        - **sshuser** - имя пользователя:

```
usermod -aG wheel sshuser
```

- Добавляем следующую строку в файл в [/etc/sudoers](https://www.opennet.ru/man.shtml?topic=sudoers&category=5&russian=0) чтобы была возможность запуска **sudo** без дополнительной аутентификации:

```
echo "sshuser ALL=(ALL:ALL) NOPASSWD: ALL" >> /etc/sudoers
```

- - **P.S.** или же открываем через текстовы редактор, например: **vim**
- Проверяем:
    - на **SRV-HQ1** выполняем вход из под пользователя **sshuser** и пытаемся повысить привелегии:

![](https://sysahelper.ru/pluginfile.php/814/mod_page/content/2/image%20%283%29.png)

Аналогично для всез остальных устройств_:_ SRV1-DT, SRV2-DT, SRV3-DT, SW1-HQ, SW2-HQ, SW3-HQ.

### R-DT | R-HQ:

- Создаём пользователя **sshuser** на маршрутизаторах с паролем **P@ssw0rd** и с максимальными привилегиями:
    - максимальным привилегиям в EcoRouter - соответствуем роль **admin**:

```
r-hq#configure terminal
r-hq(config)#username sshuser
r-hq(config-user)#password P@ssw0rd
r-hq(config-user)#role admin 
r-hq(config-user)#exit
r-hq(config)#write
r-hq(config)#
```

- Проверяем:
    - на **R-DT** выполняем вход из под пользователя **sshuser**:

![](https://sysahelper.ru/pluginfile.php/814/mod_page/content/2/image%20%284%29.png)

- - на **R-HQ** выполняем вход из под пользователя **sshuser**:

![](https://sysahelper.ru/pluginfile.php/814/mod_page/content/2/image%20%285%29.png)

### CLI | ADMIN-DT | CLI-DT | ADMIN-HQ | CLI-HQ:

- Поскольку клиенты имеют графический интерфейс - воспользуемся Центром Управления Системой (**ЦУС**):

![](https://sysahelper.ru/pluginfile.php/814/mod_page/content/2/image%20%286%29.png)

![](https://sysahelper.ru/pluginfile.php/814/mod_page/content/2/image%20%287%29.png)

![](https://sysahelper.ru/pluginfile.php/814/mod_page/content/2/image%20%288%29.png)

- - Создаём пользователя **sshuser**:

![](https://sysahelper.ru/pluginfile.php/814/mod_page/content/2/image%20%2810%29.png)

- Задаём пользователю **sshuser** пароль **P@ssw0rd**:
- - - Добавляем его в группу **wheel** - выставив чек-бокс **Входит в группу администраторов**

![](https://sysahelper.ru/pluginfile.php/814/mod_page/content/2/image%20%2811%29.png)

- Проверяем:

![](https://sysahelper.ru/pluginfile.php/814/mod_page/content/2/image%20%2812%29.png)

- Устанавливаем пакет **sudo**:
    - для доступа в сеть Интернет можно временно использовать публичный DNS (**echo 'nameserver 77.88.8.8' > /etc/resolv.conf**)

```
apt-get update && apt-get install -y sudo
```

- Добавляем следующую строку в файл в [/etc/sudoers](https://www.opennet.ru/man.shtml?topic=sudoers&category=5&russian=0) чтобы была возможность запуска **sudo** без дополнительной аутентификации:

```
echo "sshuser ALL=(ALL:ALL) NOPASSWD: ALL" >> /etc/sudoers
```

- Проверяем:

![](https://sysahelper.ru/pluginfile.php/814/mod_page/content/2/image%20%2813%29.png)

Аналогично для всез остальных устройств: ADMIN-DT, CLI-DT, ADMIN-HQ, CLI-HQ.