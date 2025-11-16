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

### 🌐 HTTP / Web (si présent)
**Technos détectées :**  
**Endpoints trouvés :**

> [!info] Fuzzing

**Hypothèses :**  
-  
-  

---

### 📁 SMB / FTP / Autres services
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

## 🟦 5. Post-exploitation / Énumération locale

> [!info] Linux (si Linux)

- whoami  
- id
- sudo -l
- find / -perm -4000 -type f 2>/dev/null

> [!info] Windows (si Windows)  
- winPEAS  
- services  
- scheduled tasks  
- credentials  

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