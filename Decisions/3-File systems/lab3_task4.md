# Лабораторная работа 3

## Задание 4

1. Raid массивы, что такое икакие бывают

    Типы RAID:

    - RAID 0 (striping - чередование) - данные разбиваются на блоки и записываются на несколько дисков одновременно (повышение производительности, нет избыточности)

    - RAID 1 (mirroring - зеркалирование) - полное копирование данных на два или более диска (избыточность)

    - RAID 5 (чередование с контролем четности) - данные и контрольные суммы (parity) распределяются по всем дискам. Нужно минимум 3 диска

    - RAID 6 (двойной контроль четности) - как RAID 5, но с двумя наборами контрольных сумм. Нужно минимум 4 диска

    - RAID 10 - комбинация RAID 1 и RAID 0

2. Добавьте в виртуальную машину 2 диска, отформатируйте их в ext4

    Добавилось два новых диска (sdc, sdd):

    ```bash
    [elizabeth@host-15 ~]$ lsblk
    NAME   MAJ:MIN RM  SIZE RO TYPE MOUNTPOINTS
    sda      8:0    0   40G  0 disk 
    ├─sda1   8:1    0  511M  0 part /boot/efi
    ├─sda2   8:2    0  7,6G  0 part [SWAP]
    └─sda3   8:3    0 31,9G  0 part /
    sdb      8:16   0   10G  0 disk 
    └─sdb1   8:17   0   10G  0 part /mnt/mydisk
    sdc      8:32   0    8G  0 disk 
    sdd      8:48   0    8G  0 disk 
    sr0     11:0    1  6,4G  0 rom  
    ```

3. Создайте из них raid 0 массив

    ```bash
    [elizabeth@host-15 ~]$ cat /proc/mdstat
    Personalities : 
    unused devices: <none>
    [elizabeth@host-15 ~]$ sudo mdadm --create /dev/md0 --level=0 --raid-devices=2 /dev/sdc /dev/sdd
    mdadm: Defaulting to version 1.2 metadata
    mdadm: array /dev/md0 started.
    [elizabeth@host-15 ~]$ sudo mkfs.ext4 /dev/md0
    [sudo] password for elizabeth:
    mke2fs 1.47.1 (20-May-2024)
    Creating filesystem with 4189696 4k blocks and 1048576 inodes
    Filesystem UUID: 7ea7cd11-3512-4a18-a4f2-9dc3b4f29a5c
    Superblock backups stored on blocks: 
        32768, 98304, 163840, 229376, 294912, 819200, 884736, 1605632, 2654208, 
        4096000

    Allocating group tables: done                            
    Writing inode tables: done                            
    Creating journal (16384 blocks): done
    Writing superblocks and filesystem accounting information: done
    ```

4. Проверьте всё ли работает

    ```bash
    [elizabeth@host-15 ~]$ cat /proc/mdstat
    Personalities : [raid0] 
    md0 : active raid0 sdd[1] sdc[0]
        16758784 blocks super 1.2 512k chunks
        
    unused devices: <none>
    ```

    ```bash
    [elizabeth@host-15 ~]$ lsblk -f
    NAME   FSTYPE            FSVER            LABEL                       UUID                                 FSAVAIL FSUSE% MOUNTPOINTS
    sda                                                                                                                       
    ├─sda1 vfat              FAT32                                        190B-4A85                                           /boot/efi
    ├─sda2 swap              1                                            58b6f079-0a64-46a8-ac65-d94a53e65351                [SWAP]
    └─sda3 ext4              1.0                                          eb15519e-54bc-4441-b4a2-ccf21c83fdcf   18,6G    35% /
    sdb                                                                                                                       
    └─sdb1 ext4              1.0                                          eb345b4b-1feb-47e7-87db-9e9c02dbd9b3    9,2G     0% /mnt/mydisk
    sdc    linux_raid_member 1.2              host-15:0                   2f70a69d-caba-df44-4590-6c0159bc1852                
    └─md0  ext4              1.0                                          7ea7cd11-3512-4a18-a4f2-9dc3b4f29a5c                
    sdd    linux_raid_member 1.2              host-15:0                   2f70a69d-caba-df44-4590-6c0159bc1852                
    └─md0  ext4              1.0                                          7ea7cd11-3512-4a18-a4f2-9dc3b4f29a5c                
    sr0    iso9660           Joliet Extension ALT Workstation 11.1 x86_64 2025-08-20-13-33-04-00                              
    ```

5. Удалите raid0 и создайте raid1

    - Создание raid1:

    ```bash
    [elizabeth@host-15 ~]$ sudo mdadm --stop /dev/md0
    mdadm: stopped /dev/md0
    [elizabeth@host-15 ~]$ sudo mdadm --zero-superblock /dev/sdc /dev/sdd
    [elizabeth@host-15 ~]$ cat /proc/mdstat
    Personalities : [raid0] 
    unused devices: <none>
    ```

    ```bash
    [elizabeth@host-15 ~]$ sudo mdadm --create /dev/md1 --level=1 --raid-devices=2 /dev/sdc /dev/sdd
    To optimalize recovery speed, it is recommended to enable write-indent bitmap, do you want to enable it now? [y/N]? y
    mdadm: Note: this array has metadata at the start and
        may not be suitable as a boot device.  If you plan to
        store '/boot' on this device please ensure that
        your boot-loader understands md/v1.x metadata, or use
        --metadata=0.90
    Continue creating array [y/N]? y
    mdadm: Defaulting to version 1.2 metadata
    mdadm: array /dev/md1 started.
    ```

    - Проверка:

    ```bash
    [elizabeth@host-15 ~]$ cat /proc/mdstat
    Personalities : [raid0] [raid1] 
    md1 : active raid1 sdd[1] sdc[0]
        8379392 blocks super 1.2 [2/2] [UU]
        bitmap: 0/1 pages [0KB], 65536KB chunk

    unused devices: <none>
    ```

    ```bash
    [elizabeth@host-15 ~]$ lsblk -f
    NAME   FSTYPE            FSVER            LABEL                       UUID                                 FSAVAIL FSUSE% MOUNTPOINTS
    sda                                                                                                                       
    ├─sda1 vfat              FAT32                                        190B-4A85                                           /boot/efi
    ├─sda2 swap              1                                            58b6f079-0a64-46a8-ac65-d94a53e65351                [SWAP]
    └─sda3 ext4              1.0                                          eb15519e-54bc-4441-b4a2-ccf21c83fdcf   18,6G    35% /
    sdb                                                                                                                       
    └─sdb1 ext4              1.0                                          eb345b4b-1feb-47e7-87db-9e9c02dbd9b3    9,2G     0% /mnt/mydisk
    sdc    linux_raid_member 1.2              host-15:1                   e852d7b4-1312-b54f-0287-53fddce4a35c                
    └─md1  ext4              1.0                                          7ea7cd11-3512-4a18-a4f2-9dc3b4f29a5c                
    sdd    linux_raid_member 1.2              host-15:1                   e852d7b4-1312-b54f-0287-53fddce4a35c                
    └─md1  ext4              1.0                                          7ea7cd11-3512-4a18-a4f2-9dc3b4f29a5c                
    sr0    iso9660           Joliet Extension ALT Workstation 11.1 x86_64 2025-08-20-13-33-04-00                              
    ```

6. В чём между ними разница?

    - RAID 0: скорость чтения/записи выше, но при отказе одного диска теряются все данные
    - RAID 1: надежность выше (зеркалирование), но эффективная емкость = емкость одного диска

7. Есть ли файловые системы которые поддерживают raid массивы без стороннего ПО

    - Btrfs - поддерживает RAID 0, 1, 10 без mdadm
    - ZFS - расширенные RAID-функции (RAID-Z)

8. Можно ли создать raid массив во время установки системы?

    Да, большинство установщиков Linux (включая Alt Linux) позволяют создавать RAID массивы на этапе разметки диска через графический интерфейс или текстовый режим. Это даёт лучшую интеграцию и надёжность системы с самого начала.