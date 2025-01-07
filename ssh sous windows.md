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
