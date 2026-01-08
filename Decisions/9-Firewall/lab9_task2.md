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
    smbclient //localhost/public -U ""%""
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