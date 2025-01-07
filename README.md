# Connexion-SSH-Windows-et-Linux
---
# 🐧 Linux
---
# 🛠️ Tutoriel : Installer et configurer SSH sur un serveur et un client Linux

SSH (Secure Shell) est un protocole essentiel pour administrer des systèmes Linux à distance en toute sécurité. Ce guide vous explique pas à pas comment installer et configurer SSH sur un serveur et un client Linux.

---

## ⚙️ Pré-requis
- Une machine Linux (Debian, Ubuntu, CentOS, ou autre).
- Accès root ou sudo sur le système.
- Une connexion Internet active.

---

## 🚀 Étape 1 : Installer SSH sur Linux

### 1️⃣ Installation sur le serveur SSH

1. **Mettre à jour le système**
   - Assurez-vous que votre système est à jour :
     ```bash
     sudo apt update && sudo apt upgrade -y   # Pour Debian/Ubuntu
     sudo yum update -y                       # Pour CentOS/RHEL
     ```

2. **Installer le package OpenSSH**
   - Sur Debian/Ubuntu :
     ```bash
     sudo apt install openssh-server -y
     ```
   - Sur CentOS/RHEL :
     ```bash
     sudo yum install openssh-server -y
     ```

3. **Vérifier le statut du service SSH**
   - Assurez-vous que le service SSH est actif :
     ```bash
     sudo systemctl enable ssh
     sudo systemctl start ssh
     sudo systemctl status ssh
     ```

### 2️⃣ Installation sur le client SSH

1. **Installer le package client SSH** (généralement déjà installé par défaut) :
   - Sur Debian/Ubuntu :
     ```bash
     sudo apt install openssh-client -y
     ```
   - Sur CentOS/RHEL :
     ```bash
     sudo yum install openssh-clients -y
     ```

2. **Tester la commande SSH**
   - Vérifiez si `ssh` est disponible :
     ```bash
     ssh -V
     ```

---

## 🚀 Étape 2 : Configurer SSH sur le serveur Linux

### 1️⃣ Modifier les paramètres dans le fichier `sshd_config`

1. **Ouvrir le fichier de configuration SSH**
   - Éditez le fichier `/etc/ssh/sshd_config` avec un éditeur de texte :
     ```bash
     sudo nano /etc/ssh/sshd_config
     ```

2. **Paramètres recommandés pour la sécurité**
   - Recherchez et modifiez les lignes suivantes :
     ```bash
     PermitRootLogin no            # Désactiver la connexion root
     PasswordAuthentication yes    # Permettre l'authentification par mot de passe (ou "no" si clé SSH utilisée)
     PubkeyAuthentication yes      # Activer l'authentification par clé publique
     ```

3. **Redémarrer le service SSH**
   - Appliquez les modifications en redémarrant le service :
     ```bash
     sudo systemctl restart ssh
     ```

### 2️⃣ Configurer le pare-feu pour SSH

1. **Ouvrir le port 22** (par défaut) :
   - Sur Debian/Ubuntu avec UFW :
     ```bash
     sudo ufw allow 22/tcp
     sudo ufw reload
     ```
   - Sur CentOS/RHEL avec Firewalld :
     ```bash
     sudo firewall-cmd --add-service=ssh --permanent
     sudo firewall-cmd --reload
     ```

2. **Vérifier l'ouverture du port** :
   ```bash
   sudo netstat -tuln | grep 22
   ```

---

## 🚀 Étape 3 : Configurer l'authentification par clé SSH

### 1️⃣ Générer une clé SSH sur le client

1. **Générer une paire de clés**
   - Sur le client, exécutez :
     ```bash
     ssh-keygen -t rsa -b 4096
     ```
   - Appuyez sur **Entrée** pour accepter le chemin par défaut (`~/.ssh/id_rsa`).

2. **Copier la clé publique sur le serveur**
   - Utilisez la commande suivante pour copier la clé sur le serveur :
     ```bash
     ssh-copy-id utilisateur@adresse_ip_serveur
     ```
   - Entrez le mot de passe utilisateur pour finaliser.

3. **Tester la connexion sans mot de passe**
   - Connectez-vous au serveur pour vérifier :
     ```bash
     ssh utilisateur@adresse_ip_serveur
     ```

### 2️⃣ Configurer le serveur pour forcer l'utilisation de la clé

1. **Modifier le fichier `sshd_config`**
   - Ajoutez ou modifiez ces lignes :
     ```bash
     PasswordAuthentication no
     PubkeyAuthentication yes
     ```

2. **Redémarrer le service SSH**
   - Appliquez les modifications :
     ```bash
     sudo systemctl restart ssh
     ```

---

## 📋 Résolution des problèmes courants

1. **Erreur : "Connection refused"**
   - Vérifiez que le service SSH est actif :
     ```bash
     sudo systemctl status ssh
     ```
   - Vérifiez le pare-feu et ouvrez le port 22.

2. **Erreur : "Permission denied"**
   - Assurez-vous que la clé publique a été correctement ajoutée dans `~/.ssh/authorized_keys`.
   - Vérifiez les permissions du répertoire `.ssh` :
     ```bash
     chmod 700 ~/.ssh
     chmod 600 ~/.ssh/authorized_keys
     ```

3. **Erreur : "Host key verification failed"**
   - Supprimez l'ancienne entrée de l'hôte dans `~/.ssh/known_hosts` :
     ```bash
     ssh-keygen -R adresse_ip_serveur
     ```

---

## ✅ Conclusion

Vous avez maintenant configuré SSH sur un serveur et un client Linux. L'authentification par clé offre une sécurité accrue et est recommandée pour la gestion à distance. N'hésitez pas à tester et à affiner votre configuration pour répondre à vos besoins spécifiques.
---
---
---
# 🪟 Windows
---
# 🛠️ Tutoriel : Installer et configurer SSH sur un serveur et un client Windows

SSH (Secure Shell) est un protocole essentiel pour administrer des serveurs à distance de manière sécurisée. Ce guide vous expliquera comment installer et configurer SSH sur un serveur Windows ainsi que sur un client Windows, adapté aux débutants.

---

## ⚙️ Pré-requis
- Un ordinateur Windows 10 ou supérieur.
- Une connexion Internet active.
- Accès administrateur sur le système.

---

## 🚀 Étape 1 : Activer et configurer le serveur SSH sur Windows

### 1️⃣ Vérifier la disponibilité de l'OpenSSH Server

1. Ouvrez les **Paramètres Windows** (Win + I).
2. Naviguez vers **Applications > Fonctionnalités facultatives**.
3. Recherchez "OpenSSH Server" dans la liste des fonctionnalités installées.
   - **Si présent** : Passez à l'étape suivante.
   - **Si absent** : Cliquez sur **Ajouter une fonctionnalité**, recherchez "OpenSSH Server" et installez-le.

### 2️⃣ Activer et démarrer le service OpenSSH Server

1. Lancez **PowerShell** en tant qu'administrateur.
2. Exécutez les commandes suivantes pour activer et démarrer le service :
   ```powershell
   Set-Service -Name sshd -StartupType Automatic
   Start-Service sshd
   ```
3. Vérifiez que le service est actif :
   ```powershell
   Get-Service -Name sshd
   ```
   - Le statut doit être "Running".

### 3️⃣ Configurer le pare-feu pour SSH

1. Ajoutez une règle au pare-feu pour permettre les connexions SSH :
   ```powershell
   New-NetFirewallRule -Name "OpenSSH-Server" -DisplayName "OpenSSH Server" -Protocol TCP -LocalPort 22 -Action Allow -Direction Inbound
   ```
2. Assurez-vous que le port 22 est ouvert.

### 4️⃣ Tester la connexion SSH au serveur

1. Depuis une autre machine, utilisez un client SSH (par exemple, PuTTY ou `ssh` en ligne de commande).
2. Connectez-vous au serveur Windows en tapant :
   ```bash
   ssh NomUtilisateur@AdresseIP
   ```
3. Entrez le mot de passe de l'utilisateur.

---

## 🚀 Étape 2 : Configurer le client SSH sur Windows

### 1️⃣ Vérifier la disponibilité du client OpenSSH

1. Ouvrez **PowerShell** ou l'invite de commandes.
2. Tapez :
   ```bash
   ssh
   ```
   - Si une aide ou un guide d'utilisation apparaît, le client SSH est installé.
   - Sinon, installez-le via **Paramètres > Applications > Fonctionnalités facultatives** ("OpenSSH Client").

### 2️⃣ Tester une connexion SSH depuis le client

1. Ouvrez PowerShell ou l'invite de commandes.
2. Connectez-vous à un serveur distant avec :
   ```bash
   ssh NomUtilisateur@AdresseIP
   ```
3. Si la clé de l'hôte est demandée, tapez "yes".
4. Entrez le mot de passe pour établir la connexion.

---

## 🔐 Étape 3 : Configuration avancée (optionnel)

### 1️⃣ Configurer l'authentification par clé SSH

1. **Générer une clé SSH sur le client**
   - Dans PowerShell, tapez :
     ```bash
     ssh-keygen
     ```
   - Suivez les instructions et sauvegardez la clé dans le répertoire par défaut.

2. **Copier la clé publique sur le serveur**
   - Exécutez :
     ```bash
     ssh-copy-id NomUtilisateur@AdresseIP
     ```
   - Si `ssh-copy-id` n'est pas disponible, copiez manuellement le contenu de `id_rsa.pub` (clé publique) dans le fichier `authorized_keys` du serveur :
     - Copiez le contenu de `C:\Users\VotreNom\.ssh\id_rsa.pub`.
     - Collez-le dans `C:\Users\NomServeur\.ssh\authorized_keys` sur le serveur.

3. **Tester l'authentification par clé**
   - Connectez-vous au serveur sans mot de passe :
     ```bash
     ssh NomUtilisateur@AdresseIP
     ```

### 2️⃣ Modifier le fichier de configuration SSH (sshd_config)

1. Ouvrez le fichier `sshd_config` situé dans `C:\ProgramData\ssh\`.
2. Modifiez ou ajoutez les lignes suivantes pour renforcer la sécurité :
   ```bash
   PasswordAuthentication no
   PubkeyAuthentication yes
   PermitRootLogin no
   ```
3. Redémarrez le service SSH :
   ```powershell
   Restart-Service sshd
   ```

---

## 📋 Résolution des problèmes courants

1. **Erreur : "Connection refused"**
   - Assurez-vous que le service SSH est en cours d'exécution.
   - Vérifiez que le port 22 est ouvert dans le pare-feu.

2. **Erreur : "Permission denied"**
   - Vérifiez les permissions du fichier `authorized_keys`.
   - Assurez-vous que l'utilisateur a les droits nécessaires.

3. **Client SSH introuvable**
   - Réinstallez le client OpenSSH via les fonctionnalités facultatives de Windows.

---

## ✅ Conclusion

Vous avez maintenant configuré SSH sur un serveur et un client Windows. Cela permet des connexions sécurisées pour la gestion et le transfert de fichiers. Si vous débutez, commencez par tester des connexions simples avant de passer à des configurations avancées comme les clés SSH.

