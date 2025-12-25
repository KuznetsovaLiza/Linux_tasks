# Лабораторная работа 8

## Задание 1

1. Установите пакет samba

    ```bash
    [elizabeth@host-15 ~]$ sudo apt-get update
    ```

    ```bash
    [elizabeth@host-15 ~]$ sudo apt-get install samba samba-client
    ```

2. Что такое общая папка, зачем она может быть нужна?

    Общая папка (shared folder) - это директория в сети, к которой могут подключаться и получать доступ несколько пользователей или компьютеров.  
    Она нужна для:
    - Совместной работы с файлами
    - Обмена документами в локальной сети
    - Централизованного хранения данных
    - Резервного копирования
    - Доступа к файлам с разных устройств

3. Создайте общую папку без пароля с правами только на чтение файлов

    ```bash
    [elizabeth@host-15 ~]$ sudo mkdir -p /srv/samba/public
    [elizabeth@host-15 ~]$ sudo chmod 755 /srv/samba/public
    [elizabeth@host-15 ~]$ sudo nano /etc/samba/smb.conf
    [elizabeth@host-15 ~]$ sudo systemctl restart smb
    [elizabeth@host-15 ~]$ echo "Файл только для чтения" | sudo tee /srv/samba/public/test.txt
    [sudo] password for elizabeth:
    Файл только для чтения
    [elizabeth@host-15 ~]$ ls -l /srv/samba/public
    итого 4
    -rw-r--r-- 1 root root 42 дек 25 17:57 test.txt
    [elizabeth@host-15 ~]$ 
    ```

    В конец файла smb.conf была добавлена секция public:

    ```bash
    [public]
       comment = Общая папка только для чтения
       path = /srv/samba/public
       browseable = yes
       read only = yes
       guest ok = yes
       create mask = 0644
       directory mask = 0755
    ```

4. Создайте общую папку с паролем с правами на чтение и запись

    ```bash
    [elizabeth@host-15 ~]$ sudo mkdir -p /srv/samba/private
    [sudo] password for elizabeth:
    [elizabeth@host-15 ~]$ sudo chmod 775 /srv/samba/private
    [elizabeth@host-15 ~]$ sudo useradd -M -s /sbin/nologin sambauser
    [elizabeth@host-15 ~]$ sudo smbpasswd -a sambauser
    New SMB password:
    Retype new SMB password:
    Added user sambauser.
    [elizabeth@host-15 ~]$ sudo chown sambauser:sambauser /srv/samba/private
    [elizabeth@host-15 ~]$ sudo nano /etc/samba/smb.conf
    [elizabeth@host-15 ~]$ sudo systemctl restart smb
    ```

    В конец файла smb.conf была добавлена секция private:

    ```bash
    [private]
       comment = Приватная папка с записью
       path = /srv/samba/private
       browseable = yes
       read only = no
       guest ok = no
       valid users = sambauser
       create mask = 0644
       directory mask = 0755
    ```

5. Создайте общую папку с доступом для какой-то группы с полными правами

    ```bash
    [elizabeth@host-15 ~]$ sudo groupadd sambagroup
    [sudo] password for elizabeth:
    [elizabeth@host-15 ~]$ sudo mkdir -p /srv/samba/group_shared
    [elizabeth@host-15 ~]$ sudo chgrp sambagroup /srv/samba/group_shared
    [elizabeth@host-15 ~]$ sudo chmod 2770 /srv/samba/group_shared
    [elizabeth@host-15 ~]$ sudo useradd -M -s /sbin/nologin smb_user1
    [elizabeth@host-15 ~]$ sudo useradd -M -s /sbin/nologin smb_user2
    [elizabeth@host-15 ~]$ sudo usermod -aG sambagroup smb_user1
    [elizabeth@host-15 ~]$ sudo usermod -aG sambagroup smb_user2
    [elizabeth@host-15 ~]$ sudo smbpasswd -a smb_user1
    New SMB password:
    Retype new SMB password:
    Added user smb_user1.
    [elizabeth@host-15 ~]$ sudo smbpasswd -a smb_user2
    New SMB password:
    Retype new SMB password:
    Added user smb_user2.
    [elizabeth@host-15 ~]$ sudo nano /etc/samba/smb.conf
    [elizabeth@host-15 ~]$ sudo systemctl restart smb
    ```

    В конец файла smb.conf была добавлена секция group_shared:

    ```bash
    [group_shared]
       comment = Папка для группы
       path = /srv/samba/group_shared
       browseable = yes
       read only = no
       guest ok = no
       valid users = @sambagroup
       create mask = 0660
       directory mask = 2770
       force group = sambagroup
    ```

6. Создайте общую папку в которой у одной группы будет полный доступ, а у другой только доступ на чтение. Третья группа не должна иметь к ней доступа

    ```bash
    [elizabeth@host-15 ~]$ sudo groupadd fullaccess
    [elizabeth@host-15 ~]$ sudo groupadd readonly
    [elizabeth@host-15 ~]$ sudo groupadd noaccess
    [elizabeth@host-15 ~]$ sudo mkdir -p /srv/samba/multi_group
    [elizabeth@host-15 ~]$ sudo chmod 770 /srv/samba/multi_group
    [elizabeth@host-15 ~]$ sudo useradd -M -s /sbin/nologin user_full
    [elizabeth@host-15 ~]$ sudo useradd -M -s /sbin/nologin user_read
    [elizabeth@host-15 ~]$ sudo useradd -M -s /sbin/nologin user_no
    [elizabeth@host-15 ~]$ sudo usermod -aG fullaccess user_full
    [elizabeth@host-15 ~]$ sudo usermod -aG readonly user_read
    [elizabeth@host-15 ~]$ sudo usermod -aG noaccess user_no
    [elizabeth@host-15 ~]$ sudo smbpasswd -a user_full
    New SMB password:
    Retype new SMB password:
    Added user user_full.
    [elizabeth@host-15 ~]$ sudo smbpasswd -a user_read
    New SMB password:
    Retype new SMB password:
    Added user user_read.
    [elizabeth@host-15 ~]$ sudo smbpasswd -a user_no
    New SMB password:
    Retype new SMB password:
    Added user user_no.
    [elizabeth@host-15 ~]$ sudo nano /etc/samba/smb.conf
    [elizabeth@host-15 ~]$ sudo systemctl restart smb
    ```

    В конец файла smb.conf была добавлена секция multi_group:

    ```bash
    [multi_group]
       comment = Папка с разными правами для групп
       path = /srv/samba/multi_group
       browseable = yes
       read only = yes
       guest ok = no
    
       write list = @fullaccess
       read list = @fullaccess, @readonly
       invalid users = @noaccess
    
       create mask = 0660
       directory mask = 0770
    ```