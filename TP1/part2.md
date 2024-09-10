# II. Servicer le programme

## Sommaire

- [II. Servicer le programme](#ii-servicer-le-programme)
  - [Sommaire](#sommaire)
  - [1. Création du service](#1-création-du-service)
  - [2. Tests](#2-tests)

## 1. Création du service

🌞 **Créer un service `efrei_server.service`**


```bash
sudo nano /etc/systemd/system/efrei_server.service
```

```systemd
[Unit]
Description=Super serveur EFREI
 
[Service]
ExecStart=/usr/local/bin/efrei_server
EnvironmentFile=
```

```bash
systemctl daemon-reload
```

## 2. Tests

🌞 **Exécuter la commande `systemctl status efrei_server`**

```bash
[toto@rocky ~]$ systemctl status efrei_server
○ efrei_server.service - Super serveur EFREI
     Loaded: loaded (/etc/systemd/system/efrei_server.service; static)
     Active: inactive (dead)
```

🌞 **Démarrer le service**

```bash
[toto@rocky ~]$ sudo systemctl start efrei_server
```

🌞 **Vérifier que le programme tourne correctement**

```bash
[toto@rocky ~]$ sudo systemctl status efrei_server
● efrei_server.service - Super serveur EFREI
     Loaded: loaded (/etc/systemd/system/efrei_server.service; static)
     Active: active (running) since Tue 2024-09-10 16:05:33 CEST; 21min ago
   Main PID: 2036 (python3)
      Tasks: 1 (limit: 10257)
     Memory: 9.4M
        CPU: 53ms
     CGroup: /system.slice/efrei_server.service
             └─2036 /bin/python3 /usr/local/bin/efrei_server/main.py
```

```bash
[toto@rocky ~]$ sudo ss -ltnp
State        Recv-Q       Send-Q             Local Address:Port             Peer Address:Port       Process                                  
LISTEN       0            100                172.16.74.175:8888                  0.0.0.0:*           users:(("python3",pid=2186,fd=6)) 
```

```bash
❯ nc 172.16.74.175 8888
Hello ! Tu veux des infos sur quoi ?
1) cpu
2) ram
3) disk
4) ls un dossier

Ton choix (1, 2, 3 ou 4) : 
```

> ➜ [**Lien vers la partie 3**](./part3.md)