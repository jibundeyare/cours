# SSH

Le protocole SSH permet d'accéder au terminal d'un serveur distant.
On peut utiliser la ligne de commande du terminal comme si un clavier et un écran étaient directement branchés dessus.

## Se connecter à un serveur distant

Avant toute connexion, le serveur vous demandera de vous authentifier, c-à-d prouver que vous êtes bien qui vous prétendez être.
Il y a deux façons très répandues de s'authentifier :

- par mot de passe
- par clé SSH

Si vous n'avez pas encore de clé SSH, le serveur vous demandera automatiquement un mot de passe.

Remplacez `user` par votre nom d'utilisateur et `host` par l'adresse ip ou le nom de domaine de votre serveur :

    ssh user@host

Exemple pour se connecter au serveur `185.71.10.24` avec le compte `root` :

    ssh root@185.71.10.24

Exemple pour se connecter au serveur `185.71.10.24` avec le compte `foo` :

    ssh foo@185.71.10.24

Exemple pour se connecter au serveur `example.com` avec le compte `foo` :

    ssh foo@example.com

**Attention** : lors de la première connexion, le client SSH vous demandera si vous acceptez ou non la signature du serveur distant. Si vous décidez de l'accepter, il faudra bien taper `yes` en toute lettre, sans quoi le client SSH croira que vous ne voulez pas l'accepter.

## Générer des clés SSH

Dans le terminal :

    ssh-keygen -t rsa -b 4096
    ssh-keygen -t dsa
    ssh-keygen -t ecdsa -b 521
    ssh-keygen -t ed25519

Si c'est la première fois que vous créez des clés SSH, vous pouvez valider les choix par défaut.

Pour un usage qui ne nécessite pas de sécurité maximale (par exemple se connecter à son VPS perso qui n'héberge que quelques sites web en PHP), je recommande de laisser vide le champ `passphrase`.
Ainsi vous ne devrez pas renseigner le mot de passe à chaque fois que vous utiliserez vos clés SSH.
Par contre si vous utilisez votre clé SSH pour vos connectez à votre portefeuille de Bitcoin (par exemple), un mot de passe est **fortement recommandé**.

## Copier ses clés publiques sur un serveur distant

Pour bénéficier de l'authentification par clé SSH (et donc sans mot de passe), vous devez d'abord copier les clés publiques sur le serveur distant auquel vous voulez vous connecter.

Remplacez `user` par votre nom d'utilisateur et `host` par l'adresse ip ou le nom de domaine de votre serveur :

    ssh-copy-id user@host

Exemple pour copier la clé ssh dans le compte `root` du serveur `example.com` :

    ssh-copy-id root@example.com

Exemple pour copier la clé ssh dans le compte `foo` du serveur `example.com` :

    ssh-copy-id foo@example.com

## Sécuriser le service SSH

Voir la section « Sécurisation avec fail2ban » dans [admin-sys.md](admin-sys.md).

## Doc

- [How to use ssh-keygen to generate a new SSH key | SSH.COM](https://www.ssh.com/ssh/keygen/)
- [Changer le mot de passe root sur un VPS | Documentation OVH](https://docs.ovh.com/fr/vps/root-password/)

