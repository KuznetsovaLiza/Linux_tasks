# Лабораторная работа 9

## Задание 2

1. Удалите iptables и установите firewalld

    - Удаление:

        ```bash
        [student@S-vm-224 ~]$ sudo iptables --version
        iptables v1.8.10 (legacy)
        [student@S-vm-224 ~]$ sudo apt-get remove iptables
        Чтение списков пакетов... Завершено
        Построение дерева зависимостей... Завершено
        Следующие пакеты будут УДАЛЕНЫ:
        iptables
        0 будет обновлено, 0 новых установлено, 1 пакетов будет удалено и 108 не будет обновлено.
        Необходимо получить 0B архивов.
        После распаковки будет освобождено 1854kB дискового пространства.
        Продолжить? [Y/n] Y
        Совершаем изменения...
        Подготовка...                           ############################################################################################## [100%]
        Очистка / удаление... 
        1: iptables-1.8.10-alt1                 #############################################################################################предупреждение: /etc/sysconfig/iptables сохранен как /etc/sysconfig/iptables.rpmsave
        # [100%]
        Завершено.
        [student@S-vm-224 ~]$ sudo iptables --version
        sudo: iptables: команда не найдена
        ```

    - Установка:

        ```bash
        [student@S-vm-224 ~]$ sudo apt-get install firewalld
        Чтение списков пакетов... Завершено
        Построение дерева зависимостей... Завершено
        Следующие дополнительные пакеты будут установлены:
        ebtables       libcrypto3    libnm       logrotate               python3-module-charset-normalizer  python3-module-pygobject3-nox
        glib2          libexpat      libnm-gir   netplan                 python3-module-dbus                python3-module-yaml
        glib2-locales  libgio        libnspr     python3                 python3-module-firewall            python3-modules-sqlite3
        iptables       libjansson4   libnss      python3-base            python3-module-libcap-ng
        iptables-ipv6  libnftables1  libsqlite3  python3-module-Cheetah  python3-module-markupsafe
        libcap-ng      libnftnl      libssl3     python3-module-cffi     python3-module-nftables
        Следующие пакеты будут ОБНОВЛЕНЫ:
        glib2          libcap-ng   libexpat  libssl3  python3       python3-module-Cheetah  python3-module-charset-normalizer  python3-module-yaml
        glib2-locales  libcrypto3  libgio    netplan  python3-base  python3-module-cffi     python3-module-markupsafe
        Следующие НОВЫЕ пакеты будут установлены:
        ebtables   iptables-ipv6  libnftnl   libnspr     logrotate                python3-module-libcap-ng       python3-modules-sqlite3
        firewalld  libjansson4    libnm      libnss      python3-module-dbus      python3-module-nftables
        iptables   libnftables1   libnm-gir  libsqlite3  python3-module-firewall  python3-module-pygobject3-nox
        15 будет обновлено, 19 новых установлено, 0 пакетов будет удалено и 93 не будет обновлено.
        Необходимо получить 21,7MB/21,9MB архивов.
        После распаковки потребуется дополнительно 21,1MB дискового пространства.
        Продолжить? [Y/n] Y
        Получено: 1 http://ftp.altlinux.org Sisyphus/x86_64/classic ebtables 2.0.11-alt3:sisyphus+344189.100.1.1@1712048586 [79,7kB]
        Получено: 2 http://ftp.altlinux.org Sisyphus/x86_64/classic libgio 2.86.3-alt1:sisyphus+402304.100.1.1@1765288024 [732kB]
        Получено: 3 http://ftp.altlinux.org Sisyphus/noarch/classic glib2-locales 2.86.3-alt1:sisyphus+402304.100.1.1@1765288024 [1266kB]
        Получено: 4 http://ftp.altlinux.org Sisyphus/x86_64/classic glib2 2.86.3-alt1:sisyphus+402304.100.1.1@1765288024 [999kB]
        Получено: 5 http://ftp.altlinux.org Sisyphus/noarch/classic iptables-ipv6 1.8.10-alt1:sisyphus+343211.300.4.2@1713373128 [14,9kB]
        Получено: 6 http://ftp.altlinux.org Sisyphus/x86_64/classic libcap-ng 0.8.5-alt1:sisyphus+389466.3700.79.1@1759573554 [24,8kB]
        Получено: 7 http://ftp.altlinux.org Sisyphus/x86_64/classic libssl3 3.5.4-alt1:sisyphus+399237.200.1.1@1762347932 [379kB]
        Получено: 8 http://ftp.altlinux.org Sisyphus/x86_64/classic libcrypto3 3.5.4-alt1:sisyphus+399237.200.1.1@1762347932 [2008kB]
        Получено: 9 http://ftp.altlinux.org Sisyphus/x86_64/classic libexpat 2.7.3-alt1:sisyphus+396928.100.1.1@1760272632 [97,5kB]
        Получено: 10 http://ftp.altlinux.org Sisyphus/x86_64/classic libjansson4 2.14.1-alt1:sisyphus+379832.100.1.1@1743438033 [66,2kB]
        Получено: 11 http://ftp.altlinux.org Sisyphus/x86_64/classic libnftnl 1.3.1-alt1:sisyphus+401755.100.1.1@1764827931 [73,9kB]
        Получено: 12 http://ftp.altlinux.org Sisyphus/x86_64/classic libnftables1 1:1.1.6-alt1:sisyphus+402127.100.1.1@1765173943 [335kB]
        Получено: 13 http://ftp.altlinux.org Sisyphus/x86_64/classic libnspr 1:4.35-alt1:sisyphus+308164.100.1.1@1665397040 [127kB]
        Получено: 14 http://ftp.altlinux.org Sisyphus/x86_64/classic libsqlite3 3.50.4-alt1:sisyphus+391668.100.1.1@1754321126 [699kB]
        Получено: 15 http://ftp.altlinux.org Sisyphus/x86_64/classic libnss 3.119-alt1:sisyphus+402027.100.4.1@1765456169 [1374kB]
        Получено: 16 http://ftp.altlinux.org Sisyphus/x86_64/classic libnm 1.55.91-alt1:sisyphus+403036.100.1.1@1765884747 [631kB]
        Получено: 17 http://ftp.altlinux.org Sisyphus/x86_64/classic libnm-gir 1.55.91-alt1:sisyphus+403036.100.1.1@1765884747 [123kB]
        Получено: 18 http://ftp.altlinux.org Sisyphus/x86_64/classic python3-module-markupsafe 1:3.0.3-alt1:sisyphus+396886.100.1.1@1760186183 [23,5kB]
        Получено: 19 http://ftp.altlinux.org Sisyphus/noarch/classic python3-module-charset-normalizer 3.4.4-alt1:sisyphus+397817.100.1.1@1760971735 [95,1kB]
        Получено: 20 http://ftp.altlinux.org Sisyphus/x86_64/classic python3-module-Cheetah 3.4.0-alt1.1.1:sisyphus+389466.14400.79.1@1759584747 [292kB]
        Получено: 21 http://ftp.altlinux.org Sisyphus/x86_64/classic python3-module-yaml 6.0.3-alt1:sisyphus+396216.400.4.1@1760409052 [194kB]
        Получено: 22 http://ftp.altlinux.org Sisyphus/x86_64/classic python3-base 3.13.11-alt1:sisyphus+402069.100.1.1@1765057787 [8137kB]
        Получено: 23 http://ftp.altlinux.org Sisyphus/x86_64/classic python3-module-cffi 2.0.0-alt1:sisyphus+397119.600.1.2@1760517196 [279kB]
        Получено: 24 http://ftp.altlinux.org Sisyphus/x86_64/classic netplan 1.1.2-alt1:sisyphus+389466.40700.80.1@1759628445 [362kB]
        Получено: 25 http://ftp.altlinux.org Sisyphus/x86_64/classic python3 3.13.11-alt1:sisyphus+402069.100.1.1@1765057787 [1742kB]
        Получено: 26 http://ftp.altlinux.org Sisyphus/x86_64/classic python3-modules-sqlite3 3.13.11-alt1:sisyphus+402069.100.1.1@1765057787 [78,0kB]
        Получено: 27 http://ftp.altlinux.org Sisyphus/x86_64/classic python3-module-dbus 1.4.0-alt1:sisyphus+389466.107400.82.1@1759848938 [139kB]
        Получено: 28 http://ftp.altlinux.org Sisyphus/x86_64/classic python3-module-pygobject3-nox 3.54.5-alt1:sisyphus+397696.100.1.1@1760879495 [313kB]
        Получено: 29 http://ftp.altlinux.org Sisyphus/noarch/classic python3-module-nftables 1:1.1.6-alt1:sisyphus+402127.100.1.1@1765173943 [18,1kB]
        Получено: 30 http://ftp.altlinux.org Sisyphus/noarch/classic python3-module-firewall 2.4.0-alt1:sisyphus+399860.100.1.1@1762958345 [474kB]
        Получено: 31 http://ftp.altlinux.org Sisyphus/x86_64/classic python3-module-libcap-ng 0.8.5-alt1:sisyphus+389466.3700.79.1@1759573554 [23,9kB]
        Получено: 32 http://ftp.altlinux.org Sisyphus/x86_64/classic logrotate 3.20.1-alt2:sisyphus+321776.100.2.1@1684960309 [61,9kB]
        Получено: 33 http://ftp.altlinux.org Sisyphus/noarch/classic firewalld 2.4.0-alt1:sisyphus+399860.100.1.1@1762958345 [401kB]
        Получено 21,7MB за 0s (35,2MB/s).
        Совершаем изменения...
        Подготовка...                           ################################################################################################ [100%]
        Обновление / установка...
        1: libsqlite3-3.50.4-alt1              ################################################################################################ [  2%]
        2: libnspr-1:4.35-alt1                 ################################################################################################ [  4%]
        3: libcrypto3-3.5.4-alt1               ################################################################################################ [  6%]
        4: iptables-1.8.10-alt1                ################################################################################################ [  8%]
        5: iptables-ipv6-1.8.10-alt1           ################################################################################################ [ 10%]
        6: libssl3-3.5.4-alt1                  ################################################################################################ [ 12%]
        7: libnss-3.119-alt1                   ################################################################################################ [ 14%]
        8: logrotate-3.20.1-alt2               ################################################################################################ [ 16%]
        9: libnftnl-1.3.1-alt1                 ################################################################################################ [ 18%]
        10: libjansson4-2.14.1-alt1             ################################################################################################ [ 20%]
        11: libnftables1-1:1.1.6-alt1           ################################################################################################ [ 22%]
        12: libexpat-2.7.3-alt1                 ################################################################################################ [ 24%]
        13: python3-3.13.11-alt1                ################################################################################################ [ 27%]
        14: python3-modules-sqlite3-3.13.11-alt1################################################################################################ [ 29%]
        15: python3-base-3.13.11-alt1           ################################################################################################ [ 31%]
        16: python3-module-nftables-1:1.1.6-alt1################################################################################################ [ 33%]
        17: python3-module-yaml-6.0.3-alt1      ################################################################################################ [ 35%]
        18: python3-module-cffi-2.0.0-alt1      ################################################################################################ [ 37%]
        19: libcap-ng-0.8.5-alt1                ################################################################################################ [ 39%]
        20: python3-module-libcap-ng-0.8.5-alt1 ################################################################################################ [ 41%]
        21: glib2-locales-2.86.3-alt1           ################################################################################################ [ 43%]
        22: glib2-2.86.3-alt1                   ################################################################################################ [ 45%]
        23: libgio-2.86.3-alt1                  ################################################################################################ [ 47%]
        24: libnm-1.55.91-alt1                  ################################################################################################ [ 49%]
        25: libnm-gir-1.55.91-alt1              ################################################################################################ [ 51%]
        26: python3-module-dbus-1.4.0-alt1      ################################################################################################ [ 53%]
        27: python3-module-pygobject3-nox-3.54.5################################################################################################ [ 55%]
        28: python3-module-firewall-2.4.0-alt1  ################################################################################################ [ 57%]
        29: ebtables-2.0.11-alt3                ################################################################################################ [ 59%]
        30: firewalld-2.4.0-alt1                ################################################################################################ [ 61%]
        31: netplan-1.1.2-alt1                  ################################################################################################ [ 63%]
        32: python3-module-markupsafe-1:3.0.3-al################################################################################################ [ 65%]
        33: python3-module-charset-normalizer-3.################################################################################################ [ 67%]
        34: python3-module-Cheetah-3.4.0-alt1.1.################################################################################################ [ 69%]
        Очистка / удаление... 
        35: netplan-1.1.2-alt1                  ################################################################################################ [ 71%]
        36: python3-module-Cheetah-3.4.0-alt1.1.################################################################################################ [ 73%]
        37: python3-module-charset-normalizer-3.################################################################################################ [ 76%]
        38: python3-module-cffi-2.0.0-alt1      ################################################################################################ [ 78%]
        39: python3-module-yaml-6.0.2-alt1.1    ################################################################################################ [ 80%]
        40: python3-module-markupsafe-1:3.0.2-al################################################################################################ [ 82%]
        41: python3-base-3.12.11-alt1           ################################################################################################ [ 84%]
        42: python3-3.12.11-alt1                ################################################################################################ [ 86%]
        43: libssl3-3.3.3-alt1                  ################################################################################################ [ 88%]
        44: libgio-2.84.4-alt1                  ################################################################################################ [ 90%]
        45: glib2-2.84.4-alt1                   ################################################################################################ [ 92%]
        46: glib2-locales-2.84.4-alt1           ################################################################################################ [ 94%]
        47: libcrypto3-3.3.3-alt1               ################################################################################################ [ 96%]
        48: libexpat-2.7.1-alt1                 ################################################################################################ [ 98%]
        49: libcap-ng-0.8.5-alt1                ################################################################################################ [100%]
        Завершено.
        ```


2. Попробуйте так-же проверить возможность подключения по ssh

    - Запускаем firewalld, включаем автозагрузку firewalld, проверяем статус firewalld:

        ```bash
        [student@S-vm-224 ~]$ sudo systemctl start firewalld
        [student@S-vm-224 ~]$ sudo systemctl enable firewalld
        Synchronizing state of firewalld.service with SysV service script with /usr/lib/systemd/systemd-sysv-install.
        Executing: /usr/lib/systemd/systemd-sysv-install enable firewalld
        [student@S-vm-224 ~]$ sudo systemctl status firewalld
        ● firewalld.service - firewalld - dynamic firewall daemon
            Loaded: loaded (/usr/lib/systemd/system/firewalld.service; enabled; preset: enabled)
            Active: active (running) since Mon 2025-12-29 14:30:02 UTC; 48s ago
        Invocation: 62677c01bb5347589feae850ec485015
            Docs: man:firewalld(1)
        Main PID: 39126 (firewalld)
            Tasks: 2 (limit: 2332)
            Memory: 27.6M (peak: 28.9M)
                CPU: 753ms
            CGroup: /system.slice/firewalld.service
                    └─39126 /usr/bin/python3 /usr/sbin/firewalld --nofork --nopid

        дек 29 14:30:01 S-vm-224 systemd[1]: Starting firewalld.service - firewalld - dynamic firewall daemon...
        дек 29 14:30:02 S-vm-224 systemd[1]: Started firewalld.service - firewalld - dynamic firewall daemon.
        ```

    - Проверка подключения по ssh:

        ```bash
        [elizabeth@host-15 ~]$ ssh student@ternar.io -p 224
        Last login: Mon Dec 29 13:45:44 2025 from 94.180.38.175
        [student@S-vm-224 ~]$ 
        ```

3. Если её нет то откройте порт

    ```bash
    [student@S-vm-224 ~]$ sudo firewall-cmd --add-port=224/tcp --permanent
    success
    [student@S-vm-224 ~]$ sudo firewall-cmd --reload
    success
    ```

4. Выведите список открытых портов с помощью firewall-cmd

    ```bash
    [student@S-vm-224 ~]$ sudo firewall-cmd --list-ports
    224/tcp
    ```

5. Можно ли там добавить порты по названию сервиса?

    Да, firewalld использует концепцию сервисов. Например:
    - ssh = порт 22/tcp
    - http = порт 80/tcp
    - https = порт 443/tcp

6. На вашей Локальной виртуальной машине попробуйте подключиться к серверу samba из предыдущих заданий

    ```bash
    [elizabeth@host-15 ~]$ smbclient //localhost/public -N
    session setup failed: NT_STATUS_LOGON_FAILURE
    ```

7. Если не получилось то откройте нужные порты

    ```bash
    [student@S-vm-224 ~]$ sudo firewall-cmd --add-service=samba --permanent
    success
    [student@S-vm-224 ~]$ sudo firewall-cmd --reload
    success 
    ```

    ```bash
    [student@S-vm-224 ~]$ sudo firewall-cmd --list-all
    public (default, active)
    target: default
    ingress-priority: 0
    egress-priority: 0
    icmp-block-inversion: no
    interfaces: 
    sources: 
    services: dhcpv6-client samba ssh
    ports: 224/tcp
    protocols: 
    forward: yes
    masquerade: no
    forward-ports: 
    source-ports: 
    icmp-blocks: 
    rich rules:
    ```

8. Сделайте так чтобы изменения были постоянными

    В firewalld изменения становятся постоянными при использовании флага --permanent, но требуют перезагрузки правил командой firewall-cmd --reload.