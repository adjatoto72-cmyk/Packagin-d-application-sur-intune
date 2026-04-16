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
<img width="603" height="175" alt="Capture d&#39;écran 2026-04-15 234902" src="https://github.com/user-attachments/assets/47ec18fd-cb05-4f46-a315-0db16712c800" />

---

### 2. Préparation de l’application

* Téléchargement de **7-Zip (version .exe)**
* Création d’un dossier de packaging :

```
C:\Intune\7zip
```

* Placement du fichier `7zip.exe` dans ce dossier
<img width="1189" height="683" alt="Capture d&#39;écran 2026-04-16 000542" src="https://github.com/user-attachments/assets/b67dd730-0505-4eec-88b3-5c5e500175fe" />

---

### 3. Création du package Intune (.intunewin)

* Utilisation de l’outil **IntuneWinAppUtil.exe**

```powershell
.\IntuneWinAppUtil.exe -c C:\Intune\7zip -s 7zip.exe -o C:\Intune
```
<img width="1024" height="722" alt="Capture d&#39;écran 2026-04-16 000556" src="https://github.com/user-attachments/assets/0e530940-6865-46dc-8f2d-650f20f0eaba" />

* Génération du fichier :

```
7zip.intunewin
```

---

### 4. Ajout de l’application dans Intune

* Type : **Windows app (Win32)**
* Upload du fichier `.intunewin`
<img width="885" height="653" alt="Capture d&#39;écran 2026-04-16 000705" src="https://github.com/user-attachments/assets/e623c1e1-d1ac-452f-bacb-3a80256c9058" />

#### Configuration :

* **Install command**

```
7zip.exe /S
```
<img width="1920" height="1032" alt="Capture d&#39;écran 2026-04-16 000904" src="https://github.com/user-attachments/assets/83a4be51-2ab9-4afa-90b5-d9f83b84c5ca" />

* **Uninstall command**

```
"C:\Program Files\7-Zip\Uninstall.exe" /S
```
<img width="1920" height="1080" alt="Capture d&#39;écran 2026-04-16 003653" src="https://github.com/user-attachments/assets/883a3172-b03c-4af1-9134-745023f39a2d" />

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
<img width="1920" height="1080" alt="Capture d&#39;écran 2026-04-16 004922" src="https://github.com/user-attachments/assets/3a01c3de-9347-4df3-9629-c69de2eb496c" />

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
<img width="741" height="305" alt="Capture d&#39;écran 2026-04-16 010351" src="https://github.com/user-attachments/assets/5a5f7591-08d9-47ef-b29d-f2839a4450fb" />


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

