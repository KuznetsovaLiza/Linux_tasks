# Лабораторная работа 5

## Задание 3

1. Посмотрите журналы ssh

    ```bash
    [elizabeth@host-15 ~]$ sudo journalctl -u sshd.service
    [sudo] password for elizabeth:
    сен 09 17:12:55 host-15 systemd[1]: Starting sshd.service - OpenSSH server daemon...
    сен 09 17:12:56 host-15 systemd[1]: Started sshd.service - OpenSSH server daemon.
    сен 09 17:12:56 host-15 sshd[968]: Server listening on 0.0.0.0 port 22.
    сен 09 17:12:56 host-15 sshd[968]: Server listening on :: port 22.
    сен 09 17:19:57 host-15 sshd[968]: Received signal 15; terminating.
    сен 09 17:19:57 host-15 systemd[1]: Stopping sshd.service - OpenSSH server daemon...
    сен 09 17:19:57 host-15 systemd[1]: sshd.service: Deactivated successfully.
    сен 09 17:19:57 host-15 systemd[1]: Stopped sshd.service - OpenSSH server daemon.
    -- Boot e8c023db1a664d5bae510088216dafe6 --
    сен 09 17:21:05 host-15 systemd[1]: Starting sshd.service - OpenSSH server daemon...
    сен 09 17:21:06 host-15 systemd[1]: Started sshd.service - OpenSSH server daemon.
    сен 09 17:21:06 host-15 sshd[960]: Server listening on 0.0.0.0 port 22.
    сен 09 17:21:06 host-15 sshd[960]: Server listening on :: port 22.
    сен 09 17:22:06 host-15 sshd[960]: Received signal 15; terminating.
    сен 09 17:22:06 host-15 systemd[1]: Stopping sshd.service - OpenSSH server daemon...
    сен 09 17:22:06 host-15 systemd[1]: sshd.service: Deactivated successfully.
    сен 09 17:22:06 host-15 systemd[1]: Stopped sshd.service - OpenSSH server daemon.
    -- Boot 4841a4e4e906406baa91df9ed347b766 --
    сен 18 12:28:18 host-15 systemd[1]: Starting sshd.service - OpenSSH server daemon...
    сен 18 12:28:22 host-15 systemd[1]: Started sshd.service - OpenSSH server daemon.
    сен 18 12:28:23 host-15 sshd[966]: Server listening on 0.0.0.0 port 22.
    сен 18 12:28:23 host-15 sshd[966]: Server listening on :: port 22.
    сен 18 13:46:31 host-15 sshd[966]: Received signal 15; terminating.

    ```

2. Выведите журналы в реальном времени

    ```bash
    [elizabeth@host-15 ~]$ sudo journalctl -f
    дек 24 17:06:48 host-15 create_files.sh[5257]: Файл 3.txt уже существует
    дек 24 17:06:48 host-15 create_files.sh[5257]: Файл 4.txt уже существует
    дек 24 17:06:48 host-15 systemd[1]: create-files.service: Deactivated successfully.
    дек 24 17:06:48 host-15 systemd[1]: Finished create-files.service - Create files with system information.
    дек 24 17:06:49 host-15 sudo[5253]: UNSPECIFIED (__progname="sudo" uid=1000 euid=0): pam_tcb(sudo:auth): Authentication passed for elizabeth from elizabeth(uid=1000)
    дек 24 17:06:49 host-15 sudo[5253]: elizabeth : HOST=host-15 ; TTY=pts/0 ; PWD=/home/elizabeth ; USER=root ; COMMAND=/bin/journalctl -u sshd.service
    дек 24 17:06:49 host-15 sudo[5253]: pam_tcb(sudo:session): Session opened for root by elizabeth(uid=1000)
    дек 24 17:07:02 host-15 sudo[5253]: pam_tcb(sudo:session): Session closed for root
    дек 24 17:07:18 host-15 sudo[5267]: elizabeth : HOST=host-15 ; TTY=pts/0 ; PWD=/home/elizabeth ; USER=root ; COMMAND=/bin/journalctl -f
    дек 24 17:07:18 host-15 sudo[5267]: pam_tcb(sudo:session): Session opened for root by elizabeth(uid=1000)

    ```

3. Выведите лог в реальном времени для службы sshd

    ```bash
    [elizabeth@host-15 ~]$ sudo journalctl -u sshd.service -f
    дек 24 13:19:15 host-15 sshd[1025]: Server listening on 0.0.0.0 port 22.
    дек 24 13:19:16 host-15 sshd[1025]: Server listening on :: port 22.
    дек 24 13:23:34 host-15 systemd[1]: Stopping sshd.service - OpenSSH server daemon...
    дек 24 13:23:34 host-15 sshd[1025]: Received signal 15; terminating.
    дек 24 13:23:34 host-15 systemd[1]: sshd.service: Deactivated successfully.
    дек 24 13:23:34 host-15 systemd[1]: Stopped sshd.service - OpenSSH server daemon.
    дек 24 13:25:24 host-15 systemd[1]: Starting sshd.service - OpenSSH server daemon...
    дек 24 13:25:24 host-15 systemd[1]: Started sshd.service - OpenSSH server daemon.
    дек 24 13:25:25 host-15 sshd[2984]: Server listening on 0.0.0.0 port 22.
    дек 24 13:25:25 host-15 sshd[2984]: Server listening on :: port 22.

    ```

4. Можно ли без команды journalctl прочитать логи systemd?

    Да, можно обойтись без journalctl, но это будет менее удобно и информативно:
    - Для быстрой проверки: sudo systemctl status service-name -l
    - Для общих логов: tail -f /var/log/syslog
    - Для загрузочных логов: sudo dmesg -T
    - Для экстренных случаев: strings /var/log/journal/*/system.journal

5. Сколько будет 2-2?

    2 - 2 = 0