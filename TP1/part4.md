# Partie 4 : Autour de l'application

## Sommaire

- [Partie 4 : Autour de l'application](#partie-4--autour-de-lapplication)
  - [Sommaire](#sommaire)
  - [1. Firewalling](#1-firewalling)
  - [2. Protéger l'app contre le flood](#2-protéger-lapp-contre-le-flood)
  - [3. Empêcher le programme de faire des actions indésirables](#3-empêcher-le-programme-de-faire-des-actions-indésirables)

## 1. Firewalling


🌞 **Configurer de façon robuste le firewall**

```bash
[toto@rocky ~]$ sudo firewall-cmd --permanent --policy myOutputPolicy --add-ingress-zone HOST
success
[toto@rocky ~]$ sudo firewall-cmd --permanent --policy myOutputPolicy --add-egress-zone ANY
success
[toto@rocky ~]$ sudo firewall-cmd --permanent --policy myOutputPolicy --set-target DROP
success
[toto@rocky ~]$ sudo firewall-cmd --reload
success
```

🌞 **Prouver que la configuration est effective**

```bash
[toto@rocky ~]$ sudo dnf update
[sudo] password for toto: 
Rocky Linux 9 - BaseOS                                                                                       0.0  B/s |   0  B     00:00    
Errors during downloading metadata for repository 'baseos':
  - Curl error (6): Couldn't resolve host name for https://mirrors.rockylinux.org/mirrorlist?arch=aarch64&repo=BaseOS-9 [Could not resolve host: mirrors.rockylinux.org]
Error: Failed to download metadata for repo 'baseos': Cannot prepare internal mirrorlist: Curl error (6): Couldn't resolve host name for https://mirrors.rockylinux.org/mirrorlist?arch=aarch64&repo=BaseOS-9 [Could not resolve host: mirrors.rockylinux.org]
```

```bash
❯ ssh rocky
Last login: Fri Sep 13 16:57:56 2024 from 172.16.74.1
[toto@rocky ~]$ 
```

```bash
❯ ping 172.16.74.175
PING 172.16.74.175 (172.16.74.175): 56 data bytes
92 bytes from 172.16.74.175: Communication prohibited by filter
Vr HL TOS  Len   ID Flg  off TTL Pro  cks      Src      Dst
 4  5  00 5400 93d8   0 0000  40  01 f9ff 172.16.74.1  172.16.74.175 
```

## 2. Protéger l'app contre le flood

- dès qu'un client se connecte à notre service, une ligne de log est ajouté au fichier de log
- cette ligne de log contient l'IP du client qui s'est connecté
- si un client se connecte + de 5 fois en moins de 10 secondes par exemple) on peut estimer que c'est du flood (tentative de DOS ?)
- il faudrait blacklister automatiquement l'IP de ce client dans le firewall
- fail2ban fait exactement ça

🌞 **Installer fail2ban sur la machine**

```bash
sudo dnf install epel-release -y
```

```bash
sudo dnf install -y fail2ban-server fail2ban-firewalld
```

🌞 **Ajouter une *jail* fail2ban**

> Création du filtre

```bash
[toto@rocky filter.d]$ cat efrei_server.conf
[Definition]
failregex = \[\d+\.\d+\] Received '.*' from \('<HOST>', \d+\)
ignoreregex =
```

> Création d'une jail

```bash
[toto@rocky jail.d]$ cat efrei_server.conf 
[efrei_server]
enabled  = true
port     = 8888
filter   = /etc/fail2ban/filter.d/efrei_server
logpath  = /var/log/efrei_server/server.log
maxretry = 6
bantime  = 600
findtime = 10
```

- elle doit lire le fichier de log du service, que vous avez normalement placé dans `/var/log/`
- repérer la ligne de connexion d'un client
- blacklist à l'aide du firewall l'IP de ce client

🌞 **Vérifier que ça fonctionne !**

- faites-vous ban ! En faisant plein de connexions rapprochées avec le client
- constatez que le ban est effectif
- levez le ban (il y a une commande pour lever un ban qu'a réalisé fail2ban)

## 3. Empêcher le programme de faire des actions indésirables

Lors de son fonctionnement, un programme peut être amené à exécuter des **appels système** (ou *syscalls*) en anglais.  
Un programme **doit** exécuter un *syscall* dès qu'il veut interagir avec une ressource du système. Par exemple :

- lire/modifier un fichier
- établir une connexion réseau
- écouter sur un port
- changer les droits d'un fichier
- obtenir la liste des processus
- lancer un nouveau processus
- etc.

➜ **Exécuter un *syscall* c'est demander au kernel de faire quelque chose.**

Ainsi, par exemple, quand on exécute la commande `cat` sur un fichier pour lire son contenu, **la commande `cat` va exécuter (entre autres) le *syscall* `open` afin de pouvoir ouvrir et lire le fichier**.

> Il se passe la même chose quand genre t'utilises Discord, et t'envoies un fichier à un pote. L'application Discord va exécuter un *syscall* pour obtenir le contenu du fichier, et l'envoyer sur le réseau.

Si le programme est exécuté par **un utilisateur qui a les droits sur ce fichier, alors le kernel autorisera ce *syscall*** et le programme `cat` pourra accéder au contenu du fichier sans erreur, et l'afficher dans le terminal.

> Dit autrement : n'importe quel programme qui accède au contenu d'un fichie (par exemple) exécute **forcément** un *syscall* pour obtenir le contenu de ce fichier. Peu importe l'OS, c'est un truc commun à tous.

➜ ***seccomp* est un outil qui permet de filtrer les *syscalls* qu'a le droit d'exécuter un programme**

On définit une liste des *syscalls* que le programme a le droit de faire, les autres seront bloqués.

> Par exemple, un *syscall* sensible est `fork()` qui permet de créer un nouveau processus.

Dans notre cas, avec notre ptit *service*, c'est un des problèmes :

- vous injectez du code dans l'application en tant que vilain hacker
- pour exécuter des programmes comme `cat` ou autres
- à chaque commande exécutée avec l'injection, un *syscall* est exécuté par le programme serveur pour demander la création d'un nouveau processus (votre injection)
- on pourrait bloquer totalement ce comportement : empêcher le *service* de lancer un autre processus que `efrei_server`

🌞 **Ajouter une politique seccomp au fichier `.service`**

- la politique doit être la plus restrictive possible
- c'est à dire que juste le strict minimum des *syscalls* nécessaires doit être autorisé

![seccomp](./img/exploit_seccomp.png)