### Задание:

a) Настройте коммутаторы SW1-HQ, SW2-HQ, SW3-HQ.

- 1. Используйте Open vSwitch
- 2. Имя коммутатора должно совпадать с коротким именем устройства
    - i. Используйте заглавные буквы

1. Передайте все физические порты коммутатору.

2. Обеспечьте включение портов, если это необходимо

c) Для каждого офиса устройства должны находиться в соответствующих VLAN

- 1. Клиенты - vlan110,
- 2. Сервера – в vlan220,
- 3. Администраторы – в vlan330.

### Вариант реализации:

**SW1-HQ:**

- Поскольку на данном этапе ещё не настройена коммутация на **SW1-HQ**, а установить пакет **openvswitch** необходимо, то можно назначив средствами **iproute2** временно на интерфейс,смотрящий в сторону **R-HQ** - тегированный подинтерфейс с IP-адресом из подсети для **vlan330:**

```
ip link add link ens19 name ens19.330 type vlan id 330
ip link set dev ens19.330 up
ip addr add 192.168.11.82/29 dev ens19.330
ip route add 0.0.0.0/0 via 192.168.11.81
echo "nameserver 77.88.8.8" > /etc/resolv.conf
```

- Обновляем список пакетов и устанавливаем **openvswitch**:

```
apt-get update && apt-get install -y openvswitch
```

- Включаем и добавляем в автозагрузку **openvswitch**:

```
systemctl enable --now openvswitch
```

- Перезагрузить сервер будет быстрее чем удалять параметры заданые в ручную через пакет **iproute2**:

```
reboot
```

- Проверяем интерфейсы и определяемся какой к кому направлен:
    - таким образом, имеем:
        - **ens19** - интерфейс в сторону **R-HQ**;
        - **ens20** - интерфейс в сторону **SW2-HQ**;
        - **ens21** - интерфейс в сторону **SW3-HQ**.

![](https://sysahelper.ru/pluginfile.php/815/mod_page/content/4/image.png)

- Обеспечим включение портов **ens20** и **ens21**:
    - Создадим для них одноимённые директории по пути **/etc/net/ifaces/<ИМЯ_ИНТЕРФЕЙСА>**:

```
mkdir /etc/net/ifaces/ens2{0,1}
```

- - Для каждого интерфейса необходимо создать конфигурационный файл **options**:
        - данный файл должен включать в себя минимально необходимое содержимое, а именно **два** параметра: **TYPE** и **BOOTPROTO**
        - создаём данный файл для интерфейса **ens20**:

```
cat <<EOF > /etc/net/ifaces/ens20/options
  TYPE=eth
  BOOTPROTO=static
EOF
```

- - - **P.S.** или же открываем через текстовы редактор, например: **vim**
    - Файл **options** для интерфейса **ens21** будет аналогичен как и для **ens20** - поэтому его можно просто скопировать:

```
cp /etc/net/ifaces/ens20/options /etc/net/ifaces/ens21/
```

- - Также важно, чтобы и для **ens19** в файле **options** параметр **BOOTPROTO** имел значение **static**:

```
sed -i "s/BOOTPROTO=dhcp/BOOTPROTO=static/g" /etc/net/ifaces/ens19/options
```

- - Перезагружаем службу **network** для применения изменений:

```
systemctl restart network
```

- - Проверяем что все интерфейсы перешли в состояние **UP**:

![](https://sysahelper.ru/pluginfile.php/815/mod_page/content/4/image%20%281%29.png)

- Создадим коммутатор имя которого должно совпадать с коротким именем устройства и с использованием заглавных букв - **SW1-HQ**:

```
ovs-vsctl add-br SW1-HQ
```

- - Проверяем:

![](https://sysahelper.ru/pluginfile.php/815/mod_page/content/4/image%20%282%29.png)

- Сетевая подсистема **etcnet** будет взаимодействовать с **openvswitch**, для того чтобы корректно можно было назначить  IP-адрес на  интерфейс управления  
    - Создаём каталог для management интерфейса с именем **MGMT**:

```
mkdir /etc/net/ifaces/MGMT
```

- - Описываем файл **options** для создания management интерфейса с именем **mgmt**:

```
vim /etc/net/ifaces/MGMT/options
```

- - Содержимое, где:
        - **TYPE** - тип интерфейса (**internal**);
        - **BOOTPROTO** - определяет как будут назначаться сетевые параметры (статически);
        - **CONFIG_IPV4** - определяет использовать конфигурацию протокола IPv4 или нет;
        - **BRIDGE** - определяет к какому мосту необходимо добавить данный интерфейс;
        - **VID** - определяет принадлежность интерфейса к VLAN;

![](https://sysahelper.ru/pluginfile.php/815/mod_page/content/4/image%20%283%29.png)

- - Назначаем IP-адрес и шлюз на созданный интерфейс **MGMT** согласно таблице адресации для Администраторской подсети:

```
echo "192.168.11.82/29" > /etc/net/ifaces/MGMT/ipv4address
```

```
echo "default via 192.168.11.81" > /etc/net/ifaces/MGMT/ipv4route
```

- - Правим основной файл **options** в котором по умолчанию сказано удалять настройки заданые через **ovs-vsctl,** т.к. через **etcnet** будет выполнено только создание **bridge** и интерфейса типа **internal** с назначением необходимого IP-адреса, а настройка функционала будет выполнена средствами **openvswitch**:

```
sed -i "s/OVS_REMOVE=yes/OVS_REMOVE=no/g" /etc/net/ifaces/default/options
```

- Перезапускаем службу **network**:

```
systemctl restart network
```

- Проверяем:
    - На текущей момент создан интерфейс управления и назначем IP-адрес из соответствующей подсети
    - Данный интерфейс управления помечен тегом (VID) 330 и добавлен в bridge SW1-HQ

![](https://sysahelper.ru/pluginfile.php/815/mod_page/content/4/image%20%284%29.png)

- Средствами **openvswitch** настраиваем следующий функционал:
    - Порт в сторону маршрутизатора и в сторону остальных коммутаторов должны быть магистральными и пропускать использующиеся VLAN-ы:

```
ovs-vsctl add-port SW1-HQ ens19 trunk=110,220,330
```

```
ovs-vsctl add-port SW1-HQ ens20 trunk=110,220,330
```

```
ovs-vsctl add-port SW1-HQ ens21 trunk=110,220,330
```

- - Включаем модуль ядра **8021q:**

```
modprobe 8021q
```

- - При необходимости добавляем и на постоянной основе:

```
echo "8021q" | tee -a /etc/modules
```

- Проверяем:
    - наличие портов в коммутаторе
    - включённый модуль
    - доступ в сеть Интернет

![](https://sysahelper.ru/pluginfile.php/815/mod_page/content/4/image%20%285%29.png)

**SW2-HQ:**

- Поскольку на данном этапе ещё не настройена коммутация на **SW2-HQ**, а установить пакет **openvswitch** необходимо, то можно назначив средствами **iproute2** временно на интерфейс смотрящий в сторону **SW1-HQ** тегированный подинтерфейс с IP-адресом из подсети для **vlan330:**

```
ip link add link ens19 name ens19.330 type vlan id 330
ip link set dev ens19.330 up
ip addr add 192.168.11.83/29 dev ens19.330
ip route add 0.0.0.0/0 via 192.168.11.81
echo "nameserver 77.88.8.8" > /etc/resolv.conf
```

- Обновляем список пакетов и устанавливаем **openvswitch**:

```
apt-get update && apt-get install -y openvswitch
```

- Включаем и добавляем в автозагрузку **openvswitch**:

```
systemctl enable --now openvswitch
```

Перезагрузить сервер будет быстрее чем удалять параметры заданые в ручную через пакет **iproute2**:

```
reboot
```

- Проверяем интерфейсы и определяемся какой к кому направлен:
    - Таким образом, имеем:
        - **ens19** - интерфейс в сторону **SW1-HQ**
        - **ens20** - интерфейс в сторону **SW3-HQ**
        - **ens21** - интерфейс в сторону **SRV1-HQ**
        - **ens22** - интерфейс в сторону **CLI-HQ**

**![](https://sysahelper.ru/pluginfile.php/815/mod_page/content/4/image%20%287%29.png)**

- Обеспечим включение портов **ens20,** **ens21** и **ens22**:
    - Создадим для них одноимённые директории по пути **/etc/net/ifaces/<ИМЯ_ИНТЕРФЕЙСА>**:

```
mkdir /etc/net/ifaces/ens2{0..2}
```

- - Для каждого интерфейса необходимо создать конфигурационный файл **options**:
        - Данный файл должен включать в себя минимально необходимое содержимое, а именно **два** параметра: **TYPE** и **BOOTPROTO**
        - Создаём данный файл для интерфейса **ens20**:

```
cat <<EOF > /etc/net/ifaces/ens20/options
  TYPE=eth
  BOOTPROTO=static
EOF
```

- - - **P.S.** или же открываем через текстовы редактор, например: **vim**
    - Файл **options** для интерфейса **ens21** и **ens22** будет аналогичен как и для **ens20**, поэтому его можно просто скопировать:

```
cp /etc/net/ifaces/ens20/options /etc/net/ifaces/ens21/
```

```
cp /etc/net/ifaces/ens20/options /etc/net/ifaces/ens22/
```

- - Также важно, чтобы для **ens19** в файле **options** параметр **BOOTPROTO** имел значение **static**:

```
sed -i "s/BOOTPROTO=dhcp/BOOTPROTO=static/g" /etc/net/ifaces/ens19/options
```

- - Перезагружаем службу **network** для применения изменений:

```
systemctl restart network
```

- - Проверяем что все интерфейсы перешли в состояние **UP**:

![](https://sysahelper.ru/pluginfile.php/815/mod_page/content/4/image%20%288%29.png)

- Создадим коммутатор имя которого должно совпадать с коротким именем устройства с использованием заглавных букв - **SW2-HQ**:

```
ovs-vsctl add-br SW2-HQ
```

- - Проверяем:

![](https://sysahelper.ru/pluginfile.php/815/mod_page/content/4/image%20%289%29.png)

- Сетевая подсистема **etcnet** будет взаимодействовать с **openvswitch**, для того чтобы корректно можно было назначить  IP-адрес на  интерфейс управления  
    - Создаём каталог для management интерфейса с именем **MGMT**:

```
mkdir /etc/net/ifaces/MGMT
```

- - Описываем файл **options** для создания management интерфейса с именем **mgmt**:

```
vim /etc/net/ifaces/MGMT/options
```

- - Содержимое, где:
        - **TYPE** - тип интерфейса (**internal**);
        - **BOOTPROTO** - определяет как будут назначаться сетевые параметры (статически);
        - **CONFIG_IPV4** - определяет использовать конфигурацию протокола IPv4 или нет;
        - **BRIDGE** - определяет к какому мосту необходимо добавить данный интерфейс;
        - **VID** - определяет принадлежность интерфейса к VLAN;

![](https://sysahelper.ru/pluginfile.php/815/mod_page/content/4/image%20%2810%29.png)

- - Назначаем IP-адрес и шлюз на созданный интерфейс **MGMT** согласно таблеце адресации для Администраторской подсети:

```
echo "192.168.11.83/29" > /etc/net/ifaces/MGMT/ipv4address
```

```
echo "default via 192.168.11.81" > /etc/net/ifaces/MGMT/ipv4route
```

- - Правим основной файл **options** в котором по умолчанию сказано удалять настройки заданые через **ovs-vsctl,** т.к. через **etcnet** будет выполнено только создание **bridge** и интерфейса типа **internal** с назначением необходимого IP-адреса, а настройка функционала будет выполнена средствами **openvswitch**:

```
sed -i "s/OVS_REMOVE=yes/OVS_REMOVE=no/g" /etc/net/ifaces/default/options
```

- Перезапускаем службу **network**:

```
systemctl restart network
```

- Проверяем:
    - На текущей момент создан интерфейс управления и назначем IP-адрес из соответствующей подсети
    - Данный интерфейс управления помечен тегом (VID) 330 и добавлен в bridge SW1-HQ

![](https://sysahelper.ru/pluginfile.php/815/mod_page/content/4/image%20%2811%29.png)

- Средствами **openvswitch** настраиваем следующий функционал:
    - Порты в сторону коммутаторов (**ens19, ens20**) должны быть магистральными и пропускать использующиеся VLAN-ы:

```
ovs-vsctl add-port SW2-HQ ens19 trunk=110,220,330
```

```
ovs-vsctl add-port SW2-HQ ens20 trunk=110,220,330
```

- - Порт в сторону **SRV1-HQ (ens21)** должен быть портом доступа и принадлежать VLAN **220** (Сервера):

```
ovs-vsctl add-port SW2-HQ ens21 tag=220
```

- - Порт в сторону **CLI-HQ (ens22)** должен быть портом доступа и принадлежать VLAN **110** (Клиенты):

```
ovs-vsctl add-port SW2-HQ ens22 tag=110
```

- - Включаем модуль ядра **8021q:**

```
modprobe 8021q
```

- - При необходимости добавляем и на постоянной основе:

```
echo "8021q" | tee -a /etc/modules
```

- Проверяем:
    - наличие портов в коммутаторе
    - включённый модуль
    - доступ в сеть Интернет

![](https://sysahelper.ru/pluginfile.php/815/mod_page/content/4/image%20%2812%29.png)

**SW3-HQ:**

- Поскольку на данном этапе ещё не настройена коммутация на **SW3-HQ**, а установить пакет **openvswitch** необходимо, то можно назначив средствами **iproute2** временно на интерфейс,смотрящий в сторону **SW1-HQ** тегированный подинтерфейс с IP-адресом из подсети для **vlan330:**

```
ip link add link ens19 name ens19.330 type vlan id 330
ip link set dev ens19.330 up
ip addr add 192.168.11.84/29 dev ens19.330
ip route add 0.0.0.0/0 via 192.168.11.81
echo "nameserver 77.88.8.8" > /etc/resolv.conf
```

- Обновляем список пакетов и устанавливаем **openvswitch**:

```
apt-get update && apt-get install -y openvswitch
```

- Включаем и добавляем в автозагрузку **openvswitch**:

```
systemctl enable --now openvswitch
```

- Перезагрузить сервер будет быстрее чем удалять параметры заданые в ручную через пакет **iproute2**:

```
reboot
```

- Проверяем интерфейсы и определяемся какой к кому направлен:
    - Таким образом, имеем:
        - **ens19** - интерфейс в сторону **SW1-HQ**
        - **ens20** - интерфейс в сторону **SW2-HQ**
        - **ens21** - интерфейс в сторону **ADMIN-HQ**

**![](https://sysahelper.ru/pluginfile.php/815/mod_page/content/4/image%20%2814%29.png)  
**

- Обеспечим включение портов **ens20** и **ens21**:
    - Создадим для них одноимённые директории по пути **/etc/net/ifaces/<ИМЯ_ИНТЕРФЕЙСА>**:

```
mkdir /etc/net/ifaces/ens2{0,1}
```

- - Для каждого интерфейса необходимо создать конфигурационный файл **options**:
        - Данный файл должен включать в себя минимально необходимое содержимое, а именно **два** параметра: **TYPE** и **BOOTPROTO**
        - Создаём данный файл для интерфейса **ens20**:

```
cat <<EOF > /etc/net/ifaces/ens20/options
  TYPE=eth
  BOOTPROTO=static
EOF
```

- - - **P.S.** или же открываем через текстовы редактор, например: **vim**
    - Файл **options** для интерфейса **ens21** будет аналогичен как и для **ens20** - поэтому его можно просто скопировать:

```
cp /etc/net/ifaces/ens20/options /etc/net/ifaces/ens21/
```

- - Также важно, чтобы и для **ens19** в файле **options** параметр **BOOTPROTO** имел значение **static**:

```
sed -i "s/BOOTPROTO=dhcp/BOOTPROTO=static/g" /etc/net/ifaces/ens19/options
```

- - Перезагружаем службу **network** для применения изменений:

```
systemctl restart network
```

- - Проверяем что все интерфейсы перешли в состояние **UP**:

![](https://sysahelper.ru/pluginfile.php/815/mod_page/content/4/image%20%2815%29.png)

- Создадим коммутатор имя которого должно совпадать с коротким именем устройства с использованием заглавных букв - **SW3-HQ**:

```
ovs-vsctl add-br SW3-HQ
```

- - Проверяем:

![](https://sysahelper.ru/pluginfile.php/815/mod_page/content/4/image%20%2816%29.png)

- Сетевая подсистема **etcnet** будет взаимодействовать с **openvswitch**, для того чтобы корректно можно было назначить  IP-адрес на  интерфейс управления  
    - Создаём каталог для management интерфейса с именем **MGMT**:

```
mkdir /etc/net/ifaces/MGMT
```

- - Описываем файл **options** для создания management интерфейса с именем **mgmt**:

```
vim /etc/net/ifaces/MGMT/options
```

- - Содержимое, где:
        - **TYPE** - тип интерфейса (**internal**);
        - **BOOTPROTO** - определяет как будут назначаться сетевые параметры (статически);
        - **CONFIG_IPV4** - определяет использовать конфигурацию протокола IPv4 или нет;
        - **BRIDGE** - определяет к какому мосту необходимо добавить данный интерфейс;
        - **VID** - определяет принадлежность интерфейса к VLAN;

![](https://sysahelper.ru/pluginfile.php/815/mod_page/content/4/image%20%2817%29.png)

- - Назначаем IP-адрес и шлюз на созданный интерфейс **MGMT** согласно таблице адресации для Администраторской подсети:

```
echo "192.168.11.84/29" > /etc/net/ifaces/MGMT/ipv4address
```

```
echo "default via 192.168.11.81" > /etc/net/ifaces/MGMT/ipv4route
```

- - Правим основной файл **options** в котором по умолчанию сказано удалять настройки заданые через **ovs-vsctl,** т.к. через **etcnet** будет выполнено только создание **bridge** и интерфейса типа **internal** с назначением необходимого IP-адреса, а настройка функционала будет выполнена средствами **openvswitch**:

```
sed -i "s/OVS_REMOVE=yes/OVS_REMOVE=no/g" /etc/net/ifaces/default/options
```

- Перезапускаем службу **network**:

```
systemctl restart network
```

- Проверяем:
    - На текущей момент создан интерфейс управления и назначем IP-адрес из соответствующей подсети
    - Данный интерфейс управления помечен тегом (VID) 330 и добавлен в bridge SW1-HQ

![](https://sysahelper.ru/pluginfile.php/815/mod_page/content/4/image%20%2818%29.png)

- Средствами **openvswitch** настраиваем следующий функционал:
    - Порты в сторону коммутаторов (**ens19, ens20**) должны быть магистральными и пропускать использующиеся VLAN-ы:

```
ovs-vsctl add-port SW3-HQ ens19 trunk=110,220,330
```

```
ovs-vsctl add-port SW3-HQ ens20 trunk=110,220,330
```

- - Порт в сторону **ADMIN-HQ (ens21)** должен быть портом доступа и принадлежать VLAN - **330** (Администраторы):

```
ovs-vsctl add-port SW3-HQ ens21 tag=330
```

- - Включаем модуль ядра **8021q:**

```
modprobe 8021q
```

- - При необходимости добавляем и на постоянной основе:

```
echo "8021q" | tee -a /etc/modules
```### Задание:

1. Настройте протокол основного дерева

- i. Корнем дерева должен выступать SW1-HQ

### Вариант реализации:

**SW1-HQ:**

Настроим Bridge **SW1-HQ** на участие в дереве 802.1D: 

```
ovs-vsctl set bridge SW1-HQ stp_enable=true
```

- - Задаём приоритет для Bridge **SW1-HQ** в **16384**, т.к. по условиям задания он должен быть корневым:

```
ovs−vsctl set bridge SW1-HQ other_config:stp-priority=16384
```

- - Проверяем:

![](https://sysahelper.ru/pluginfile.php/816/mod_page/content/2/image.png)

**SW2-HQ:**

- Настроим Bridge **SW2-HQ** на участие в дереве 802.1D: 

```
ovs-vsctl set bridge SW2-HQ stp_enable=true
```

- - Задаём приоритет для Bridge **SW2-HQ** в **24576**, т.к. по условиям задания **SW1-HQ** должен быть корневым:

```
ovs−vsctl set bridge SW2-HQ other_config:stp-priority=24576
```

- - Проверяем:

![](https://sysahelper.ru/pluginfile.php/816/mod_page/content/2/image%20%281%29.png)

**SW3-HQ:**

- Настроим Bridge **SW3-HQ** на участие в дереве 802.1D: 

```
ovs-vsctl set bridge SW3-HQ stp_enable=true
```

- - Задаём приоритет для Bridge **SW3-HQ** в **28672**, т.к. по условиям задания **SW1-HQ** должен быть корневым:

```
ovs−vsctl set bridge SW3-HQ other_config:stp-priority=28672
```

- - Проверяем:

![](https://sysahelper.ru/pluginfile.php/816/mod_page/content/2/image%20%282%29.png)