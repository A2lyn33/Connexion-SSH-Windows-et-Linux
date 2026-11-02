# Administrer Windows et Linux avec SSH
**Fiches de laboratoire · administration à distance**

Configurer une connexion SSH, comprendre l'authentification par clé et vérifier le résultat.

## Guides

- [Serveur et client Linux — Debian/Ubuntu](ssh%20sous%20linux.md)
- [Serveur et client Windows](ssh%20sous%20windows.md)

## Démonstration à préparer

| Vérification | Preuve à conserver |
| --- | --- |
| Service actif et port accessible | État du service et test depuis le client |
| Identité du serveur | Comparaison de l'empreinte avec celle relevée sur le serveur |
| Connexion par clé | Connexion réussie en forçant la méthode `publickey` |
| Compte distant attendu | Sortie de `whoami` sur le serveur |
| Dépannage | Symptôme, diagnostic et solution vérifiée |

Utiliser des comptes et adresses de laboratoire. Les clés privées et mots de passe restent hors du dépôt.

**Statut :** documentation relue en novembre 2026, notamment pour les comptes administrateurs Windows et la vérification des clés d'hôte. Aucun nouveau test de connexion entre deux machines n'a été réalisé à cette occasion.

[Retour au portfolio](https://github.com/A2lyn33)
