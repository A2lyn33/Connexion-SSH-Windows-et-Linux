# SSH sous Windows — laboratoire

## Préparer le serveur

Sur la machine qui recevra les connexions, vérifier la présence d'OpenSSH Server dans les fonctionnalités Windows. L'installation et la gestion du service nécessitent une console PowerShell administrateur.

```powershell
Get-Service sshd
Set-Service -Name sshd -StartupType Automatic
Start-Service sshd
```

Vérifier la règle de pare-feu OpenSSH existante et limiter son accès aux machines d'administration prévues. Depuis le client :

```powershell
Test-NetConnection -ComputerName ADRESSE_SERVEUR -Port 22
ssh utilisateur@ADRESSE_SERVEUR
```

Les valeurs en majuscules sont à remplacer. Avant d'accepter une clé d'hôte, comparer son empreinte avec celle relevée sur le serveur par un canal de confiance.

## Authentification par clé

Créer une paire de clés sur le client avec `ssh-keygen`, en protégeant la clé privée par une phrase de passe. Choisir un nouveau nom si le fichier proposé existe déjà.

Copier uniquement la **clé publique** sur le serveur. L'emplacement dépend du compte et de la configuration effective de `sshd_config` :

| Compte Windows distant | Emplacement habituel |
| --- | --- |
| Utilisateur standard | `C:\Users\NomUtilisateur\.ssh\authorized_keys` |
| Membre du groupe Administrateurs, avec la configuration OpenSSH par défaut | `C:\ProgramData\ssh\administrators_authorized_keys` |

Les permissions du fichier doivent être conformes aux exigences OpenSSH ; voir la documentation Microsoft ci-dessous. Une copie dans le profil de l'utilisateur administrateur ne suffit pas avec la configuration par défaut.

Tester depuis le client :

```powershell
ssh -o PreferredAuthentications=publickey -o PasswordAuthentication=no utilisateur@ADRESSE_SERVEUR
```

## Modifier la configuration

Le fichier est généralement `C:\ProgramData\ssh\sshd_config`. Avant de désactiver les mots de passe, réussir une connexion par clé dans une deuxième session et conserver un accès local de secours.

Après modification, vérifier la syntaxe avec le binaire `sshd.exe -t` de l'installation OpenSSH, puis redémarrer le service :

```powershell
Restart-Service sshd
```

Tester une nouvelle connexion. Ne pas copier automatiquement les directives propres à un serveur Linux dans la configuration Windows.

## Dépanner

- **Port inaccessible :** vérifier l'adresse, le service et la portée du pare-feu.
- **Clé refusée :** vérifier le compte distant, le chemin des clés autorisées et leurs permissions.
- **Empreinte différente :** confirmer l'identité du serveur avant de modifier `known_hosts`.

**Statut :** fiche relue en novembre 2026 ; connexion réelle à valider dans un laboratoire Windows.

Références : [Microsoft — gestion des clés](https://learn.microsoft.com/en-us/windows-server/administration/openssh/openssh_keymanagement) · [Configuration du serveur](https://learn.microsoft.com/en-us/windows-server/administration/openssh/openssh-server-configuration).

[Retour au sommaire](README.md)
