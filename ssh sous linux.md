# SSH sous Linux — laboratoire Debian/Ubuntu

## Préparer le serveur

Avec un compte disposant de sudo :

```bash
sudo apt update
sudo apt install openssh-server
sudo systemctl enable --now ssh
sudo systemctl status ssh
```

Sur le client, installer `openssh-client` si nécessaire. Vérifier la connectivité réseau et les règles du pare-feu ; limiter les accès SSH au réseau ou au poste d'administration autorisé.

## Vérifier l'identité du serveur

Relever son empreinte depuis une console de confiance, par exemple pour la clé Ed25519 :

```bash
sudo ssh-keygen -lf /etc/ssh/ssh_host_ed25519_key.pub
```

Depuis le client :

```bash
ssh utilisateur@adresse_ip_serveur
```

Comparer l'empreinte affichée avec celle du serveur avant d'accepter la connexion.

## Utiliser une clé

Sur le client, créer une clé dédiée en choisissant une phrase de passe. Si une clé existe déjà à l'emplacement proposé, choisir un autre nom pour la nouvelle clé.

```bash
ssh-keygen -t ed25519
ssh-copy-id -i ~/.ssh/id_ed25519.pub utilisateur@adresse_ip_serveur
ssh -o PreferredAuthentications=publickey -o PasswordAuthentication=no utilisateur@adresse_ip_serveur
```

Le dernier test force l'authentification par clé. Une demande de phrase de passe de la clé locale reste normale.

## Modifier la configuration

Garder la session actuelle ouverte et un accès console de secours. Après réussite du test par clé dans une deuxième session, adapter `/etc/ssh/sshd_config` et les éventuels fichiers inclus :

```text
PermitRootLogin no
PubkeyAuthentication yes
PasswordAuthentication no
KbdInteractiveAuthentication no
```

Contrôler la syntaxe avant de recharger :

```bash
sudo sshd -t
sudo systemctl reload ssh
```

Si le contrôle échoue, corriger l'erreur avant le rechargement. Vérifier ensuite une nouvelle connexion.

## Dépanner

| Symptôme | Vérification |
| --- | --- |
| Connexion refusée | Adresse, port, service `ssh` et pare-feu |
| Permission refusée | Compte, clé publique, propriétaire et permissions de `~/.ssh` et `authorized_keys` |
| Clé d'hôte différente | Confirmer la raison du changement et comparer l'empreinte via une console de confiance |

Ne retirer une ancienne entrée de `known_hosts` qu'après validation de l'identité du serveur. Une différence inattendue doit être investiguée.

**Statut :** fiche documentaire ; validation pratique à réaliser sur les machines du laboratoire.

Référence : [OpenSSH — documentation officielle](https://www.openssh.com/manual.html).

[Retour au sommaire](README.md)
