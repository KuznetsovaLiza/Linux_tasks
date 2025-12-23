# Лабораторная работа 3

## Задание 2

1. Какая структура каталогов в linux? Выведите список файлов в корне системы

    - Структура каталогов в Linux:

    /  
    ├── bin/  
    ├── boot/  
    ├── dev/  
    ├── etc/  
    ├── home/  
    ├── lib/  
    ├── media/  
    ├── mnt/  
    ├── opt/  
    ├── root/  
    ├── sbin/  
    ├── srv/  
    ├── tmp/  
    ├── usr/  
    │   ├── bin/  
    │   ├── sbin/  
    │   ├── lib/  
    │   └── local/  
    └── var/  
    │   ├── cache/  
    │   ├── log/  
    │   ├── spool/  
    │   └── tmp/

    - Список файлов в корне:

    ```bash
    [elizabeth@host-15 ~]$ ls -la /
    итого 76
    drwxr-xr-x  19 root root  4096 сен  9 17:12 .
    drwxr-xr-x  19 root root  4096 сен  9 17:12 ..
    lrwxrwxrwx   1 root root     7 апр  6  2024 bin -> usr/bin
    drwx------   4 root root  4096 сен  9 17:00 boot
    drwxr-xr-x  20 root root  3900 дек 22 19:45 dev
    drwxr-xr-x 149 root root 12288 дек 22 19:45 etc
    drwxr-xr-x   5 root root  4096 дек 21 20:35 home
    lrwxrwxrwx   1 root root     7 апр  6  2024 lib -> usr/lib
    lrwxrwxrwx   1 root root     9 апр  6  2024 lib64 -> usr/lib64
    lrwxrwxrwx   1 root root    10 апр  6  2024 libx32 -> usr/libx32
    drwx------   2 root root 16384 сен  9 16:46 lost+found
    drwxr-xr-x   3 root root  4096 сен  9 17:12 media
    drwxr-xr-x   2 root root  4096 апр  6  2024 mnt
    drwxr-xr-x   2 root root  4096 апр  6  2024 opt
    dr-xr-xr-x 259 root root     0 дек 22 19:45 proc
    drwx------   7 root root  4096 дек 21 20:37 root
    drwxr-xr-x  50 root root  1220 дек 22 19:47 run
    lrwxrwxrwx   1 root root     8 апр  6  2024 sbin -> usr/sbin
    dr-xr-xr-x   2 root root  4096 апр  6  2024 selinux
    drwxr-xr-x   2 root root  4096 апр  6  2024 srv
    dr-xr-xr-x  13 root root     0 дек 22 19:45 sys
    drwxrwxrwt  18 root root   440 дек 22 19:55 tmp
    drwxr-xr-x  14 root root  4096 сен  9 16:46 usr
    drwxr-xr-x  17 root root  4096 сен  9 17:12 var
    ```

2. Где хранятся папки пользователей в системе?

    Папки пользователей в системе:

    ```bash
    [elizabeth@host-15 ~]$ ls /home/
    elizabeth  user1  user2
    ```

3. Где домашняя папка суперпользователя?

    Домашняя папка root:

    ```bash
    [elizabeth@host-15 ~]$ echo $HOME
    /home/elizabeth
    ```

4. Где хранятся основные конфигурационные файлы в системе?

    Основные конфигурации:

    ```bash
    [elizabeth@host-15 ~]$ ls /etc/
    adjtime                 dnsmasq.conf.d        initrd.mk            net                    rc.d                 sudo.conf
    alterator               dnsmasq.d             initrd.mk.d          netconfig              reader.conf.d        sudoers
    alternatives            dvdauthor.conf        inputrc              NetworkManager         realmd.conf          sudoers.d
    altlinux-release        e2scrub.conf          inxi.conf            nfs.conf               rearj.cfg            sysconfig
    apf                     eac                   ipp-usb              nsswitch.conf          redhat-release       sysctl.conf
    apt                     egl                   iproute2             nsswitch.conf.bak      request-key.conf     sysctl.d
    at.deny                 environment           java                 nsswitch.conf.rpmorig  request-key.d        sysfs.conf
    audit                   exports               jvm                  nvme                   resolv.conf          sysfs.d
    auto.avahi              fedora-release        jvm-common           openal                 resolv.conf.bak      syslog.d
    autofs.conf             filesystems           kernel               OpenCL                 resolvconf.conf      systemd
    auto.master             firsttime.d           keys                 openldap               resolv.conf.dnsmasq  system-release
    auto.smb                flexiblasrc           krb5.conf            opensc.conf            role                 tcb
    auto.tab                flexiblasrc.d         krb5.conf.d          openssh                role.d               timeshift
    avahi                   fonts                 ld.so.cache          openssl                rpc                  tmpfiles.d
    bash_completion.d       fprintd.conf          ld.so.conf           openvpn                rpm                  tpm2-tss
    bashrc                  fstab                 ld.so.conf.d         opt                    rygel.conf           ts.conf
    bashrc.d                fuse.conf             LexmarkZ11           os-release             samba                udev
    beesu.conf              fwupd                 lftp.conf            PackageKit             sane.d               udisks2
    bindresvport.blacklist  gai.conf              libao.conf           pam.d                  sasl2                UPower
    binfmt.d                gdm                   libaudit.conf        paperspecs             screencap            urlview
    bluetooth               geoclue               libblockdev          passwd                 screenrc             usbguard
    buildreqs               glvnd                 libinput             passwd-                scsi_id.config       usb_modeswitch.conf
    chromium                gnome-remote-desktop  libnl                passwdqc.conf          securetty            vconsole.conf
    chrony.conf             gnupg                 libnvidia32current   perl5                  security             vdpau_wrapper.cfg
    chrony.keys             GREP_COLORS           libnvidiacurrent     pipewire               selinux              vim
    chroot.d                groff                 libssh               pkcs11                 sensors3.conf        vmware-tools
    control.d               group                 libwacom             pki                    sensors.d            vpnc
    cracklib                group-                locale.conf          plymouth               services             vulkan
    credstore               grub.cfg              local-policy         pnm2ppa.conf           sgml                 warnquota.conf
    credstore.encrypted     grub.d                local-policy-system  polkit-1               shadow               wgetrc
    cron.d                  gshadow               localtime            ppp                    shadow-              wpa_supplicant
    cron.daily              gshadow-              login.defs           printcap               shadow-maint         wpa_supplicant.conf
    cron.deny               gss                   logrotate.conf       profile                shells               X11
    cron.hourly             gtk-2.0               logrotate.d          profile.d              sisyphus-updates     xattr.conf
    cron.monthly            gtk-3.0               lvm                  protocols              skel                 xdg
    crontab                 gtk-4.0               machine-id           pulse                  skel.be_BY.CP1251    xl2tpd
    crontab.template        hooks                 magic                qemu                   skel.de_DE           xml
    cron.weekly             host.conf             man_db.conf          qt5                    skel.de_DE@euro      zlerc
    cups                    hostname              mc                   qt6                    skel.fr_FR           zlogout
    cupshelpers             hosts                 mdadm.conf.sample    quotagrpadmins         skel.fr_FR@euro      zprofile
    dbus-1                  hosts.allow           mke2fs.conf          quotatab               skel.ru_RU.CP1251    zshenv
    dconf                   hosts.deny            ModemManager         rc0.d                  skel.ru_RU.KOI8-R    zshrc
    default                 hp                    modprobe.d           rc1.d                  skel.uk_UA.CP1251    zshrc.d
    default-environment     httpd2                modules              rc2.d                  skel.uk_UA.KOI8-U
    depmod.d                idmapd.conf           modules-load.d       rc3.d                  smartd.conf
    dhcpcd.conf             info-dir              motd                 rc4.d                  smartd_warning.sh
    DIR_COLORS              init.d                mtab                 rc5.d                  speech-dispatcher
    dnsmasq.conf            initlog.conf          nanorc               rc6.d                  strongswan
    ```

5. Что за папки /bin, /sbin, usr/sbin, /usr/sbin

    - /bin - важные команды, доступные всем пользователям, необходимые для работы системы в однопользовательском режиме
    - /sbin - команды для системного администрирования, требующие прав root
    - /usr/bin - большинство пользовательских команд, установленных через пакетный менеджер
    - /usr/sbin - системные команды для администратора, которые не требуются для минимальной загрузки (из пакетов)