# Проверка подключения к папкам (samba)

## Linux

### Public:

```bash
[elizabeth@host-15 ~]$ smbclient //localhost/public -U ""%""
Try "help" to get a list of possible commands.
smb: \> ls
  .                                   D        0  Thu Dec 25 17:57:03 2025
  ..                                  D        0  Thu Dec 25 17:57:03 2025
  test.txt                            N       42  Thu Dec 25 17:57:03 2025

		32717596 blocks of size 1024. 17575348 blocks available
smb: \> get test.txt
getting file \test.txt of size 42 as test.txt (4,1 KiloBytes/sec) (average 4,1 KiloBytes/sec)
smb: \> put newfile.txt
NT_STATUS_ACCESS_DENIED opening remote file \newfile.txt
smb: \> quit
[elizabeth@host-15 ~]$ 
```

### Private:

```bash
[elizabeth@host-15 ~]$ smbclient //localhost/private -U sambauser
Password for [SAMBA\sambauser]:
Try "help" to get a list of possible commands.
smb: \> ls
  .                                   D        0  Thu Jan  8 13:56:37 2026
  ..                                  D        0  Thu Jan  8 13:56:37 2026
  test.txt                            N       45  Thu Dec 25 18:20:27 2025

		32717596 blocks of size 1024. 17575352 blocks available
smb: \> put newfile.txt
putting file newfile.txt as \newfile.txt (1,8 kb/s) (average 1,8 kb/s)
smb: \> ls
  .                                   D        0  Thu Jan  8 13:56:48 2026
  ..                                  D        0  Thu Jan  8 13:56:48 2026
  test.txt                            N       45  Thu Dec 25 18:20:27 2025
  newfile.txt                         A        9  Thu Jan  8 13:56:48 2026

		32717596 blocks of size 1024. 17575348 blocks available
smb: \> quit
[elizabeth@host-15 ~]$
``` 

### Group_shared:

```bash
[elizabeth@host-15 ~]$ smbclient //localhost/group_shared -U smb_user1
Password for [SAMBA\smb_user1]:
Try "help" to get a list of possible commands.
smb: \> ls
  .                                   D        0  Thu Dec 25 18:26:41 2025
  ..                                  D        0  Thu Dec 25 18:26:41 2025

		32717596 blocks of size 1024. 17575344 blocks available
smb: \> put newfile.txt
putting file newfile.txt as \newfile.txt (1,8 kb/s) (average 1,8 kb/s)
smb: \> ls
  .                                   D        0  Thu Jan  8 14:10:59 2026
  ..                                  D        0  Thu Jan  8 14:10:59 2026
  newfile.txt                         A        9  Thu Jan  8 14:10:59 2026

		32717596 blocks of size 1024. 17575340 blocks available
smb: \> get newfile.txt
getting file \newfile.txt of size 9 as newfile.txt (2,2 KiloBytes/sec) (average 2,2 KiloBytes/sec)
smb: \> get newfile.txt /dev/stdout
new file
getting file \newfile.txt of size 9 as /dev/stdout (8,8 KiloBytes/sec) (average 3,5 KiloBytes/sec)
smb: \> quit
[elizabeth@host-15 ~]$ 
```

### Также были внесены изменения, так как возникала ошибка:

```bash
[elizabeth@host-15 ~]$ smbclient //localhost/multi_group -U user_full
Password for [SAMBA\user_full]:
Try "help" to get a list of possible commands.
smb: \> ls
NT_STATUS_ACCESS_DENIED listing \*
```

Исправление владельца папки

До исправления:

```bash
[elizabeth@host-15 ~]$ ls -ld /srv/samba/multi_group
drwxrwx--- 2 root root 4096 дек 25 18:46 /srv/samba/multi_group
```

После:

```bash
[elizabeth@host-15 ~]$ sudo chgrp fullaccess /srv/samba/multi_group
[sudo] password for elizabeth:
[elizabeth@host-15 ~]$ ls -ld /srv/samba/multi_group
drwxrwx--- 2 root fullaccess 4096 дек 25 18:46 /srv/samba/multi_group
```

Изменение секции multi_group:

```bash
[elizabeth@host-15 ~]$ sudo nano /etc/samba/smb.conf
```

```bash
[multi_group]
   comment = Папка с разными правами для групп
   path = /srv/samba/multi_group
   browseable = yes
   read only = yes
   guest ok = no
   
   valid users = @fullaccess, @readonly
   
   write list = @fullaccess
   
   force group = fullaccess
   create mask = 0660
   directory mask = 0770
```

```bash
[elizabeth@host-15 ~]$ sudo systemctl restart smb nmb
```

### Multi_group:

```bash
[elizabeth@host-15 ~]$ smbclient //localhost/multi_group -U user_full
Password for [SAMBA\user_full]:
Try "help" to get a list of possible commands.
smb: \> ls
  .                                   D        0  Thu Dec 25 18:46:34 2025
  ..                                  D        0  Thu Dec 25 18:46:34 2025

		32717596 blocks of size 1024. 17567124 blocks available
smb: \> put newfile.txt
putting file newfile.txt as \newfile.txt (1,5 kb/s) (average 1,5 kb/s)
smb: \> ls
  .                                   D        0  Thu Jan  8 14:59:50 2026
  ..                                  D        0  Thu Jan  8 14:59:50 2026
  newfile.txt                         A        9  Thu Jan  8 14:59:50 2026

		32717596 blocks of size 1024. 17567120 blocks available
smb: \> get newfile.txt /dev/stdout
new file
getting file \newfile.txt of size 9 as /dev/stdout (4,4 KiloBytes/sec) (average 4,4 KiloBytes/sec)
smb: \> quit
```

```bash
[elizabeth@host-15 ~]$ smbclient //localhost/multi_group -U user_read
Password for [SAMBA\user_read]:
Try "help" to get a list of possible commands.
smb: \> ls
  .                                   D        0  Thu Jan  8 14:59:50 2026
  ..                                  D        0  Thu Jan  8 14:59:50 2026
  newfile.txt                         A        9  Thu Jan  8 14:59:50 2026

		32717596 blocks of size 1024. 17567120 blocks available
smb: \> put test.txt
NT_STATUS_ACCESS_DENIED opening remote file \test.txt
smb: \> ls
  .                                   D        0  Thu Jan  8 14:59:50 2026
  ..                                  D        0  Thu Jan  8 14:59:50 2026
  newfile.txt                         A        9  Thu Jan  8 14:59:50 2026

		32717596 blocks of size 1024. 17567120 blocks available
smb: \> get newfile.txt /dev/stdout
new file
getting file \newfile.txt of size 9 as /dev/stdout (2,9 KiloBytes/sec) (average 2,9 KiloBytes/sec)
smb: \> quit
```

```bash
[elizabeth@host-15 ~]$ smbclient //localhost/multi_group -U user_no
Password for [SAMBA\user_no]:
tree connect failed: NT_STATUS_ACCESS_DENIED
[elizabeth@host-15 ~]$
``` 


## Windows

### IP адрес виртуальной машины:
```bash
[elizabeth@host-15 ~]$ hostname -i
192.168.0.104
```

### cmd:

### Проверка каждой папки:

Проверяем доступность сервера: ping 192.168.0.104

### Public:

![](check_win1.png)
![](check_win2.png)
![](check_win3.png)
![](check_win4.png)
![](check_win5.png)

### Private:

![](check_win6.png)

Причина ошибки 1219:  
Windows кеширует учетные данные для сетевых ресурсов. Когда происходит подключение к \\192.168.0.104 с разными пользователями (например, сначала к public без пароля, потом к private с паролем), возникает конфликт.

Решение:
- Посмотреть текущие подключения: net use
- Отключить ВСЕ сетевые подключения: net use * /delete /y
- Подключиться к новым ресурсам

![](check_win7.png)
![](check_win8.png)
![](check_win9.png)

### Далее проверяется каждая папка по аналогии

### Group_shared:

![](check_win10.png)
![](check_win11.png)
![](check_win12.png)

### Multi_group:

![](check_win13.png)
![](check_win14.png)
![](check_win15.png)

![](check_win16.png)
![](check_win17.png)
![](check_win18.png)

![](check_win19.png)
![](check_win20.png)
![](check_win21.png)