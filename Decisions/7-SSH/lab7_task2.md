# Лабораторная работа 7

## Задание 2

1. Где хранятся пользовательские и системные настройки подключения?

    - Пользовательские настройки: ~/.ssh/config
    - Системные настройки: /etc/openssh/ssh_config

2. Что за файл options?

    Файл options - устаревшие конфигурационные файлы SSH клиента (~/.ssh/config), где хранятся параметры подключения к разным хостам.

3. Отредактируйте файл options так, чтобы можно было подключаться не вводя имя пользователя и порт

    ```bash
    [elizabeth@host-15 ~]$ nano ~/.ssh/config
    ```

4. Назовите подключение удобным для вас способом

    Добавленное содержимое файла config:

    ```bash
    Host myserver
        HostName ternar.io
        User student
        Port 224
    ```

5. Проверьте работоспособность

    ```bash
    [elizabeth@host-15 ~]$ cat ~/.ssh/config
    # OpenSSH client configuration file format is described in ssh_config(5) manual page.
    Host myserver
        HostName ternar.io
        User student
        Port 224
    ```

    ```bash
    [elizabeth@host-15 ~]$ ssh myserver
    student@ternar.io's password: 
    Last login: Mon Dec 29 07:43:03 2025 from 94.180.38.175
    [student@S-vm-224 ~]$ 
    ```