# Лабораторная работа 5

## Задание 1

1. Что такое systemd юнит?

    systemd юнит - это конфигурационный файл, который описывает как systemd должен управлять службой, устройством, точкой монтирования и другими системными ресурсами. Юниты имеют расширение .service, .timer, .mount, .socket и т.д.

2. Проверьте статус любого systemd юнита, какую информацию выводит эта команда?

    ```bash
    [root@host-15 ~]# systemctl status sshd.service
    ● sshd.service - OpenSSH server daemon
        Loaded: loaded (/usr/lib/systemd/system/sshd.service; enabled; preset: enabled)
        Active: active (running) since Wed 2025-12-24 13:19:15 +04; 2min 36s ago
        Process: 974 ExecStartPre=/usr/bin/ssh-keygen -A (code=exited, status=0/SUCCESS)
        Process: 989 ExecStartPre=/usr/sbin/sshd -t (code=exited, status=0/SUCCESS)
    Main PID: 1025 (sshd)
        Tasks: 1 (limit: 4620)
        Memory: 3.7M (peak: 4.0M)
            CPU: 243ms
        CGroup: /system.slice/sshd.service
                └─1025 "sshd: /usr/sbin/sshd -D [listener] 0 of 10-100 startups"

    дек 24 13:19:14 host-15 systemd[1]: Starting sshd.service - OpenSSH server daemon...
    дек 24 13:19:15 host-15 systemd[1]: Started sshd.service - OpenSSH server daemon.
    дек 24 13:19:15 host-15 sshd[1025]: Server listening on 0.0.0.0 port 22.
    дек 24 13:19:16 host-15 sshd[1025]: Server listening on :: port 22.
    ```

    Информация, которую выводит команда:
    - Состояние службы (активна/неактивна)
    - Описание службы
    - Загружена ли она
    - PID процесса
    - Журнальные сообщения
    - Информация о процессе

3. Попробуйте остановить сервис.

    ```bash
    [root@host-15 ~]# systemctl stop sshd.service
    [root@host-15 ~]# systemctl status sshd.service
    ○ sshd.service - OpenSSH server daemon
        Loaded: loaded (/usr/lib/systemd/system/sshd.service; enabled; preset: enabled)
        Active: inactive (dead) since Wed 2025-12-24 13:23:34 +04; 58s ago
    Duration: 4min 18.834s
        Process: 974 ExecStartPre=/usr/bin/ssh-keygen -A (code=exited, status=0/SUCCESS)
        Process: 989 ExecStartPre=/usr/sbin/sshd -t (code=exited, status=0/SUCCESS)
        Process: 1025 ExecStart=/usr/sbin/sshd -D $EXTRAOPTIONS (code=exited, status=0/SUCCESS)
    Main PID: 1025 (code=exited, status=0/SUCCESS)
            CPU: 246ms

    дек 24 13:19:14 host-15 systemd[1]: Starting sshd.service - OpenSSH server daemon...
    дек 24 13:19:15 host-15 systemd[1]: Started sshd.service - OpenSSH server daemon.
    дек 24 13:19:15 host-15 sshd[1025]: Server listening on 0.0.0.0 port 22.
    дек 24 13:19:16 host-15 sshd[1025]: Server listening on :: port 22.
    дек 24 13:23:34 host-15 systemd[1]: Stopping sshd.service - OpenSSH server daemon...
    дек 24 13:23:34 host-15 sshd[1025]: Received signal 15; terminating.
    дек 24 13:23:34 host-15 systemd[1]: sshd.service: Deactivated successfully.
    дек 24 13:23:34 host-15 systemd[1]: Stopped sshd.service - OpenSSH server daemon.
    ```

4. Перезапустите его.

    ```bash
    [root@host-15 ~]# systemctl restart sshd.service
    [root@host-15 ~]# systemctl status sshd.service
    ● sshd.service - OpenSSH server daemon
        Loaded: loaded (/usr/lib/systemd/system/sshd.service; enabled; preset: enabled)
        Active: active (running) since Wed 2025-12-24 13:25:24 +04; 16s ago
        Process: 2980 ExecStartPre=/usr/bin/ssh-keygen -A (code=exited, status=0/SUCCESS)
        Process: 2982 ExecStartPre=/usr/sbin/sshd -t (code=exited, status=0/SUCCESS)
    Main PID: 2984 (sshd)
        Tasks: 1 (limit: 4620)
        Memory: 1.2M (peak: 1.5M)
            CPU: 218ms
        CGroup: /system.slice/sshd.service
                └─2984 "sshd: /usr/sbin/sshd -D [listener] 0 of 10-100 startups"

    дек 24 13:25:24 host-15 systemd[1]: Starting sshd.service - OpenSSH server daemon...
    дек 24 13:25:24 host-15 systemd[1]: Started sshd.service - OpenSSH server daemon.
    дек 24 13:25:25 host-15 sshd[2984]: Server listening on 0.0.0.0 port 22.
    дек 24 13:25:25 host-15 sshd[2984]: Server listening on :: port 22.
    ```

5. Удалите из автозагрузки

    ```bash
    [root@host-15 ~]# systemctl disable sshd.service
    Synchronizing state of sshd.service with SysV service script with /usr/lib/systemd/systemd-sysv-install.
    Executing: /usr/lib/systemd/systemd-sysv-install disable sshd
    Removed "/etc/systemd/system/multi-user.target.wants/sshd.service".
    ```

6. Верните обратно

    ```bash
    [root@host-15 ~]# systemctl enable sshd.service
    Synchronizing state of sshd.service with SysV service script with /usr/lib/systemd/systemd-sysv-install.
    Executing: /usr/lib/systemd/systemd-sysv-install enable sshd
    Created symlink /etc/systemd/system/multi-user.target.wants/sshd.service → /usr/lib/systemd/system/sshd.service.
    ```

7. Что такое таймеры?

    Таймеры - это systemd юниты, которые запускают другие юниты по расписанию. Они более гибкие и интегрированы в systemd.