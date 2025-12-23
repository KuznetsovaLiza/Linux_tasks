# Лабораторная работа 3

## Задание 1

1. Какие файловые системы вы знаете?

    Основные ФС в Linux:

    - Ext/Ext2/Ext3/Ext4 (основные для Linux)

    - XFS (для больших файлов)

    - Btrfs (с поддержкой снапшотов)

    - PROCFS - /proc (отображает состояние ядра и процессов)

    - SYSFS - /sys (интерфейс к структурам ядра)

    - TMPFS (временные файлы в RAM)

    - ZFS (продвинутые функции)

    - FAT32/NTFS (совместимость с Windows)

2. Как можно классифицировать файловые системы? в чём отличия?

    Классификация файловых систем:

    - По наличию журналирования:
        - Журналируемые - сохраняют историю операций в специальном журнале (Ext3, Ext4, XFS, Btrfs)
        - Нежурналируемые - не сохраняют логов операций (Ext, Ext2)

    - По типу и назначению:
        - Основные дисковые ФС: Extended File System (Ext) семейство (Ext, Ext2, Ext3, Ext4)
        - Другие дисковые ФС (XFS, Btrfs)
        - Специальные (виртуальные) ФС (VFS (абстрактный слой ядра), PROCFS (/proc), SYSFS (/sys), TMPFS)

    - По архитектуре:
        - С традиционной структурой каталогов (вложенные папки и файлы)
        - Виртуальные ФС (не имеющие физического носителя)

    - По способу хранения метаданных:
        - С полным журналированием (Ext3, Ext4 - записывают все изменения)
        - С журналированием только метаданных (XFS - записывает только изменения метаданных)

    Основные отличия:

    - Ext: нежурналируемая первая расширенная ФС для Linux

    - Ext2: нежурналируемая, максимальный размер файла - 2 ТБ, простая структура

    - Ext3: первая журналируемая в семействе Ext, восстановление данных после сбоев, запись логов операций

    - Ext4: убраны ограничения предыдущих версий, стабильная и надежная, по умолчанию в большинстве дистрибутивов

    - XFS: журналируемая, быстрая работа с большими файлами, можно увеличивать, но нельзя уменьшать разделы, риск потери данных при отключении питания

    - Btrfs: высокая отказоустойчивость, поддержка снапшотов, динамическое изменение размеров разделов, современная альтернатива Ext4

    - PROCFS (/proc): виртуальная ФС, отображает состояние ядра и процессов, содержит /sys для параметров sysctl

    - SYSFS (/sys): псевдофайловая система, интерфейс к структурам ядра, автоматически монтируется в /sys

    - TMPFS: располагается в оперативной памяти, для временных файлов, быстрый доступ, данные удаляются при перезагрузке

    - ZFS: продвинутая файловая система, диски объединяются в пулы, из которых выделяются файловые системы, данные никогда не перезаписываются (каждая операция создает новые блоки), поддержка снапшотов

    - FAT32: устаревшая файловая система от Microsoft, максимальная совместимость со всеми ОС и устройствами, ограничения (максимальный размер файла - 4 ГБ)

    - NTFS: современная файловая система Windows, поддержка в Linux через драйвер NTFS-3G, журналируемая с расширенными функциями, шифрование файлов

3. Какие файловые системы используются в linux?
    В Linux используются в основном: Ext/Ext2/Ext3/Ext4, XFS, Btrfs, ZFS, FAT32/NTFS, tmpfs

4. Как можно создать файловую систему на диске?
    Команды для создания файловых систем в Linux (синтаксис):
    - sudo mkfs [опции] -t <тип_фс> <устройство> [размер]
    - sudo mkfs.<тип_фс> [опции] <устройство> [размер]

5. Как можно подключить диск в систему, что такое монтирование?

    Монтирование — это процесс подключения файловой системы устройства (диска, раздела, USB-флешки) к определённой директории (точке монтирования) в дереве каталогов Linux. Без монтирования система "не видит" содержимое диска, даже если он физически подключен.

    Процесс подключения диска:

    - Определить диск и разделы
        - Посмотреть все диски

        ```bash
        lsblk
        ```

        - Посмотреть подробнее

        ```bash
        sudo fdisk -l /dev/sdb
        sudo blkid /dev/sdb1
        ```

        Первая команда покажет информация о разделах на конкретном диске
        
        Вторая покажет уникальный идентификатор файловой системы (UUID) и тип ФС

    - Создать точку монтирования
        - Создать директорию

        ```bash
        sudo mkdir /mnt/mydisk
        ```

        - Или в домашней директории

        ```bash
        mkdir ~/external_disk
        ```

    - Монтировать диск
        - Если тип ФС известен

        ```bash
        sudo mount /dev/sdb1 /mnt/mydisk
        ```
        - Если не известен (автоопределение)

        ```bash
        sudo mount -t auto /dev/sdb1 /mnt/mydisk
        ```

    - Проверить, что смонтировалось, посмотреть содержимое

    ```bash
    mount | grep sdb1
    ls -la /mnt/mydisk
    ```

6. Файловая система procfs, cifs, tpmfs, sysfs. В чём особенности каждой из них? Вывести каталоги к которым примонтированы эти файловые системы

    Особенности ФС:
    - procfs - информация о процессах и системе
    - sysfs - информация об устройствах ядра
    - tmpfs - временные файлы в ОЗУ
    - cifs - сетевые файловые системы Windows

    Вывод каталогов этих ФС:
    ```bash
    [elizabeth@host-15 ~]$ cat /proc/mounts | grep -E '(proc|sys|cifs|tmpfs)'
    udevfs /dev devtmpfs rw,relatime,size=5120k,nr_inodes=496165,mode=755,inode64 0 0
    runfs /run tmpfs rw,relatime,mode=755,inode64 0 0
    proc /proc proc rw,nosuid,noexec,relatime,gid=19 0 0
    sysfs /sys sysfs rw,nosuid,nodev,noexec,relatime 0 0
    securityfs /sys/kernel/security securityfs rw,nosuid,nodev,noexec,relatime 0 0
    tmpfs /dev/shm tmpfs rw,nosuid,nodev,inode64 0 0
    cgroup2 /sys/fs/cgroup cgroup2 rw,nosuid,nodev,noexec,relatime,nsdelegate,memory_recursiveprot 0 0
    pstore /sys/fs/pstore pstore rw,nosuid,nodev,noexec,relatime 0 0
    efivarfs /sys/firmware/efi/efivars efivarfs rw,nosuid,nodev,noexec,relatime 0 0
    bpf /sys/fs/bpf bpf rw,nosuid,nodev,noexec,relatime,mode=700 0 0
    systemd-1 /proc/sys/fs/binfmt_misc autofs rw,relatime,fd=30,pgrp=1,timeout=0,minproto=5,maxproto=5,direct,pipe_ino=3272 0 0
    debugfs /sys/kernel/debug debugfs rw,nosuid,nodev,noexec,relatime 0 0
    tracefs /sys/kernel/tracing tracefs rw,nosuid,nodev,noexec,relatime 0 0
    fusectl /sys/fs/fuse/connections fusectl rw,nosuid,nodev,noexec,relatime 0 0
    configfs /sys/kernel/config configfs rw,nosuid,nodev,noexec,relatime 0 0
    tmpfs /tmp tmpfs rw,nosuid,relatime,inode64 0 0
    tmpfs /run/user/1000 tmpfs rw,nosuid,nodev,relatime,size=399432k,nr_inodes=99858,mode=700,uid=1000,gid=1000,inode64 0 0
    ```

7. Как можно получить информацию о системе используя лишь команду cat? вывести иформацию о процессоре и состоянии памяти системы

    Команда cat может читать специальные файлы в Linux, которые содержат информацию о системе.

    Информация о процессоре:

    ```bash
    [elizabeth@host-15 ~]$ cat /proc/cpuinfo
    processor	: 0
    vendor_id	: GenuineIntel
    cpu family	: 6
    model		: 126
    model name	: Intel(R) Core(TM) i5-1035G1 CPU @ 1.00GHz
    stepping	: 5
    microcode	: 0xa0
    cpu MHz		: 1190.402
    cache size	: 6144 KB
    physical id	: 0
    siblings	: 2
    core id		: 0
    cpu cores	: 2
    apicid		: 0
    initial apicid	: 0
    fpu		: yes
    fpu_exception	: yes
    cpuid level	: 22
    wp		: yes
    flags		: fpu vme de pse tsc msr pae mce cx8 apic sep mtrr pge mca cmov pat pse36 clflush mmx fxsr sse sse2 ht syscall nx rdtscp lm constant_tsc rep_good nopl xtopology nonstop_tsc cpuid tsc_known_freq pni pclmulqdq ssse3 fma cx16 pcid sse4_1 sse4_2 x2apic movbe popcnt aes xsave avx f16c rdrand hypervisor lahf_lm abm 3dnowprefetch fsgsbase bmi1 avx2 bmi2 invpcid rdseed adx clflushopt sha_ni arat md_clear flush_l1d arch_capabilities
    bugs		: spectre_v1 spectre_v2 spec_store_bypass swapgs itlb_multihit srbds mmio_stale_data retbleed gds bhi its its_native_only
    bogomips	: 2380.80
    clflush size	: 64
    cache_alignment	: 64
    address sizes	: 39 bits physical, 48 bits virtual
    power management:

    processor	: 1
    vendor_id	: GenuineIntel
    cpu family	: 6
    model		: 126
    model name	: Intel(R) Core(TM) i5-1035G1 CPU @ 1.00GHz
    stepping	: 5
    microcode	: 0xa0
    cpu MHz		: 1190.402
    cache size	: 6144 KB
    physical id	: 0
    siblings	: 2
    core id		: 1
    cpu cores	: 2
    apicid		: 1
    initial apicid	: 1
    fpu		: yes
    fpu_exception	: yes
    cpuid level	: 22
    wp		: yes
    flags		: fpu vme de pse tsc msr pae mce cx8 apic sep mtrr pge mca cmov pat pse36 clflush mmx fxsr sse sse2 ht syscall nx rdtscp lm constant_tsc rep_good nopl xtopology nonstop_tsc cpuid tsc_known_freq pni pclmulqdq ssse3 fma cx16 pcid sse4_1 sse4_2 x2apic movbe popcnt aes xsave avx f16c rdrand hypervisor lahf_lm abm 3dnowprefetch fsgsbase bmi1 avx2 bmi2 invpcid rdseed adx clflushopt sha_ni arat md_clear flush_l1d arch_capabilities
    bugs		: spectre_v1 spectre_v2 spec_store_bypass swapgs itlb_multihit srbds mmio_stale_data retbleed gds bhi its its_native_only
    bogomips	: 2380.80
    clflush size	: 64
    cache_alignment	: 64
    address sizes	: 39 bits physical, 48 bits virtual
    power management:
    ```

    Информация о состоянии памяти системы:

    ```bash
    [elizabeth@host-15 ~]$ cat /proc/meminfo
    MemTotal:        3994352 kB
    MemFree:         1674616 kB
    MemAvailable:    2717216 kB
    Buffers:           36756 kB
    Cached:          1211780 kB
    SwapCached:            0 kB
    Active:           666244 kB
    Inactive:        1434228 kB
    Active(anon):       1820 kB
    Inactive(anon):   869696 kB
    Active(file):     664424 kB
    Inactive(file):   564532 kB
    Unevictable:           0 kB
    Mlocked:               0 kB
    SwapTotal:       7988220 kB
    SwapFree:        7988220 kB
    Zswap:                 0 kB
    Zswapped:              0 kB
    Dirty:                36 kB
    Writeback:             0 kB
    AnonPages:        849928 kB
    Mapped:           355460 kB
    Shmem:             19580 kB
    KReclaimable:      32680 kB
    Slab:              83384 kB
    SReclaimable:      32680 kB
    SUnreclaim:        50704 kB
    KernelStack:        9792 kB
    PageTables:        17152 kB
    SecPageTables:         0 kB
    NFS_Unstable:          0 kB
    Bounce:                0 kB
    WritebackTmp:          0 kB
    CommitLimit:     9985396 kB
    Committed_AS:    5414284 kB
    VmallocTotal:   34359738367 kB
    VmallocUsed:       28172 kB
    VmallocChunk:          0 kB
    Percpu:             1200 kB
    HardwareCorrupted:     0 kB
    AnonHugePages:    239616 kB
    ShmemHugePages:        0 kB
    ShmemPmdMapped:        0 kB
    FileHugePages:    151552 kB
    FilePmdMapped:     63488 kB
    CmaTotal:              0 kB
    CmaFree:               0 kB
    Unaccepted:            0 kB
    HugePages_Total:       0
    HugePages_Free:        0
    HugePages_Rsvd:        0
    HugePages_Surp:        0
    Hugepagesize:       2048 kB
    Hugetlb:               0 kB
    DirectMap4k:       91400 kB
    DirectMap2M:     4083712 kB
    ```
