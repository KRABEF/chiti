### Задание:

a) Настройте на маршрутизаторах динамическую трансляцию адресов.

b) Все устройства во всех офисах должны иметь доступ к сети Интернет

### Вариант реализации:

### R-DT:

- С точки зрения **EcoRouter** - реализуем конфигурацию **static source PAT:**
    - интерсейс в сторону **ISP** с именем **int** - назначаем как **nat outside**:

```
r-dt#configure terminal
r-dt(config)#interface isp
r-dt(config-if)#ip nat outside
r-dt(config-if)#exit
r-dt(config)#
```

- - Интерфейс **int1** в сторону **FW-DT** - назначаем как **nat inside**:

```
r-dt(config)#interface int1
r-dt(config-if)#ip nat inside
r-dt(config-if)#exit
r-dt(config)#
```

- - Создаём пула адресов с именем **LOCAL-DT** для входящего трафика - указываем диапазон адресов из выделенной подсети:

```
r-dt(config)#ip nat pool LOCAL-DT 192.168.33.1-192.168.33.254
```

- - Задаём правила для трансляции адресов:

```
r-dt(config)#ip nat source dynamic inside pool LOCAL-DT overload 172.16.4.14
r-dt(config)#write
r-dt(config)#
```