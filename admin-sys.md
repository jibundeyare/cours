# Administration système

## Le compte Root

Le compte root permet de modifier le système.
Par exemple : installer un nouveau paquet, supprimer un paquet, mettre à jour un paquet, modifier la configuration de paquets installés, etc.

Il est **très fortement** recommandé d'utiliser la commande `sudo` au lieu d'utiliser la commande `su`.

Voici comment fonctionne `sudo` : [xkcd: Sandwich](https://www.xkcd.com/149/).

Par exemple, pour mettre à jour la liste des paquets :

    sudo apt update

Si nécessaire, pour devenir root :

    su

## Installation du package sudo

Vous ne pouvez pas utiliser la commande `sudo` pour installer le package `sudo` puisque le package `sudo` n'est pas encore installé !

D'abord devenez `root` :

    su

Puis installez le package `sudo` :

    apt install sudo

Si votre compte utilisateur existe déjà, ajoutez-le dans le groupe des *sudoers*.
Toujours en tant que root, remplacez `user` par votre nom d'utilisateur :

    usermod -a -G sudo user

Exemple pour rendre *sudoer* l'utilisateur `foo` :

    usermod -a -G sudo foo

**Attention** : pour que le changement soit pris en compte et que votre utilisateur puisse utiliser la commande `sudo`, vous devez fermer sa session puis la réouvrir. Sur votre poste **local**, il se peut même que vous deviez redémarrer l'ordi.

## Création d'un nouvel utilisateur

Remplacez `user` par votre nom d'utilisateur :

    useradd -m -G sudo -s /bin/bash user

Exemple pour créer un utilisateur nommé `foo` qui pourra utiliser `sudo` et dont le terminal par défaut sera `bash` :

    useradd -m -G sudo -s /bin/bash foo 

**Attention** : il est fortement recommandé de n'utiliser que des lettres minuscules et des chiffres arabes.
Évitez absolument d'utiliser des accents ou des caractères spéciaux (point d'exclamation, dièse, etc).

## Attribuer ou changer le mot de passe d'un utilisateur

Changez `user` par le nom de l'utilisateur dont vous voulez changer le mot de passe :

    passwd user

Exemple pour changer le mot de passe d'un utiliser nommé `foo` :

    passwd foo

**NB** Quand on tape le mot de passe, rien n'apparaît, c'est normal.
C'est une question de sécurité.

## La propriété et les permissions des fichiers et dossiers

Sous Linux, les fichiers et dossiers ont un propriétaire, un groupe et des permissions.

il n'y a que trois types de permissions :

- permission de lire
- permission d'écrire (ajouter, modifier, supprimer)
- permission d'exécuter (rendre un fichier exéctubale ou rendre un dossier accessible)

Cela permet de définir :

- ce que le propriétaire peut faire avec le fichier ou dossier
- ce que le groupe peut faire avec le fichier ou dossier
- ce que les autres peuvent faire avec le fichier ou dossier

La commande `chown` permet de changer le propriétaire et le groupe d'un fichier ou d'un dossier.
La commande `chmod` permet de changer les permissions d'un fichier ou d'un dossier.

La page [Chmod Calculator](https://chmod-calculator.com/) permet de calculer la formule magique pour appliquer les permissions que vous souhaitez.

## Doc

- [Chmod Calculator](https://chmod-calculator.com/)

