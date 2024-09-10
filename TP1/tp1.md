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

[Les fichiers sont ici](../files)

# Suite du TP

Vous pouvez enchaîner sur les 3 parties du TP, dans l'ordre :

- [**Partie 1** : Host & Hack](./part1.md)
- [**Partie 2** : Servicer le programme](./part2.md)
- [**Partie 3** : MAKE SERVICES GREAT AGAIN](./part3.md)
- [**Partie 4** : Autour de l'application](./part4.md)
