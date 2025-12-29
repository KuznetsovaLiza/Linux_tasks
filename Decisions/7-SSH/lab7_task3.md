# Лабораторная работа 7

## Задание 3

1. Что такое ssh ключи и зачем они нужны?

    SSH ключи - это пара криптографических ключей (публичный и приватный) для аутентификации без пароля. Они более безопасны, чем пароли, и удобны для автоматизации.

2. Как их создать?

    Генерация пары ключей: ssh-keygen -t ed25519 -C "ваш_email@example.com"

3. Создайте пару публичный/приватный ключ ed_25519, где они хранятся?

    ```bash
    [elizabeth@host-15 ~]$ ssh-keygen -t ed25519
    Generating public/private ed25519 key pair.
    Enter file in which to save the key (/home/elizabeth/.ssh/id_ed25519): 
    Enter passphrase (empty for no passphrase): 
    Enter same passphrase again: 
    Your identification has been saved in /home/elizabeth/.ssh/id_ed25519
    Your public key has been saved in /home/elizabeth/.ssh/id_ed25519.pub
    The key fingerprint is:
    SHA256:CF+z2DH6GtEh1qFsE8lTJIpfMnAjeioEdqXydEa4H+o elizabeth@host-15
    The key's randomart image is:
    +--[ED25519 256]--+
    |..o.=+o++        |
    |o..*o++= .       |
    |.ooo*oO.B        |
    |.o+ooX X *       |
    |o  .+ B S        |
    |.  . . o         |
    |  .   . .        |
    |   E   o         |
    |      .          |
    +----[SHA256]-----+
    [elizabeth@host-15 ~]$ ls -la ~/.ssh
    итого 32
    drwx------  2 elizabeth elizabeth 4096 дек 28 19:36 .
    drwx------ 18 elizabeth elizabeth 4096 дек 28 19:04 ..
    -rw-------  1 elizabeth elizabeth   75 июл 26  2021 authorized_keys
    -rw-------  1 elizabeth elizabeth   86 июл 26  2021 config
    -rw-------  1 elizabeth elizabeth  411 дек 28 19:36 id_ed25519
    -rw-r--r--  1 elizabeth elizabeth   99 дек 28 19:36 id_ed25519.pub
    -rw-------  1 elizabeth elizabeth  843 дек 27 16:24 known_hosts
    -rw-r--r--  1 elizabeth elizabeth   97 дек 27 16:23 known_hosts.old
    ```

    - Приватный ключ: ~/.ssh/id_ed25519
    - Публичный ключ: ~/.ssh/id_ed25519.pub

4. Скопируйте публичный ключ на ваш сервер, в каком файле он будет храниться?

    ```bash
    [elizabeth@host-15 ~]$ ssh-copy-id -i ~/.ssh/id_ed25519.pub -p 224 student@ternar.io
    /usr/bin/ssh-copy-id: INFO: Source of key(s) to be installed: "/home/elizabeth/.ssh/id_ed25519.pub"
    /usr/bin/ssh-copy-id: INFO: attempting to log in with the new key(s), to filter out any that are already installed
    /usr/bin/ssh-copy-id: INFO: 1 key(s) remain to be installed -- if you are prompted now it is to install the new keys
    student@ternar.io's password: 

    Number of key(s) added: 1

    Now try logging into the machine, with:   "ssh -p 224 'student@ternar.io'"
    and check to make sure that only the key(s) you wanted were added.

    [elizabeth@host-15 ~]$ cat ~/.ssh/id_ed25519.pub
    ssh-ed25519 AAAAC3NzaC1lZDI1NTE5AAAAIE6w3l6/GwjOKWLRWw4YPSgHMOGXVenBvnvfmC+U4ZTX elizabeth@host-15
    ```

    Файл хранения публичного ключа на сервере: ~/.ssh/authorized_keys

    ```bash
    [student@S-vm-224 ~]$ cat ~/.ssh/authorized_keys 
    # OpenSSH authorized_keys file format is described in sshd(8) manual page.
    ssh-ed25519 AAAAC3NzaC1lZDI1NTE5AAAAIE6w3l6/GwjOKWLRWw4YPSgHMOGXVenBvnvfmC+U4ZTX elizabeth@host-15
    ```

5. Попробуйте подключиться к серверу, у вас запросили пароль?

    ```bash
    [elizabeth@host-15 ~]$ ssh student@ternar.io -p 224
    Last login: Sun Dec 28 17:59:38 2025 from 94.180.38.175
    [student@S-vm-224 ~]$
    ```

    Пароль не был запрошен

6. Запретите подключение с паролем для всех пользователей, оставьте только с помощью ключа.

    ```bash
    [student@S-vm-224 ~]$ sudo vi /etc/openssh/sshd_config
    [student@S-vm-224 ~]$ sudo systemctl restart sshd
    ```

    В файле sshd_config были изменены строчки:
    - PubkeyAuthentication yes
    - PasswordAuthentication no

    Подключение с ключом:

    ```bash
    [elizabeth@host-15 ~]$ ssh -i ~/.ssh/id_ed25519 -p 224 student@ternar.io
    Last login: Mon Dec 29 08:21:10 2025 from 94.180.38.175
    [student@S-vm-224 ~]$  
    ```