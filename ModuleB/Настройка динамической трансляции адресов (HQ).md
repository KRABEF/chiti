### Задание:

a) Настройте на маршрутизаторах динамическую трансляцию адресов.

b) Все устройства во всех офисах должны иметь доступ к сети Интернет

### Вариант реализации:

### R-HQ:

- С точки зрения **EcoRouter** - реализуем конфигурацию **static source PAT:**
    - Интерсейс в сторону **ISP** с именем **isp** - назначаем как **nat outside**:

```
r-hq#configure terminal
r-hq(config)#interface isp
r-hq(config-if)#ip nat outside
r-hq(config-if)#exit
r-hq(config)#
```

- - Подинтерфейсы  **vl110, vl220, vl330,** которые смотрят в сторону **SW1-HQ** - назначаем как **nat inside**:

```
r-hq(config)#interface vl110
r-hq(config-if)#ip nat inside
r-hq(config-if)#exit
r-hq(config)#
r-hq(config)#interface vl220
r-hq(config-if)#ip nat inside
r-hq(config-if)#exit
r-hq(config)#
r-hq(config)#interface vl330
r-hq(config-if)#ip nat inside
r-hq(config-if)#exit
r-hq(config)#
```

- - создаём пула адресов с именем **LOCAL-HQ** для входящего трафика - указываем диапазон адресов из выделенной подсети:

```
r-hq(config)#ip nat pool LOCAL-HQ 192.168.11.1-192.168.11.254
```

- - задаём правила для трансляции адресов:

```
r-hq(config)#ip nat source dynamic inside pool LOCAL-HQ overload 172.16.5.14
r-hq(config)#write
r-hq(config)#
```

#### Для проверки:

- Проверяем:
    - т.к. на данном этапе ещё не настройена коммутация на **SW1-HQ** - проверить работоспособность **NAT** можно назначив средствами **iproute2** временно на интерфейс **SW1-HQ** на интерфейс,смотрящий в сторону **R-HQ** - тегированный подинтерфейс с IP-адресом из подсети для **vlan330:**

```
ip link add link ens19 name ens19.330 type vlan id 330
ip link set dev ens19.330 up
ip addr add 192.168.11.82/29 dev ens19.330
ip route add 0.0.0.0/0 via 192.168.11.81
```

- - затем проверяем доступ в сеть Интернет:

![](https://sysahelper.ru/pluginfile.php/821/mod_page/content/2/image.png)

- - после чего можно проверить таблицу трансляции адресов на **R-HQ**:

![](https://sysahelper.ru/pluginfile.php/821/mod_page/content/2/image%20%281%29.png)