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
