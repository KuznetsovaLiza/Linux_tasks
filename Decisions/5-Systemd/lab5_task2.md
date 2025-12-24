# Лабораторная работа 5

## Задание 2

1. Создайте скрипт который создаёт папку заполняет её файлами ( имена 1-4 ) и записывает в них информацию о текущей дате, версии ядра, имени компьютера и списке всех файлов в домашнем каталоге пользователя от которого выполняется скрипт( не забудьте сделать проверку на существование файлов и папок)

    - Скрипт:

    ```bash
    #!/bin/bash

    # Переходим в домашнюю директорию пользователя
    cd ~

    FOLDER_NAME="systemd_task_folder"

    if [ ! -d "$FOLDER_NAME" ]; then
        mkdir "$FOLDER_NAME"
        echo "Папка $FOLDER_NAME создана"
    else
        echo "Папка $FOLDER_NAME уже существует"
    fi

    cd "$FOLDER_NAME"

    for i in {1..4}; do
        if [ ! -f "$i.txt" ]; then
            {
                echo "=== Информация в файле $i.txt ==="
                echo "Дата и время: $(date)"
                echo "Версия ядра: $(uname -r)"
                echo "Имя компьютера: $(hostname)"
                echo "Список файлов в домашнем каталоге:"
                ls -la ~/
                echo "=== Конец информации ==="
            } > "$i.txt"
            echo "Файл $i.txt создан"
        else
            echo "Файл $i.txt уже существует"
        fi
    done
    ```

    - Создание и работа файла со скриптом:

    ```bash
    [elizabeth@host-15 ~]$ vim create_files.sh
    [elizabeth@host-15 ~]$ chmod +x create_files.sh
    [elizabeth@host-15 ~]$ ./create_files.sh
    Папка systemd_task_folder создана
    Файл 1.txt создан
    Файл 2.txt создан
    Файл 3.txt создан
    Файл 4.txt создан
    ```

2. Создайте юнит который будет вызывать этот скрипт при запуске. Проверьте

    - Содержимое юнита:

    ```bash
    [Unit]
    Description=Create files with system information

    [Service]
    Type=oneshot
    ExecStart=/home/elizabeth/create_files.sh
    User=elizabeth
    WorkingDirectory=/home/elizabeth

    [Install]
    WantedBy=multi-user.target
    ```

    - Создание юнита:

    ```bash
    [elizabeth@host-15 ~]$ sudo vim /etc/systemd/system/create-files.service
    ```

    - Проверка:

    ```bash
    [elizabeth@host-15 ~]$ sudo systemctl daemon-reload
    [elizabeth@host-15 ~]$ sudo systemctl start create-files.service
    [elizabeth@host-15 ~]$ sudo systemctl status create-files.service
    ○ create-files.service - Create files with system information
        Loaded: loaded (/etc/systemd/system/create-files.service; disabled; preset: disabled)
        Active: inactive (dead)

    дек 24 15:01:58 host-15 systemd[1]: create-files.service: Failed with result 'exit-code'.
    дек 24 15:01:58 host-15 systemd[1]: Failed to start create-files.service - Create files with system information.
    дек 24 15:26:01 host-15 systemd[1]: Starting create-files.service - Create files with system information...
    дек 24 15:26:01 host-15 create_files.sh[4252]: Папка systemd_task_folder уже существует
    дек 24 15:26:01 host-15 create_files.sh[4252]: Файл 1.txt уже существует
    дек 24 15:26:01 host-15 create_files.sh[4252]: Файл 2.txt уже существует
    дек 24 15:26:01 host-15 create_files.sh[4252]: Файл 3.txt уже существует
    дек 24 15:26:01 host-15 create_files.sh[4252]: Файл 4.txt уже существует
    дек 24 15:26:01 host-15 systemd[1]: create-files.service: Deactivated successfully.
    дек 24 15:26:01 host-15 systemd[1]: Finished create-files.service - Create files with system information.
    [elizabeth@host-15 ~]$ ls -la systemd_task_folder/
    итого 24
    drwxr-xr-x  2 elizabeth elizabeth 4096 дек 24 13:44 .
    drwx------ 18 elizabeth elizabeth 4096 дек 24 13:44 ..
    -rw-r--r--  1 elizabeth elizabeth 2667 дек 24 13:44 1.txt
    -rw-r--r--  1 elizabeth elizabeth 2667 дек 24 13:44 2.txt
    -rw-r--r--  1 elizabeth elizabeth 2667 дек 24 13:44 3.txt
    -rw-r--r--  1 elizabeth elizabeth 2667 дек 24 13:44 4.txt
    ```

3. Создайте таймер который будет вызывать выполнение одноимённого systemd юнита каждые 5 минут.

    - Содержимое юнита:

    ```bash
    [Unit]
    Description=Run create-files every 5 minutes

    [Timer]
    OnBootSec=5min
    OnUnitActiveSec=5min

    [Install]
    WantedBy=timers.target
    ```

    - Создание юнита:

    ```bash
    [elizabeth@host-15 ~]$ sudo vim /etc/systemd/system/create-files.timer
    ```

    - Активируем таймер:

        ```bash
        [elizabeth@host-15 ~]$ sudo systemctl daemon-reload
        [elizabeth@host-15 ~]$ sudo systemctl enable create-files.timer
        Created symlink /etc/systemd/system/timers.target.wants/create-files.timer → /etc/systemd/system/create-files.timer.
        [elizabeth@host-15 ~]$ sudo systemctl start create-files.timer
        [elizabeth@host-15 ~]$ sudo systemctl list-timers
        NEXT                            LEFT LAST                              PASSED UNIT                         ACTIVATES                >
        Wed 2025-12-24 16:15:07 +04 4min 44s Wed 2025-12-24 16:10:07 +04      15s ago create-files.timer           create-files.service
        Thu 2025-12-25 13:33:19 +04      21h Wed 2025-12-24 13:33:19 +04 2h 37min ago systemd-tmpfiles-clean.timer systemd-tmpfiles-clean.se>
        Mon 2025-12-29 00:09:31 +04   4 days Mon 2025-12-22 21:18:10 +04            - fstrim.timer                 fstrim.service
        -                                  - Wed 2025-12-24 13:19:47 +04 2h 50min ago mdadm-last-resort@md1.timer  mdadm-last-resort@md1.ser>

        4 timers listed.
        ```

        - Вывод команды sudo systemctl list-timers:
            - NEXT — когда таймер сработает в следующий раз
            - LEFT — сколько осталось до следующего запуска
            - LAST — когда таймер срабатывал в последний раз
            - PASSED — сколько времени прошло с последнего запуска
            - UNIT — имя таймера
            - ACTIVATES — какой сервис/юнит запускается

4. От какого пользователя вызываются юниты по умолчанию?

    По умолчанию юниты выполняются от пользователя root, если не указано иное в секции [Service] с директивой User=.

5. Создайте пользователя от имени которого будет выполняться ваш скрипт.

    ```bash
    [elizabeth@host-15 ~]$ sudo useradd -m -s /bin/bash scriptuser
    [sudo] password for elizabeth: 
    [elizabeth@host-15 ~]$ sudo passwd scriptuser
    passwd: updating all authentication tokens for user scriptuser.

    You can now choose the new password or passphrase.

    A valid password should be a mix of upper and lower case letters, digits, and
    other characters.  You can use a password containing at least 4 characters
    from at least 3 of these 4 classes.
    An upper case letter that begins the password and a digit that ends it do not
    count towards the number of character classes used.

    A passphrase should be of at least 3 words, 6 to 72 characters long, and
    contain enough different characters.

    Alternatively, if no one else can see your terminal now, you can pick this as
    your password: "Fake4profit3Isaac".

    Enter new password: 
    Re-type new password: 
    passwd: all authentication tokens updated successfully.
    ```

6. Дополните юнит информацией о пользователе от которого должен выполняться скрипт.

    - Новое содержимое юнита:

    ```bash
    [Unit]
    Description=Create files with system information

    [Service]
    Type=oneshot
    ExecStart=/home/scriptuser/create_files.sh
    User=scriptuser
    WorkingDirectory=/home/scriptuser

    [Install]
    WantedBy=multi-user.target
    ```

    - Изменение юнита:

    ```bash
    [elizabeth@host-15 ~]$ sudo vim /etc/systemd/system/create-files.service
    [sudo] password for elizabeth:
    [elizabeth@host-15 ~]$ sudo cp create_files.sh /home/scriptuser/
    [elizabeth@host-15 ~]$ sudo chown scriptuser:scriptuser /home/scriptuser/create_files.sh
    [elizabeth@host-15 ~]$ sudo chmod +x /home/scriptuser/create_files.sh
    ```

    - Проверка:

    ```bash
    [elizabeth@host-15 ~]$ sudo systemctl daemon-reload
    [elizabeth@host-15 ~]$ sudo systemctl start create-files.service
    [elizabeth@host-15 ~]$ sudo systemctl status create-files.service
    ○ create-files.service - Create files with system information
        Loaded: loaded (/etc/systemd/system/create-files.service; disabled; preset: disabled)
        Active: inactive (dead) since Wed 2025-12-24 16:51:40 +04; 21s ago
    TriggeredBy: ● create-files.timer
        Process: 5116 ExecStart=/home/scriptuser/create_files.sh (code=exited, status=0/SUCCESS)
    Main PID: 5116 (code=exited, status=0/SUCCESS)
            CPU: 122ms

    дек 24 16:51:40 host-15 systemd[1]: Starting create-files.service - Create files with system information...
    дек 24 16:51:40 host-15 create_files.sh[5116]: Папка systemd_task_folder создана
    дек 24 16:51:40 host-15 create_files.sh[5116]: Файл 1.txt создан
    дек 24 16:51:40 host-15 create_files.sh[5116]: Файл 2.txt создан
    дек 24 16:51:40 host-15 create_files.sh[5116]: Файл 3.txt создан
    дек 24 16:51:40 host-15 create_files.sh[5116]: Файл 4.txt создан
    дек 24 16:51:40 host-15 systemd[1]: create-files.service: Deactivated successfully.
    дек 24 16:51:40 host-15 systemd[1]: Finished create-files.service - Create files with system information.
    ```

7. Дополните ваш скрипт так, что бы он независимо от местоположения всегда выполнялся в домашней папке того кто его вызывает.

    Исходный скрипт удовлетворяет требованию:

    ```bash
    #!/bin/bash

    cd ~

    FOLDER_NAME="systemd_task_folder"

    if [ ! -d "$FOLDER_NAME" ]; then
        mkdir "$FOLDER_NAME"
        echo "Папка $FOLDER_NAME создана"
    else
        echo "Папка $FOLDER_NAME уже существует"
    fi

    cd "$FOLDER_NAME"

    for i in {1..4}; do
        if [ ! -f "$i.txt" ]; then
            {
                echo "=== Информация в файле $i.txt ==="
                echo "Дата и время: $(date)"
                echo "Версия ядра: $(uname -r)"
                echo "Имя компьютера: $(hostname)"
                echo "Список файлов в домашнем каталоге:"
                ls -la ~/
                echo "=== Конец информации ==="
            } > "$i.txt"
            echo "Файл $i.txt создан"
        else
            echo "Файл $i.txt уже существует"
        fi
    done
    ```