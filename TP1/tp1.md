# TP1 : Héberger un service

# Sommaire

- [TP1 : Héberger un service](#tp1--héberger-un-service)
- [Sommaire](#sommaire)
- [0. Prérequis](#0-prérequis)
- [Suite du TP](#suite-du-tp)

# 0. Prérequis

🌞 **Boom ça commence direct : je veux l'état initial du firewall**

```bash
[toto@rocky ~]$ sudo firewall-cmd --state
running

[toto@rocky ~]$ sudo firewall-cmd --list-all
public (active)
  target: default
  icmp-block-inversion: no
  interfaces: ens160 ens256
  sources: 
  services: ssh
[...]
```

➜ **Gestion d'utilisateurs**

🌞 **Fichiers /etc/sudoers /etc/passwd /etc/group** dans le dépôt de compte-rendu svp !

```bash
[toto@rocky ~]$ sudo cat /etc/sudoers

Defaults   !visiblepw

Defaults    always_set_home
Defaults    match_group_by_gid

Defaults    always_query_group_plugin

Defaults    env_reset
Defaults    env_keep =  "COLORS DISPLAY HOSTNAME HISTSIZE KDEDIR LS_COLORS"
Defaults    env_keep += "MAIL PS1 PS2 QTDIR USERNAME LANG LC_ADDRESS LC_CTYPE"
Defaults    env_keep += "LC_COLLATE LC_IDENTIFICATION LC_MEASUREMENT LC_MESSAGES"
Defaults    env_keep += "LC_MONETARY LC_NAME LC_NUMERIC LC_PAPER LC_TELEPHONE"
Defaults    env_keep += "LC_TIME LC_ALL LANGUAGE LINGUAS _XKB_CHARSET XAUTHORITY"

Defaults    secure_path = /sbin:/bin:/usr/sbin:/usr/bin

root	ALL=(ALL) 	ALL

%wheel	ALL=(ALL)	ALL
```

```bash
[toto@rocky ~]$ cat /etc/passwd 
root:x:0:0:root:/root:/bin/bash
bin:x:1:1:bin:/bin:/sbin/nologin
daemon:x:2:2:daemon:/sbin:/sbin/nologin
adm:x:3:4:adm:/var/adm:/sbin/nologin
lp:x:4:7:lp:/var/spool/lpd:/sbin/nologin
sync:x:5:0:sync:/sbin:/bin/sync
shutdown:x:6:0:shutdown:/sbin:/sbin/shutdown
halt:x:7:0:halt:/sbin:/sbin/halt
mail:x:8:12:mail:/var/spool/mail:/sbin/nologin
operator:x:11:0:operator:/root:/sbin/nologin
games:x:12:100:games:/usr/games:/sbin/nologin
ftp:x:14:50:FTP User:/var/ftp:/sbin/nologin
nobody:x:65534:65534:Kernel Overflow User:/:/sbin/nologin
systemd-coredump:x:999:997:systemd Core Dumper:/:/sbin/nologin
dbus:x:81:81:System message bus:/:/sbin/nologin
polkitd:x:998:996:User for polkitd:/:/sbin/nologin
tss:x:59:59:Account used for TPM access:/:/usr/sbin/nologin
sssd:x:997:994:User for sssd:/:/sbin/nologin
sshd:x:74:74:Privilege-separated SSH:/usr/share/empty.sshd:/sbin/nologin
chrony:x:996:993:chrony system user:/var/lib/chrony:/sbin/nologin
systemd-oom:x:991:991:systemd Userspace OOM Killer:/:/usr/sbin/nologin
toto:x:1000:1000:toto:/home/toto:/bin/bash
```

```bash
[toto@rocky ~]$ cat /etc/group
root:x:0:
bin:x:1:
daemon:x:2:
sys:x:3:
adm:x:4:
tty:x:5:
disk:x:6:
lp:x:7:
mem:x:8:
kmem:x:9:
wheel:x:10:toto
cdrom:x:11:
mail:x:12:
man:x:15:
dialout:x:18:
floppy:x:19:
games:x:20:
tape:x:33:
video:x:39:
ftp:x:50:
lock:x:54:
audio:x:63:
users:x:100:
nobody:x:65534:
utmp:x:22:
utempter:x:35:
input:x:999:
kvm:x:36:
render:x:998:
systemd-journal:x:190:
systemd-coredump:x:997:
dbus:x:81:
polkitd:x:996:
ssh_keys:x:995:
tss:x:59:
sssd:x:994:
sshd:x:74:
chrony:x:993:
sgx:x:992:
systemd-oom:x:991:
toto:x:1000:
```

# Suite du TP

Vous pouvez enchaîner sur les 3 parties du TP, dans l'ordre :

- [**Partie 1** : Host & Hack](./part1.md)
- [**Partie 2** : Servicer le programme](./part2.md)
- [**Partie 3** : MAKE SERVICES GREAT AGAIN](./part3.md)
- [**Partie 4** : Autour de l'application](./part4.md)
