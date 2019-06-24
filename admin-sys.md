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

    useradd -G sudo -s /bin/bash user

Exemple pour créer un utilisateur nommé `foo` qui pourra utiliser `sudo` et dont le terminal par défaut sera `bash` :

    useradd -G sudo -s /bin/bash foo 
