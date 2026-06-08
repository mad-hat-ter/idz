# Итоговое домашнее задание «Основы виртуализации и контейнеризации»

## Запуск проекта

Запуск: docker compose up -d

Проверка работы: docker compose ps

## VM
- **ОС:** Ubuntu Server 25.04 LTS
- **CPU:** 4 vCPU
- **RAM:** 4 ГБ
- **Диск:** 25 ГБ
- **Сеть:** Сетевой мост
- **Пользователь:** `seconduser` с правами `sudo`
- **Firewall:** UFW разрешает SSH и порт 80

## Информация о VM

1. hostnamectl

```
 Static hostname: vboxuser
       Icon name: computer-vm
         Chassis: vm 🖴
      Machine ID: 26b4bd3c87c34403882c3bff8a020035
         Boot ID: 029d962d16c24ee394cfa6a426464266
  Virtualization: oracle
Operating System: Ubuntu 26.04 LTS                
          Kernel: Linux 7.0.0-22-generic
    Architecture: x86-64
 Hardware Vendor: innotek GmbH
  Hardware Model: VirtualBox
Hardware Version: 1.2
Firmware Version: VirtualBox
   Firmware Date: Fri 2006-12-01
    Firmware Age: 19y 6month 6d                   
```

2. ip a

```
1: lo: <LOOPBACK,UP,LOWER_UP> mtu 65536 qdisc noqueue state UNKNOWN group default qlen 1000
    link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00
    inet 127.0.0.1/8 scope host lo
       valid_lft forever preferred_lft forever
    inet6 ::1/128 scope host noprefixroute 
       valid_lft forever preferred_lft forever
2: enp0s3: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc fq_codel state UP group default qlen 1000
    link/ether 08:00:27:18:03:21 brd ff:ff:ff:ff:ff:ff
    altname enx080027180321
    inet 192.168.0.135/24 brd 192.168.0.255 scope global dynamic noprefixroute enp0s3
       valid_lft 85615sec preferred_lft 85615sec
    inet6 fe80::a00:27ff:fe18:321/64 scope link noprefixroute 
       valid_lft forever preferred_lft forever
3: docker0: <NO-CARRIER,BROADCAST,MULTICAST,UP> mtu 1500 qdisc noqueue state DOWN group default 
    link/ether ca:61:5e:b9:3a:d2 brd ff:ff:ff:ff:ff:ff
    inet 172.17.0.1/16 brd 172.17.255.255 scope global docker0
       valid_lft forever preferred_lft forever
11: br-8b166f3c8f08: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc noqueue state UP group default 
    link/ether d6:f4:be:9a:1c:d4 brd ff:ff:ff:ff:ff:ff
    inet 172.18.0.1/16 brd 172.18.255.255 scope global br-8b166f3c8f08
       valid_lft forever preferred_lft forever
    inet6 fe80::d4f4:beff:fe9a:1cd4/64 scope link proto kernel_ll 
       valid_lft forever preferred_lft forever
12: vethd5f96b9@if2: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc noqueue master br-8b166f3c8f08 state UP group default 
    link/ether 4a:19:d5:ce:2a:c6 brd ff:ff:ff:ff:ff:ff link-netnsid 0
    inet6 fe80::4819:d5ff:fece:2ac6/64 scope link proto kernel_ll 
       valid_lft forever preferred_lft forever
13: vethe0bfda3@if2: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc noqueue master br-8b166f3c8f08 state UP group default 
    link/ether 0e:cd:e7:48:1a:b8 brd ff:ff:ff:ff:ff:ff link-netnsid 1
    inet6 fe80::ccd:e7ff:fe48:1ab8/64 scope link proto kernel_ll 
       valid_lft forever preferred_lft forever
14: veth35b1a86@if2: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc noqueue master br-8b166f3c8f08 state UP group default 
    link/ether ce:d4:2d:72:6c:8d brd ff:ff:ff:ff:ff:ff link-netnsid 2
    inet6 fe80::ccd4:2dff:fe72:6c8d/64 scope link proto kernel_ll 
       valid_lft forever preferred_lft forever
```

3. docker version

```
Client: Docker Engine - Communit
 Version:           29.5.3
 API version:       1.54
 Go version:        go1.26.4
 Git commit:        d1c06ef
 Built:             Wed Jun  3 18:00:10 2026
 OS/Arch:           linux/amd64
 Context:           default

Server: Docker Engine - Community
 Engine:
  Version:          29.5.3
  API version:      1.54 (minimum version 1.40)
  Go version:       go1.26.4
  Git commit:       285b471
  Built:            Wed Jun  3 18:00:10 2026
  OS/Arch:          linux/amd64
  Experimental:     false
 containerd:
  Version:          v2.2.4
  GitCommit:        193637f7ee8ae5f5aa5248f49e7baa3e6164966e
 runc:
  Version:          1.3.5
  GitCommit:        v1.3.5-0-g488fc13e
 docker-init:
  Version:          0.19.0
  GitCommit:        de40ad0
```

4. docker compose version

Docker Compose version v5.1.4

5. docker compose ps

```
NAME          IMAGE                COMMAND                  SERVICE   CREATED         STATUS                   PORTS
idz-app-1     traefik/whoami       "/whoami"                app       6 minutes ago   Up 6 minutes             80/tcp
idz-db-1      postgres:15-alpine   "docker-entrypoint.s…"   db        6 minutes ago   Up 6 minutes (healthy)   5432/tcp
idz-proxy-1   nginx:alpine         "/docker-entrypoint.…"   proxy     6 minutes ago   Up 6 minutes             0.0.0.0:80->80/tcp, [::]:80->80/tcp
```

6. curl -I http://localhost/
   
```
HTTP/1.1 200 OK
Server: nginx/1.31.1
Date: Sun, 07 Jun 2026 22:15:16 GMT
Content-Type: text/plain; charset=utf-8
Content-Length: 157
Connection: keep-alive
```

7. curl http://192.168.0.135/

```
Hostname: cb86965583e3
IP: 127.0.0.1
IP: ::1
IP: 172.18.0.3
RemoteAddr: 172.18.0.4:55380
GET / HTTP/1.1
Host: app
User-Agent: curl/8.18.0
Accept: */*
```
