# Лабораторная работа 7

## Задание 1

1. Какой по умолчанию используется порт для подключения?

    По умолчанию используется порт 22

2. Можно ли его изменить? если да то как?

    - Можно в файле конфигурации изменить строчку Port:

        ```bash
        sudo nano /etc/ssh/sshd_config
        ```

    - Затем перезагрузить службу:

        ```bash
        sudo systemctl restart sshd
        ```

3. Какая служба отвечает за обработку запросов на подключения по ssh?

    Служба sshd (OpenSSH Daemon)

4. Какой файл конфигурации отвечает за его настройку?

    /etc/ssh/sshd_config

5. Попробуйте подключиться по ssh к предоставленному вам серверу

    ```bash
    [elizabeth@host-15 ~]$ ssh student@ternar.io -p 224
    The authenticity of host '[ternar.io]:224 ([95.31.204.147]:224)' can't be established.
    ED25519 key fingerprint is SHA256:qQvhr2jX4W5vId+xaLLzMh2PYHYj/Oba062vA5O0fzY.
    This key is not known by any other names.
    Are you sure you want to continue connecting (yes/no/[fingerprint])? yes
    Warning: Permanently added '[ternar.io]:224' (ED25519) to the list of known hosts.
    student@ternar.io's password: 
    Last login: Tue Nov 25 10:55:44 2025
    [student@S-vm-224 ~]$ 
    ```

6. Отредактируйте файл настроек на сервере так, чтобы была возможность подключиться к серверу используя пользователя root

    ```bash
    [student@S-vm-224 ~]$ sudo vi /etc/openssh/sshd_config
    [student@S-vm-224 ~]$ sudo systemctl restart sshd
    ```

    - В файле sshd_config была изменена строка: PermitRootLogin yes

    - Проверка настроек в файле конфигурации:

        ```bash
        [student@S-vm-224 ~]$ sudo grep -i "permitroot" /etc/openssh/sshd_config
        PermitRootLogin yes
        # the setting of "PermitRootLogin prohibit-password".
        ```

        - PermitRootLogin yes - активная строка
        - Комментарий ниже объясняет альтернативную настройку

7. Измените количество ошибок ввода пароля перед сбросом соединения, покажите эти изменения

    ```bash
    [student@S-vm-224 ~]$ sudo vi /etc/openssh/sshd_config
    [student@S-vm-224 ~]$ sudo systemctl restart sshd
    ```

    - В файле sshd_config была изменена строка: MaxAuthTries 3 (устанавливает максимальное количество неудачных попыток аутентификации)

    - Проверка настроек:

        ```bash
        [student@S-vm-224 ~]$ sudo grep MaxAuthTries /etc/openssh/sshd_config
        MaxAuthTries 3
        ```

8. Создайте пользователя ssh-user и попробуйте им подключиться к серверу

    ```bash
    [student@S-vm-224 ~]$ sudo useradd -m ssh-user
    [student@S-vm-224 ~]$ sudo passwd ssh-user
    passwd: updating all authentication tokens for user ssh-user.

    You can now choose the new password or passphrase.

    A valid password should be a mix of upper and lower case letters, digits, and
    other characters.  You can use a password containing at least 7 characters
    from all of these classes, or a password containing at least 8 characters
    from just 3 of these 4 classes.
    An upper case letter that begins the password and a digit that ends it do not
    count towards the number of character classes used.

    A passphrase should be of at least 3 words, 11 to 72 characters long, and
    contain enough different characters.

    Alternatively, if no one else can see your terminal now, you can pick this as
    your password: "Shout*sketch!all".

    Enter new password: 
    Weak password: based on personal login information.
    Re-type new password: 
    passwd: all authentication tokens updated successfully.
    ```

    ```bash
    [elizabeth@host-15 ~]$ ssh ssh-user@ternar.io -p 224
    ssh-user@ternar.io's password: 
    Last login: Sat Dec 27 14:36:33 2025 from 192.168.4.1
    [ssh-user@S-vm-224 ~]$ 
    ```

9. Ограничте ему возможность подключения к серверу

    ```bash
    [student@S-vm-224 ~]$ sudo vi /etc/openssh/sshd_config
    [student@S-vm-224 ~]$ sudo systemctl restart sshd
    ```

    ```bash
    [elizabeth@host-15 ~]$ ssh ssh-user@ternar.io -p 224
    ssh-user@ternar.io's password: 
    ssh: Permission denied, please try again.
    ssh-user@ternar.io's password: 
    ssh: Received disconnect from 95.31.204.147 port 224:2: Too many authentication failures
    Disconnected from 95.31.204.147 port 224
    ```

10. Как вы это сделали?

    В файл /etc/openssh/sshd_config была добавлена строка:  
    DenyUsers ssh-user (пользователь ssh-user добавлен в список DenyUsers, что и означает запрет на подключение)

11. Что хранится в файле known_hosts?

    В файле known_hosts хранятся отпечатки ключей (fingerprints) SSH-серверов, к которым вы подключались. Он нужен для защиты от атак типа "man-in-the-middle" (MITM).