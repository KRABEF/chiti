### Задание:

c) Все устройства должны быть настроены на использование обоих внутренних DNS серверов.

- 1. Для офиса HQ основным DNS сервером является SRV1-HQ
- 2. Для офиса DT основным DNS сервером является SRV1-DT

### Вариант реализации:

### SRV2-DT | SRV3-DT | ADMIN-DT:

- Задаём настройки DNS:

```
cat <<EOF > /etc/net/ifaces/ens19/resolv.conf
  search au.team
  nameserver 192.168.33.66
  nameserver 192.168.11.66
EOF
```

- Перезагружаем службу **network**:

```
systemctl restart network
```

### SW1-HQ | SW2-HQ | SW3-HQ:

- Задаём настройки DNS:

```
cat <<EOF > /etc/net/ifaces/MGMT/resolv.conf
  search au.team
  nameserver 192.168.11.66
  nameserver 192.168.33.66
EOF
```

- Перезагружаем службу **network**:

```
systemctl restart network
```

### ADMIN-HQ:

- Задаём настройки DNS:

```
cat <<EOF > /etc/net/ifaces/ens19/resolv.conf
  search au.team
  nameserver 192.168.11.66
  nameserver 192.168.33.66
EOF
```

- Перезагружаем службу **network**:

```
systemctl restart network
```

### R-HQ:

- Задаём настройки DNS:

```
r-hq#configure terminal
r-hq(config)#ip name-server 192.168.11.66 192.168.33.66
r-hq(config)#ip domain-name au.team
r-hq(config)#write
r-hq(config)#
```

### R-DT:

- Задаём настройки DNS:

```
r-dt#configure terminal
r-dt(config)#ip name-server 192.168.33.66 192.168.11.66
r-dt(config)#ip domain-name au.team
r-dt(config)#write
r-dt(config)#
```