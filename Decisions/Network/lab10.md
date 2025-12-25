# Лабораторная работа 10

## Задание 1

1. Выведите список интерфейсов, какими способами можно это сделать?

    - Способ 1:
        ```bash
        [elizabeth@host-15 ~]$ ip addr show
        1: lo: <LOOPBACK,UP,LOWER_UP> mtu 65536 qdisc noqueue state UNKNOWN group default qlen 1000
            link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00
            inet 127.0.0.1/8 scope host lo
            valid_lft forever preferred_lft forever
            inet6 ::1/128 scope host noprefixroute 
            valid_lft forever preferred_lft forever
        2: enp0s3: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc fq_codel state UP group default qlen 1000
            link/ether 08:00:27:99:6e:bd brd ff:ff:ff:ff:ff:ff
            inet 10.0.2.15/24 brd 10.0.2.255 scope global dynamic noprefixroute enp0s3
            valid_lft 77801sec preferred_lft 77801sec
            inet6 fd17:625c:f037:2:a00:27ff:fe99:6ebd/64 scope global dynamic mngtmpaddr proto kernel_ra 
            valid_lft 86398sec preferred_lft 14398sec
            inet6 fe80::a00:27ff:fe99:6ebd/64 scope link proto kernel_ll 
            valid_lft forever preferred_lft forever
        ```

    - Способ 2:

        ```bash
        ip link show
        ```

    - Способ 3: ifconfig (если установлен)

        ```bash
        ifconfig -a
        ```

    - Способ 4: /proc

        ```bash
        cat /proc/net/dev
        ```

    - Способ 5: ls с сетевыми устройствами

        ```bash
        ls /sys/class/net/
        ```

    - Способ 6: nmcli (если NetworkManager установлен)

        ```bash
        nmcli device status
        ```

2. Попробуйте изменить ip адрес

    ```bash
    [elizabeth@host-15 ~]$ sudo ip addr del 10.0.2.15/24 dev enp0s3
    [sudo] password for elizabeth:
    [elizabeth@host-15 ~]$ ip addr show enp0s3
    2: enp0s3: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc fq_codel state UP group default qlen 1000
        link/ether 08:00:27:99:6e:bd brd ff:ff:ff:ff:ff:ff
        inet6 fd17:625c:f037:2:a00:27ff:fe99:6ebd/64 scope global dynamic mngtmpaddr proto kernel_ra 
        valid_lft 86283sec preferred_lft 14283sec
        inet6 fe80::a00:27ff:fe99:6ebd/64 scope link proto kernel_ll 
        valid_lft forever preferred_lft forever
    [elizabeth@host-15 ~]$ sudo ip addr add 10.0.2.15/24 dev enp0s3
    [elizabeth@host-15 ~]$ ip addr show enp0s3
    2: enp0s3: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc fq_codel state UP group default qlen 1000
        link/ether 08:00:27:99:6e:bd brd ff:ff:ff:ff:ff:ff
        inet 10.0.2.15/24 scope global dynamic enp0s3
        valid_lft 77036sec preferred_lft 77036sec
        inet6 fd17:625c:f037:2:a00:27ff:fe99:6ebd/64 scope global dynamic mngtmpaddr proto kernel_ra 
        valid_lft 86229sec preferred_lft 14229sec
        inet6 fe80::a00:27ff:fe99:6ebd/64 scope link proto kernel_ll 
        valid_lft forever preferred_lft forever
    ```

3. Попробуйте добавить несколько ip адресов на сетевую карту

    ```bash
    [elizabeth@host-15 ~]$ sudo ip addr add 10.0.2.16/24 dev enp0s3 label enp0s3:1
    [elizabeth@host-15 ~]$ sudo ip addr add 10.0.2.17/24 dev enp0s3 label enp0s3:2
    [elizabeth@host-15 ~]$ ip addr show enp0s3
    2: enp0s3: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc fq_codel state UP group default qlen 1000
        link/ether 08:00:27:99:6e:bd brd ff:ff:ff:ff:ff:ff
        inet 10.0.2.15/24 scope global dynamic enp0s3
        valid_lft 76533sec preferred_lft 76533sec
        inet 10.0.2.16/24 scope global secondary enp0s3:1
        valid_lft forever preferred_lft forever
        inet 10.0.2.17/24 scope global secondary enp0s3:2
        valid_lft forever preferred_lft forever
        inet6 fd17:625c:f037:2:a00:27ff:fe99:6ebd/64 scope global dynamic mngtmpaddr proto kernel_ra 
        valid_lft 86213sec preferred_lft 14213sec
        inet6 fe80::a00:27ff:fe99:6ebd/64 scope link proto kernel_ll 
        valid_lft forever preferred_lft forever
    ```

4. Выведите список маршрутов

    ```bash
    [elizabeth@host-15 ~]$ ip route show
    default via 10.0.2.2 dev enp0s3 proto dhcp src 10.0.2.15 metric 100 
    10.0.2.0/24 dev enp0s3 proto kernel scope link src 10.0.2.15 
    10.0.2.0/24 dev enp0s3 proto kernel scope link src 10.0.2.15 metric 100 
    ```

5. Выведите arp таблицу

    ```bash
    [elizabeth@host-15 ~]$ ip neigh show
    10.0.2.2 dev enp0s3 lladdr 52:55:0a:00:02:02 STALE 
    fd17:625c:f037:2::2 dev enp0s3 lladdr 52:56:00:00:00:02 router STALE 
    fe80::2 dev enp0s3 lladdr 52:56:00:00:00:02 router STALE 
    ```

6. Что такое ip адрес?

    IP-адрес — это уникальный числовой идентификатор, присваиваемый каждому устройству в компьютерной сети и определяемый протоколом IP (Internetwork Protocol). Он состоит из четырёх байтов, записываемых традиционно в десятичной системе счисления и разделяемых точкой. Существуют IPv4 (32-битные, например 192.168.1.1) и IPv6 (128-битные) адреса.

7. Для чего нужны маршруты?

    Маршруты определяют путь, по которому сетевые пакеты будут передаваться от одного устройства к другому в сети. Они указывают:
    - Как достичь определенной сети или хоста
    - Через какой интерфейс отправлять пакеты
    - Через какой шлюз (gateway) передавать трафик

8. Что за протокол arp?

    ARP (Address Resolution Protocol) — протокол для определения MAC-адреса по известному IP-адресу в локальной сети.

9. Что такое dhcp?

    DHCP (Dynamic Host Configuration Protocol) — протокол автоматической настройки сетевых параметров: IP-адрес, маска подсети, шлюз по умолчанию, DNS-серверы и др.

10. Что такое dns?

    Система доменных имен (DNS) является одной из фундаментальных технологий современной интернет-среды и представляет собой распределенную систему хранения и обработки информации о доменных зонах. Она необходима, в первую очередь, для соотнесения IP-адресов устройств в сети и более удобных для человеческого восприятия символьных имен (вместо числовых адресов).

11. Как называется один из протоколов синхронизации времени?

    Один из протоколов — NTP (Network Time Protocol).

12. Что такое широковещательный запрос, зачем он нужен?

    Широковещательный запрос (broadcast) — отправка пакета всем устройствам в локальной сети. Нужен для обнаружения устройств (например, DHCP-сервера), ARP-запросов, рассылки информации всем клиентам сети.

13. Какой адрес является широковещательным?

    В IPv4 широковещательный адрес обычно заканчивается на 255:
    - Для сети 192.168.1.0/24: 192.168.1.255
    - Ограниченный широковещательный адрес: 255.255.255.255

14. Какие ещё параметры можно задать сетевой карте?

    - MTU (Maximum Transmission Unit):
        ```bash
        sudo ip link set eth0 mtu 1500
        ```

    - Включение/выключение интерфейса:
        ```bash
        sudo ip link set eth0 up/down
        ```

    - Скорость и дуплекс:
        ```bash
        sudo ethtool -s eth0 speed 100 duplex full
        ```

    - Промисуальный режим:
        ```bash
        sudo ip link set eth0 promisc on
        ```

    - Длина очереди передачи:
        ```bash
        sudo ip link set eth0 txqueuelen 1000
        ```

15. Что такое маска подсети? зачем она нужна?

    Маска подсети определяет, какая часть IP-адреса относится к сети, а какая — к хосту. Например:
    - IP: 192.168.1.10
    - Маска: 255.255.255.0 (/24)
    - Сеть: 192.168.1.0
    - Хост: 10

    Маска нужна для:
    - Определения границ сети
    - Разделения больших сетей на подсети
    - Эффективной маршрутизации
    - Контроля широковещательного домена