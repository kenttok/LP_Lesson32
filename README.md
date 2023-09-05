## Статическая и динамическая маршрутизация, OSPF

Vagrantfile - вагрант файл

## Логи
### Лог ассиметричного роутинга

Подключаемся к router1. Запускаем ping до router2
```
root@router1:~# traceroute 192.168.20.1
traceroute to 192.168.20.1 (192.168.20.1), 30 hops max, 60 byte packets
 1  10.0.12.2 (10.0.12.2)  0.578 ms  0.807 ms  0.670 ms
 2  192.168.20.1 (192.168.20.1)  1.015 ms  1.192 ms  0.873 ms
root@router1:~# ping -I 192.168.10.1 192.168.20.1
PING 192.168.20.1 (192.168.20.1) from 192.168.10.1 : 56(84) bytes of data.
64 bytes from 192.168.20.1: icmp_seq=1 ttl=64 time=1.20 ms
...
```
Подключаемся к router2 и запускаем tcpdump на нужных интерфейсах. И видим что получаем пакеты мы с одного интерфейса а отправляем через другой.
```
vagrant@router2:~$ sudo tcpdump -i enp0s9
tcpdump: verbose output suppressed, use -v or -vv for full protocol decode
listening on enp0s9, link-type EN10MB (Ethernet), capture size 262144 bytes
07:08:41.339038 IP 192.168.10.1 > router2: ICMP echo request, id 3, seq 16, length 64
07:08:42.340398 IP 192.168.10.1 > router2: ICMP echo request, id 3, seq 17, length 64


vagrant@router2:~$ sudo tcpdump -i enp0s8
tcpdump: verbose output suppressed, use -v or -vv for full protocol decode
listening on enp0s8, link-type EN10MB (Ethernet), capture size 262144 bytes
07:09:17.842664 IP router2 > 192.168.10.1: ICMP echo reply, id 4, seq 4, length 64
07:09:18.843572 IP router2 > 192.168.10.1: ICMP echo reply, id 4, seq 5, length 64
```

### Лог восстановление симетричного роутинга
Изменяем cost на R2 enp0s8.  
Подключаемся к router2 и снимаем дамп
```
rvagrant@router2:~$ sudo tcpdump -i enp0s9
tcpdump: verbose output suppressed, use -v or -vv for full protocol decode
listening on enp0s9, link-type EN10MB (Ethernet), capture size 262144 bytes
07:10:23.664164 IP 192.168.10.1 > router2: ICMP echo request, id 5, seq 14, length 64
07:10:23.664188 IP router2 > 192.168.10.1: ICMP echo reply, id 5, seq 14, length 64
07:10:24.665999 IP 192.168.10.1 > router2: ICMP echo request, id 5, seq 15, length 64

```
