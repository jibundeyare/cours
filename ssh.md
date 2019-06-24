# SSH

## Générer les clés SSH

Dans le terminal :

    ssh-keygen -t rsa -b 4096
    ssh-keygen -t dsa
    ssh-keygen -t ecdsa -b 521
    ssh-keygen -t ed25519

Si c'est la première fois que vous créez des clés SSH, vous pouvez valider les choix par défaut.

Pour un usage qui ne nécessite pas de sécurité maximale (par exemple se connecter à son VPS perso qui n'héberge que quelques sites web en PHP), je recommande de laisser vide le champ `passphrase`.
Ainsi vous ne devrez pas renseigner le mot de passe à chaque fois que vous utiliserez vos clés SSH.
Par contre si vous utilisez votre clé SSH pour vos connectez à votre portefeuille de Bitcoin (par exemple), un mot de passe est **fortement recommandé**.

## Se connecter à un serveur distant

Remplacez `user` par votre nom d'utilisateur et `host` par l'adresse ip ou le nom de domaine de votre serveur :

    ssh user@host

Exemple pour se connecter au serveur `185.71.10.24` avec le compte `root` :

    ssh root@185.71.10.24

Exemple pour se connecter au serveur `185.71.10.24` avec le compte `foo` :

    ssh foo@185.71.10.24

Exemple pour se connecter au serveur `example.com` avec le compte `foo` :

    ssh foo@example.com

## Copier ses clés publiques sur un serveur distant

Remplacez `user` par votre nom d'utilisateur et `host` par l'adresse ip ou le nom de domaine de votre serveur :

    ssh-copy-id user@host

Exemple pour copier la clé ssh dans le compte `root` du serveur `example.com` :

    ssh-copy-id root@example.com

Exemple pour copier la clé ssh dans le compte `foo` du serveur `example.com` :

    ssh-copy-id foo@example.com

## Doc

- [How to use ssh-keygen to generate a new SSH key | SSH.COM](https://www.ssh.com/ssh/keygen/)
