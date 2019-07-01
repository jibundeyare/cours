# APT

APT (Advanced Package Tool) est gestionnaire de package de la distribution linux Debian.

Note : un package peut être c'est une application graphique, une application en ligne de commande, un service, un plugin, un driver, une librairie logicielle, ...

Note bis : en français on dit « un paquet ».

## Principe

Pour installer des packages, votre machine se connecte à un répertoire et télécharge la liste des packages disponibles.
Le fichier `/etc/sources.list` permet de définir à quel répoertoires `apt` se connecte pour télécharger la liste des packages disponibles.
Cette liste permet d'obtenir des informations comme le nom d'un package, sa description, sa version, les packages dont il dépend, les packages qui dépendent de lui.

À partir du moment ou vous avez la liste des packages, vous pouvez installer de nouveaux packages ou mettre à jour des packages déjà installés.

## Utilisation

Mettre à jour la liste des packages :

    sudo apt update

Pour chercher un package par mot clé (`foo` par exemple) :

    apt search foo

Mettre à jour tous les packages déjà installés sans en installer de nouveaux ni supprimer de packages installés :

    sudo apt upgrade

Mettre à jour tous les packages déjà installés et installe de nouveaux packages si besoin :

    sudo apt dist-upgrade

Installer le package `foo` :

    sudo apt install foo

Supprimer (désinstalle) le package `foo` :

    sudo apt remove foo

Supprimer (désinstalle) le package `foo` et tout les fichiers liés au package (configuration notamment) :

    sudo apt remove --purge foo
    # ou alors
    sudo apt purge foo

Afficher le manuel de `apt` :

    man apt

Note : pour sortir du manuel, tapez `q`

