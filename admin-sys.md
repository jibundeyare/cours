# Administration système

## APT

Pour en savoir plus sur le gestionnaire de paquet `apt`, voir [apt.md](apt.md).

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

### `chown` (CHange OWNer)

La commande `chown` permet de changer le propriétaire et le groupe d'un fichier ou d'un dossier.

En général, avec la distribution Linux Debian, on utilise le même nom pour le propriétaire et le groupe.

Pour que `toto` devienne le propriétaire et le groupe d'un dossier ou d'un fichier `~/foo` :

    sudo chown toto:toto ~/foo

Pour que `toto` devienne le propriétaire et le groupe de tous les sous-dossiers et fichiers contenu dans le dossier `~/bar` :

    sudo chown -R toto:toto ~/bar

### `chmod` (CHange MODe)

La commande `chmod` permet de changer les permissions d'un fichier ou d'un dossier.

En général on applique les droits `775` aux dossiers :

- propriétaire : lecture, écriture et exécution
- groupe : lecture, écriture et exécution
- autre : lecture et exécution

En général on applique les droits `664` aux fichiers :

- propriétaire : lecture et écriture
- groupe : lecture et écriture
- autre : lecture seule

Pour appliquer ce schéma de droits sur tous les sous-dossiers contenu dans le dossier `~/foo` :

    sudo find ~/foo -type d -exec chmod 0775 {} \;

Pour appliquer ce schéma de droits sur tous les fichiers contenu dans le dossier `~/foo` :

    sudo find ~/foo -type f -exec chmod 0664 {} \;

La page [Chmod Calculator](https://chmod-calculator.com/) permet de calculer la formule magique pour appliquer les permissions que vous souhaitez.

## Sécurisation avec fail2ban

On peut être sûr que dès qu'un service web est mis en place, des bots vont faire leur apparition pour tenter de passer à travers les mécanismes de sécurité.
Un des moyens de s'en prémunir est d'installer fail2ban.

Fail2ban est typiquement utilisé pour sécuriser SSH.
Il permet de bannir pendant quelques minutes les adresses IP qui se trompent plusieurs fois de suite de mot de passe lors d'une connection SSH.
Cela rend inopérant les attaques de type « brute force ».

Mais fail2ban peut être utiliser pour sécuriser d'autres services aussi (mail, ftp, etc).

Il suffit d'installer la package car il est préconfiguré pour fonctionner avec SSH :

    sudo apt install fail2ban

## SSH

Pour configurer un accès SSH, voir [ssh.md](ssh.md).

## Doc

- [Chmod Calculator](https://chmod-calculator.com/)

