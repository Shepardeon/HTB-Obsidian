---
tags:
  - htb
machine: Bashed
os: Linux
difficulty: Easy
date: 16/11/2025
status: en-cours
---
---
# Bashed

> [!summary] Résumé rapide  
> (Compléter après la machine : vecteur d’entrée + privesc)

---

## 🟦 Informations générales

- **Nom de la machine :** Bashed
- **OS :** Linux
- **Difficulty :** Easy
- **Auteur :** Arrexel
- **Date :** 16/11/2025
- **Statut :** En cours

---
## 🟦 1. Reconnaissance réseau


> [!info] 1.2 Nmap – Scan initial

```bash
nmap -p- 10.129.57.23 --min-rate 5000
```

**Services découverts :**

```bash
Starting Nmap 7.95 ( https://nmap.org ) at 2025-11-15 23:59 CET
Nmap scan report for 10.129.57.23
Host is up (0.051s latency).
Not shown: 65534 closed tcp ports (reset)
PORT   STATE SERVICE
80/tcp open  http

Nmap done: 1 IP address (1 host up) scanned in 19.99 seconds
```


> [!info] 1.3 Nmap – Scan complet

```bash
nmap -sVC -p80 10.129.57.23
```

**Infos supplémentaires :**

```bash
Starting Nmap 7.95 ( https://nmap.org ) at 2025-11-16 00:00 CET 
Nmap scan report for 10.129.57.23
Host is up (0.024s latency).

PORT   STATE SERVICE VERSION
80/tcp open  http    Apache httpd 2.4.18 ((Ubuntu))
|_http-server-header: Apache/2.4.18 (Ubuntu)
|_http-title: Arrexel's Development Site

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 7.09 seconds
```

---
## 🟦 2. Analyse des services

Le site semble être un blog d'un développeur/pentesteur

![[Pasted image 20251116000413.png]]

Dans le menu on note la mention "Colorlib" :
![[Pasted image 20251116000731.png]]
Peut-être une techno Wordpress sous-jacente.

Le blog contient des informations à propos d'un outils nommé "phpbash" qui permet d'utiliser un reverse shell PHP depuis un navigateur quand le port SSH est inaccessible.

https://github.com/Arrexel/phpbash

### 🌐 HTTP / Web
**Technos détectées :**  
-  Apache/2.4.18

**Endpoints trouvés :**

> [!info] Fuzzing

```bash
gobuster dir -w ~/Pentest/Wordlists/dirb/common.txt -u http://bashed.htb/
```

```bash
/.hta                 (Status: 403) [Size: 289]
/.htaccess            (Status: 403) [Size: 294]
/.htpasswd            (Status: 403) [Size: 294]
/css                  (Status: 301) [Size: 306] [--> http://bashed.htb/css/]
/dev                  (Status: 301) [Size: 306] [--> http://bashed.htb/dev/]
/fonts                (Status: 301) [Size: 308] [--> http://bashed.htb/fonts/]
/images               (Status: 301) [Size: 309] [--> http://bashed.htb/images/]
/index.html           (Status: 200) [Size: 7743]
/js                   (Status: 301) [Size: 305] [--> http://bashed.htb/js/]
/php                  (Status: 301) [Size: 306] [--> http://bashed.htb/php/]
/server-status        (Status: 403) [Size: 298]
/uploads              (Status: 301) [Size: 310] [--> http://bashed.htb/uploads/]
Progress: 4613 / 4613 (100.00%)
```

On note les endpoints intéressants suivants :
- /dev => contient le script phpbash.php
- /php => contient un script sendMail.php
- /uploads => vide, contient peut-être des fichiers supplémentaires

---
## 🟦 3. Points d’entrée potentiels

-  Le script phpbash.php semble être tout donné pour se connecter à la machine

---

## 🟦 4. Exploitation

> [!tip] 4.1 PoC

On se dirige juste sur http://bashed.htb/dev/phpbash.php

**Résultat :**  On a un reverse shell sur bashed.htb.

**Accès obtenu :**  
- User :  www-data
- Contexte :  shell

---

## 🟦 5. Post-exploitation / Énumération locale

**Enumération des utilisateurs**

```bash
cat /etc/passwd
```

```bash
root:x:0:0:root:/root:/bin/bash
--
arrexel:x:1000:1000:arrexel,,,:/home/arrexel:/bin/bash  
scriptmanager:x:1001:1001:,,,:/home/scriptmanager:/bin/bash
```

Il y a donc 3 utilisateurs sur la machine : `root`, `arrexel` et `scriptmanager`.

On a accès au flag utilisateur dans `/home/arrexel`.

### 5.1 Mouvement latéral

L'utilisateur `www-data` peut exécuter des commandes en tant que `scriptmanager` sans mot de passe :
```bash
sudo -u scriptmanager whoami
scriptmanager
```

On peut accéder au dossier `/var/www/html/php` pour lire le contenu du fichier `sendMail.php`

```bash
ls -la
```

```bash
total 12  
drw-r-xr-x 2 root root 4096 Jun 2 2022 .  
drw-r-xr-x 10 root root 4096 Jun 2 2022 ..  
-rw-r-xr-x 1 root root 1652 Dec 4 2017 sendMail.php
```

On remarque que le script tente d'inclure un fichier de config :
```php
include_once (dirname(dirname(__FILE__)) . '/config.php');
```

Le fichier de config se trouve dans `/var/www/html/config.php` :

```php
<?php

//SITE GLOBAL CONFIGURATION  
$email = "yourmail@here.com"; //<-- Your email  
  
?>
```

> [!info] Linux

- whoami  
- id
- sudo -l
- find / -perm -4000 -type f 2>/dev/null

**Infos importantes :**

---

## 🟦 6. Élévation de privilèges

### Méthode retenue :
- Type :  
- Pourquoi ça marche :  
- Commande(s) / manip(s) :

**Contexte final :** root / administrator

---

## 🟦 7. Résumé final

> [!success] Ce que j’ai appris  
-  
-  

> [!failure] Ce que j’ai raté / À éviter  
-  
-  

> [!attention] Patterns utiles  
-  

---

## 🟦 8. Annexes
- Captures
- Snippets
- Fichiers intéressants