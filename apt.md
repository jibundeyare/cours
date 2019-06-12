# APT

APT (Advanced Package Tool) est gestionnaire de paquet de la distribution linux Debian.

Note : un paquet peut être c'est une application graphique, une application en ligne de commande, un service, un plugin, un driver, une librairie logicielle, ...

## Principe

Pour installer des paquets, votre machine se connecte à un répertoire et télécharge la liste des paquets disponibles.
Le fichier `/etc/sources.list` permet de définir à quel répoertoires `apt` se connecte pour télécharger la liste des paquets disponibles.
Cette liste permet d'obtenir des informations comme le nom d'un paquet, sa description, sa version, les paquets dont il dépend, les paquets qui dépendent de lui.

À partir du moment ou vous avez la liste des paquets, vous pouvez installer de nouveaux paquets ou mettre à jour des paquets déjà installés.

## Utilisation

Mettre à jour al liste des paquets :

    sudo apt update

Mettre à jour tous les paquets déjà installés sans en installer de nouveaux ni supprimer de paquets installés :

    sudo apt upgrade

Mettre à jour tous les paquets déjà installés et installe de nouveaux paquets si besoin :

    sudo apt dist-upgrade

Installer le paquet `foo` :

    sudo apt install foo

Supprimer (désinstalle) le paquet `foo` :

    sudo apt remove foo

Afficher le manuel de `apt` :

    man apt

Note : pour sortir du manuel, tapez `q`

