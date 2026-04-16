# Packagin-d-application-sur-intune
ajout d'application avec intune

# 📦 Intune Lab – Déploiement d'application Win32 (7-Zip)

## 🎯 Objectif

Mettre en place un environnement Microsoft Intune et déployer une application Windows (7-Zip) de manière automatique sur un poste client.

---

## 🖥️ Environnement

* Machine virtuelle Windows 11 (VMware)
* Tenant Microsoft 365 / Intune (trial)
* Compte utilisateur Azure AD
* Outil de packaging Intune (IntuneWinAppUtil)

---

## ⚙️ Étapes réalisées

### 1. Préparation du poste client

* Installation d’une VM Windows 11
* Jointure du poste à Azure AD
* Vérification de l’enrôlement dans Intune :

```powershell
dsregcmd /status
```

---

### 2. Préparation de l’application

* Téléchargement de **7-Zip (version .exe)**
* Création d’un dossier de packaging :

```
C:\Intune\7zip
```

* Placement du fichier `7zip.exe` dans ce dossier

---

### 3. Création du package Intune (.intunewin)

* Utilisation de l’outil **IntuneWinAppUtil.exe**

```powershell
.\IntuneWinAppUtil.exe -c C:\Intune\7zip -s 7zip.exe -o C:\Intune
```

* Génération du fichier :

```
7zip.intunewin
```

---

### 4. Ajout de l’application dans Intune

* Type : **Windows app (Win32)**
* Upload du fichier `.intunewin`

#### Configuration :

* **Install command**

```
7zip.exe /S
```

* **Uninstall command**

```
"C:\Program Files\7-Zip\Uninstall.exe" /S
```

* **Install behavior**

```
System
```

---

### 5. Detection rule

* Type : File
* Path :

```
C:\Program Files\7-Zip
```

* File :

```
7zFM.exe
```

* Method : Exists

---

### 6. Assignation

* Groupe : **All devices**
* Mode : **Required**

---

### 7. Déploiement et vérification

* Synchronisation du poste client :

```powershell
dsregcmd /refreshprt
```

* Vérification de l’installation :

```powershell
Test-Path "C:\Program Files\7-Zip\7zFM.exe"
```

✔️ Résultat : **True → application installée avec succès**

---

## 🧠 Compétences acquises

* Packaging d’application Win32
* Utilisation d’Intune pour le déploiement logiciel
* Gestion des règles de détection
* Assignation et suivi de déploiement
* Troubleshooting Intune (sync, ciblage, logs)

---

## 🚀 Conclusion

Ce lab m’a permis de comprendre le processus complet de déploiement d’application via Microsoft Intune, depuis le packaging jusqu’à l’installation automatique sur un poste client.

---

## 🔜 Prochaines étapes

* Déploiement d’autres applications (Chrome, MSI vs EXE)
* Mise en place de politiques de sécurité (compliance, configuration)
* Introduction à SCCM (Microsoft Endpoint Configuration Manager)

---

