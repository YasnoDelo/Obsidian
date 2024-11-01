In enable mode:
1) ```show hosts```
2) ```telnet name```
3) ```show ip int brief``` 

In conf_t mode:
1) ```interface fa1/0/7```
2) ```no shutdown```
3) ```show cdp neigh```

# Many Routers
#### R1 (usual)
```
conf t
int E2/0
ip add 192.168.0.1 255.255.255.0
no sh
```

#### R3 (+ telnet)
```
conf t
int E2/0
ip add 192.168.0.3 255.255.255.0
no sh

line vty 0 1869
login local
  
username usr secret pwd
username cisco secret cisco
enable secret cisco
```
 
### Настройка Telnet
```
line vty 0 1869
login local
  
username usr secret pwd
username cisco secret cisco
enable secret cisco
```

### Настройка VLAN
```
conf t
int fa0/0
no sh

int fa0/0.20
enc dot 20
ip add 192.168.20.1 255.255.255.0

int fa0/0.13
enc dot 13 nat
ip add 192.168.13.1 255.255.255.0
```

| Команда                             | Описание                                              |
| ----------------------------------- | ----------------------------------------------------- |
| `int fa0/0.13`                      | Переходим на виртуальный интерфейс                    |
| `enc dot 20`                        | Инкапсулируем в dot1q (т. е. превращаем в trunk)      |
| `enc dot 13 nat`                    | Инкапсулируем в dot1q и говорим, что native VLAN = 13 |
| `ip add 192.168.20.1 255.255.255.0` | Добавляем виртуальному интерфейсу ip-адресс           |
|                                     |                                                       |
### Настройка Роутера как хоста
```
conf t
int fa0/0
no sh

int fa0/0.20
ip add 192.168.20.2 255.255.255.0
ip route 0.0.0.0 0.0.0.0 192.168.20.1
```

### DHCP
```
conf t
int fa0/0
ip add 192.168.0.1 255.255.255.0
no sh

ip dhcp pool Name
netw 192.168.0.0 /24
default-router 192.168.0.1
lease N N N

ip dhcp excl 192.168.0.1 192.168.0.15

```

| Команда                                                                                                 | Описание                                                            |
| ------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------- |
| `ip dhcp pool` + Name                                                                                   | Инициализируем пул                                                  |
| `netw 192.168.0.0 /24`                                                                                  | Задаём доступные адресса                                            |
| `default-router 192.168.0.1`                                                                            | Дефолт гейтвей                                                      |
| `lease N N N`                                                                                           | Настройка времени аренды                                            |
| `ip dhcp excl 192.168.0.1 192.168.0.15`                                                                 | Исключить адреса                                                    |
| `ip route` + адресс сети + маска сети + default gateway + индекс приоритетности (меньше - приоритетнее) | Настроить default gateway                                           |
| `ip route 0.0.0.0 0.0.0.0 192.168.20.1`                                                                 | Настроить так, чтобы все запросы перенаправлялись на `192.168.20.1` |

### ACL basic (составляем таблицу)
```
ip access-list standart 1
permit/deny + sourse_IP

int + int_name
ip access-group 1 out
```

### ACL extended (составляем таблицу)
```
ip access-list extended 100
permit/deny + TEG + sourse_IP + destination_IP + wildcard
exit

int + int_name
ip access-group 1 out
```
	

