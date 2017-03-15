#Create sub interface or Secondary IP Address on Ubuntu/Debian

## Create subinterface on Ubuntu – Debian without restart network service

1. Check present ip address on eth0
```
root@ubuntu:~# ip addr
1: lo: <LOOPBACK,UP,LOWER_UP> mtu 65536 qdisc noqueue state UNKNOWN
link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00
inet 127.0.0.1/8 scope host lo
inet6 ::1/128 scope host
valid_lft forever preferred_lft forever
2: eth0: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc pfifo_fast state UNKNOWN qlen 1000
link/ether 00:0c:29:17:b1:f4 brd ff:ff:ff:ff:ff:ff
inet 192.168.0.106/24 brd 192.168.0.255 scope global eth0
inet6 fe80::20c:29ff:fe17:b1f4/64 scope link
valid_lft forever preferred_lft forever
root@ubuntu:~#
```

1. Configure second IP address on subinterface
```
root@ubuntu:~# ifconfig eth0:0 192.168.0.107 netmask 255.255.255.0
root@ubuntu:~# ip addr
1: lo: <LOOPBACK,UP,LOWER_UP> mtu 65536 qdisc noqueue state UNKNOWN
link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00
inet 127.0.0.1/8 scope host lo
inet6 ::1/128 scope host
valid_lft forever preferred_lft forever
2: eth0: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc pfifo_fast state UNKNOWN qlen 1000
link/ether 00:0c:29:17:b1:f4 brd ff:ff:ff:ff:ff:ff
inet 192.168.0.106/24 brd 192.168.0.255 scope global eth0
inet 192.168.0.107/24 brd 192.168.0.255 scope global secondary eth0:0
inet6 fe80::20c:29ff:fe17:b1f4/64 scope link
valid_lft forever preferred_lft forever
root@ubuntu:~#
```

1. Create subinterface on Ubuntu – Debian permanently. Add eth0:0 interface to /etc/network/interfaces
```
root@ubuntu:~# vi /etc/network/interfaces
# interfaces(5) file used by ifup(8) and ifdown(8)
auto lo
iface lo inet loopback
auto eth0
iface eth0 inet static
address 192.168.0.106
netmask 255.255.255.0
network 192.168.0.0
broadcast 192.168.0.255
gateway 192.168.0.1
auto eth0:0
iface eth0:0 inet static
address 192.168.0.107
netmask 255.255.255.0
root@ubuntu:~#
```

1. Restart networking service
```
root@ubuntu:~# /etc/init.d/networking restart
Rather than invoking init scripts through /etc/init.d, use the service(8)
utility, e.g. service networking restartSince the script you are attempting to invoke has been converted to an
Upstart job, you may also use the stop(8) and then start(8) utilities,
e.g. stop networking ; start networking. The restart(8) utility is also availabl e.
networking stop/waiting
networking start/running
root@ubuntu:~# ip addr
1: lo: <LOOPBACK,UP,LOWER_UP> mtu 65536 qdisc noqueue state UNKNOWN
link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00
inet 127.0.0.1/8 scope host lo
inet6 ::1/128 scope host
valid_lft forever preferred_lft forever
2: eth0: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc pfifo_fast state UNKNOWN qlen 1000
link/ether 00:0c:29:17:b1:f4 brd ff:ff:ff:ff:ff:ff
inet 192.168.0.106/24 brd 192.168.0.255 scope global eth0
inet 192.168.0.107/24 brd 192.168.0.255 scope global secondary eth0:0
inet6 fe80::20c:29ff:fe17:b1f4/64 scope link
valid_lft forever preferred_lft forever
```