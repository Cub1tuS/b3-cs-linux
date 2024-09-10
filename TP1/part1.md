# I. Partie 1 : Host & Hack

## Sommaire

- [I. Partie 1 : Host \& Hack](#i-partie-1--host--hack)
  - [Sommaire](#sommaire)
  - [1. A vos marques](#1-a-vos-marques)
  - [2. Prêts](#2-prêts)
  - [3. Hackez](#3-hackez)

## 1. A vos marques

🌞 **Télécharger l'application depuis votre VM**

```bash
[toto@rocky ~]$ wget https://gitlab.com/it4lik/b3-csec-2024/-/raw/main/efrei_server
```

🌞 **Lancer l'application `efrei_server`**

```bash
[toto@rocky ~]$ python main.py 
```

```bash
[toto@rocky ~]$ export LISTEN_ADDRESS=172.16.74.175
```

🌞 **Prouvez que l'application écoute sur l'IP que vous avez spécifiée**

- profitez-en pour repérer le port TCP sur lequel écoute l'application
- ça se fait en une seule commande `ss`
- filtrez la sortie de la commande avec un `| grep` pour mettre en évidence la ligne intéressante dans le compte-rendu

## 2. Prêts

🌞 **Se connecter à l'application depuis votre PC**

- depuis votre PC ! (pas depuis une VM)
- depuis votre PC, utilisez une commande `nc` (netcat) pour vous connecter à l'application
  - il faudra l'installer si vous ne l'avez pas sur votre PC :)
- il faudra ouvrir un port firewall sur la VM (celui sur lequel écoute `efrei_server`, que vous avez repéré à l'étape précédente) pour qu'un client puisse se connecter

```bash
# avec netcat, vous pourrez vous connecter en saissant :
nc <IP> <PORT>
```

## 3. Hackez

🌞 **Euh bah... hackez l'application !**

- elle est vulnérable
- c'est un cas d'école, je pense que ça prendra pas longtemps à la plupart d'entre vous :d
- bref, en tant que clients, vous pouvez avoir un shell sur la machine serveur, et exécuter des commandes

> N'hésitez pas à me demander de l'aide pour cette section si c'est po clair ni intuitif pour vous.

🌟 **BONUS : DOS l'application**

- il faut rendre l'application inopérante
- pour être précis, un DOS ici, c'est qu'aucun autre client ne doit pouvoir se connecter
- utilisez un autre vecteur que la vulnérabilité précédente pour provoquer le DOS

![Hac](./img/hac.png)

---

➜ **BON** on a une application qui tourne sur une machine Linux, à l'arrache.  
**Nos dévs sont nuls, l'app est vulnérable.** 🙃

> *ui c moa le dév é alor ?!*

Dans le reste du TP, **on va continuer à bosser sur l'hébergement mais en sachant ça : l'app est vulnérable.**  
Notre but va donc être de proposer l'hébergement de cette application vulnérable, mais en **minimisant l'impact de cette vulnérabilité** le plus possible.

➜ **Comment héberger une application vulnérable et dormir (à peu près) sur ses deux oreilles ?**

> *Bonne question Jamy ! Continuons le TP :d*

> ➜ [**Lien vers la partie 2**](./part2.md)