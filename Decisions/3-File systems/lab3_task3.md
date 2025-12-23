# Лабораторная работа 3

## Задание 3

1. Выведите содержимое fstab. Что хранится в fstab?

    ```bash
    [elizabeth@host-15 ~]$ cat /etc/fstab
    proc		/proc			proc	nosuid,noexec,gid=proc				0 0
    devpts		/dev/pts		devpts  nosuid,noexec,gid=tty,mode=620,ptmxmode=0666	0 0
    tmpfs		/tmp			tmpfs	nosuid						0 0
    UUID=eb15519e-54bc-4441-b4a2-ccf21c83fdcf	/	ext4	relatime	1	1
    UUID=190B-4A85	/boot/efi	vfat	umask=0,quiet,showexec,iocharset=utf8,codepage=866	1	2
    UUID=58b6f079-0a64-46a8-ac65-d94a53e65351	swap	swap	defaults	0	0
    /dev/sr0	/media/ALTLinux	udf,iso9660	ro,noauto,user,utf8,nofail,comment=x-gvfs-show	0 0
    ```

    Формат записей: [устройство] [точка_монтирования] [тип_ФС] [опции] [dump] [pass]

2. Добавьте в виртуальную машину ещё один диск

    До добавления диска:

    ```bash
    [elizabeth@host-15 ~]$ lsblk
    NAME   MAJ:MIN RM  SIZE RO TYPE MOUNTPOINTS
    sda      8:0    0   40G  0 disk 
    ├─sda1   8:1    0  511M  0 part /boot/efi
    ├─sda2   8:2    0  7,6G  0 part [SWAP]
    └─sda3   8:3    0 31,9G  0 part /
    sr0     11:0    1  6,4G  0 rom
    ```

3. Узнайте как система видит ваш диск - выведите информацию о блочных устройствах

    После добавления диска (sdb - новый диск):

    ```bash
    [elizabeth@host-15 ~]$ lsblk
    NAME   MAJ:MIN RM  SIZE RO TYPE MOUNTPOINTS
    sda      8:0    0   40G  0 disk 
    ├─sda1   8:1    0  511M  0 part /boot/efi
    ├─sda2   8:2    0  7,6G  0 part [SWAP]
    └─sda3   8:3    0 31,9G  0 part /
    sdb      8:16   0   10G  0 disk 
    sr0     11:0    1  6,4G  0 rom      
    ```

4. С помощью полученной информации создайте на диске таблицу разделов и файловую систему ext4

    - Создание раздела:

    ```bash
    [elizabeth@host-15 ~]$ sudo fdisk /dev/sdb
    [sudo] password for elizabeth:

    Добро пожаловать в fdisk (util-linux 2.39.2).
    Изменения останутся только в памяти до тех пор, пока вы не решите записать их.
    Будьте внимательны, используя команду write.

    Устройство не содержит стандартной таблицы разделов.
    Created a new DOS (MBR) disklabel with disk identifier 0x0c853c5a.

    Команда (m для справки): n
    Тип раздела
    p   основной (0 primary, 0 extended, 4 free)
    e   расширенный (контейнер для логических разделов)
    Выберите (по умолчанию - p):p
    Номер раздела (1-4, default 1): 1
    Первый сектор (2048-20971519, default 2048): 
    Last sector, +/-sectors or +/-size{K,M,G,T,P} (2048-20971519, default 20971519): 

    Создан новый раздел 1 с типом 'Linux' и размером 10 GiB.

    Команда (m для справки): p
    Диск /dev/sdb: 10 GiB, 10737418240 байт, 20971520 секторов
    Disk model: VBOX HARDDISK   
    Единицы: секторов по 1 * 512 = 512 байт
    Размер сектора (логический/физический): 512 байт / 512 байт
    Размер I/O (минимальный/оптимальный): 512 байт / 512 байт
    Тип метки диска: dos
    Идентификатор диска: 0x0c853c5a

    Устр-во    Загрузочный начало    Конец  Секторы Размер Идентификатор Тип
    /dev/sdb1                2048 20971519 20969472    10G            83 Linux

    Команда (m для справки): w
    Таблица разделов была изменена.
    Вызывается ioctl() для перечитывания таблицы разделов.
    Синхронизируются диски. 
    ```

    - Создание ФС ext4:

    ```bash
    [elizabeth@host-15 ~]$ sudo mkfs.ext4 /dev/sdb1
    [sudo] password for elizabeth:
    mke2fs 1.47.1 (20-May-2024)
    Creating filesystem with 2621184 4k blocks and 655360 inodes
    Filesystem UUID: eb345b4b-1feb-47e7-87db-9e9c02dbd9b3
    Superblock backups stored on blocks: 
        32768, 98304, 163840, 229376, 294912, 819200, 884736, 1605632

    Allocating group tables: done                            
    Writing inode tables: done                            
    Creating journal (16384 blocks): done
    Writing superblocks and filesystem accounting information: done
    ```

5. Примонтируте диск в каталог /mnt

    ```bash
    [elizabeth@host-15 ~]$ sudo mkdir /mnt/mydisk
    [elizabeth@host-15 ~]$ ls /mnt
    mydisk
    [elizabeth@host-15 ~]$ sudo mount /dev/sdb1 /mnt/mydisk
    ```

6. Зайдите в каталог и создайте там файлы

    ```bash
    [elizabeth@host-15 ~]$ ls /mnt/mydisk
    lost+found
    [elizabeth@host-15 ~]$ sudo touch /mnt/mydisk/test_file1.txt
    [elizabeth@host-15 ~]$ sudo touch /mnt/mydisk/test_file2.txt
    [elizabeth@host-15 ~]$ ls /mnt/mydisk
    lost+found  test_file1.txt  test_file2.txt
    ```

7. Отмонтируйте диск и проверьте остались ли файлы

    ```bash
    [elizabeth@host-15 ~]$ sudo umount /mnt/mydisk
    [elizabeth@host-15 ~]$ ls /mnt/mydisk
    [elizabeth@host-15 ~]$ 
    ```

8. Сделайте так чтобы диск автоматически подключался при загрузке систем ( добавьте информацию о нём в fstab)

    ```bash
    [elizabeth@host-15 ~]$ sudo blkid /dev/sdb1
    /dev/sdb1: UUID="eb345b4b-1feb-47e7-87db-9e9c02dbd9b3" BLOCK_SIZE="4096" TYPE="ext4" PARTUUID="0c853c5a-01"
    [elizabeth@host-15 ~]$ sudo nano /etc/fstab
    ```

    В конец файла fstab была добавлена строка: UUID=eb345b4b-1feb-47e7-87db-9e9c02dbd9b3 /mnt/mydisk ext4 defaults 0 2

9. Проверьте корректность записанных в fstab данных перед перезагрузкой

    ```bash
    [elizabeth@host-15 ~]$ cat /etc/fstab
    proc		/proc			proc	nosuid,noexec,gid=proc				0 0
    devpts		/dev/pts		devpts  nosuid,noexec,gid=tty,mode=620,ptmxmode=0666	0 0
    tmpfs		/tmp			tmpfs	nosuid						0 0
    UUID=eb15519e-54bc-4441-b4a2-ccf21c83fdcf	/	ext4	relatime	1	1
    UUID=190B-4A85	/boot/efi	vfat	umask=0,quiet,showexec,iocharset=utf8,codepage=866	1	2
    UUID=58b6f079-0a64-46a8-ac65-d94a53e65351	swap	swap	defaults	0	0
    /dev/sr0	/media/ALTLinux	udf,iso9660	ro,noauto,user,utf8,nofail,comment=x-gvfs-show	0 0
    UUID=eb345b4b-1feb-47e7-87db-9e9c02dbd9b3 /mnt/mydisk ext4 defaults 0 2
    [elizabeth@host-15 ~]$ sudo mount -a
    [sudo] password for elizabeth:
    [elizabeth@host-15 ~]$ ls /mnt/mydisk/
    lost+found  test_file1.txt  test_file2.txt
    ```

10. Перезагрузите систему и убедитесь что диск был подключён к системе

    ```bash
    [elizabeth@host-15 ~]$ sudo reboot
    ```

    ```bash
    [elizabeth@host-15 ~]$ mount | grep sdb1
    /dev/sdb1 on /mnt/mydisk type ext4 (rw,relatime)
    [elizabeth@host-15 ~]$ ls -la /mnt/mydisk
    итого 24
    drwxr-xr-x 3 root root  4096 дек 23 11:31 .
    drwxr-xr-x 3 root root  4096 дек 22 22:22 ..
    drwx------ 2 root root 16384 дек 23 11:25 lost+found
    -rw-r--r-- 1 root root     0 дек 23 11:31 test_file1.txt
    -rw-r--r-- 1 root root     0 дек 23 11:31 test_file2.txt
    ```