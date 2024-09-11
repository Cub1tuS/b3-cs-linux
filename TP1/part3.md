# III. MAKE SERVICES GREAT AGAIN

## Sommaire

- [III. MAKE SERVICES GREAT AGAIN](#iii-make-services-great-again)
  - [Sommaire](#sommaire)
  - [1. Restart automatique](#1-restart-automatique)
  - [2. Utilisateur applicatif](#2-utilisateur-applicatif)
  - [3. Maîtrisez l'emplacement des fichiers](#3-maîtrisez-lemplacement-des-fichiers)
  - [4. Security hardening](#4-security-hardening)

## 1. Restart automatique

🌞 **Ajoutez une clause dans le fichier `efrei_server.service` pour le restart automatique**

```bash
Restart=always
```

🌞 **Testez que ça fonctionne**

```bash
ps -e | grep python
[...]
2186 ?        00:00:00 python3
# ui jé le code directement
[...]
```

```bash
sudo kill 2410
```

```bash
[toto@rocky ~]$ ps -e | grep python
   2461 ?        00:00:00 python3
```

## 2. Utilisateur applicatif

🌞 **Créer un utilisateur applicatif**

```bash
sudo useradd -d /var/lib/efrei_server/ -s /sbin/nologin efrei-user
```

🌞 **Modifier le service pour que ce nouvel utilisateur lance le programme `efrei_server`**

```bash
User=efrei-user
Group=efrei-user
```

🌞 **Vérifier que le programme s'exécute bien sous l'identité de ce nouvel utilisateur**

```bash
[toto@rocky sbin]$ ps -ef | grep efrei
efrei-u+    1574       1  0 11:27 ?        00:00:00 /bin/python3 /usr/local/bin/efrei_server/main.py
toto        1619    1373  0 11:35 pts/0    00:00:00 grep --color=auto efrei
# le nom du user était trop grand :(
```

## 3. Maîtrisez l'emplacement des fichiers

🌞 **Choisir l'emplacement du fichier de logs**

```bash
[toto@rocky tmp]$ mkdir /var/log/efrei_server
```

```bash
[toto@rocky tmp]$ cat /usr/local/bin/efrei_server/env 
LISTEN_ADDRESS=172.16.74.175
LOG_DIR=/var/log/efrei_server/
```

```bash
rm /tmp/server.log 
```

🌞 **Maîtriser les permissions du fichier de logs**

```bash
sudo chmod 700 efrei_server/
```

```bash
sudo chown efrei_user:efrei-user efrei_server/
``` 

## 4. Security hardening

Il existe beaucoup de clauses qu'on peut ajouter dans un fichier `.service` pour que *systemd* s'occupe de sécuriser le service, en l'isolant du reste du système par exemple.

Ainsi, une commande est fournie `systemd-analyze security` qui permet de voir quelles mesures de sécurité on a activé. Un score (un peu arbitraire) est attribué au *service* ; cela représente son "niveau de sécurité".

Cette commande est **très** pratique d'un point de vue pédagogique : elle va vous montrer toutes les clauses qu'on peut ajouter dans un `.service` pour renforcer sa sécurité.

🌞 **Modifier le `.service` pour augmenter son niveau de sécurité**

- ajoutez au moins 5 clauses dans le fichier pour augmenter le niveau de sécurité de l'application
- n'utilisez que des clauses que vous comprenez, useless sinon

🌟 **BONUS : Essayez d'avoir le score le plus haut avec `systemd-analyze security`**

➜ 💡💡💡 **A ce stade, vous pouvez ré-essayez l'injection que vous avez trouvé dans la partie 1. Normalement, on peut faire déjà moins de trucs avec.**

> ➜ [**Lien vers la partie 4**](./part4.md)