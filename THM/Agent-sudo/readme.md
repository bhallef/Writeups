# THM - Agent Sudo
![logo du challenge](logo.png)
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

J'essai la connexion au FTP avec anonymous mais sans succès.

La page html nous indiques qu'avec un user-agent différent on peut avoir accès à des informations supplémentaires. Le user-agent qui nous affichera des informations différentes correspond au nom de code de l'agent.