# Symfony installer 1.5.11 deprecated

*Attention : ce cours montre comment installer la version obsolète 1.5.11 de cet outil.*

Cet outil permet d'installer Symfony.

La version obsolète 1.5.11 de cet outil permet d'installer Symfony 2.x et une version de Symfony 3.4 qui a une structure de dossier compatible avec Symfony 2.x et incompatible avec Symfony 4.x

## Installation de l'ancien outil (version `1.5.11`)

### Windows

Ouvrir un terminal et se déplacer dans sa home (`C:\Users\[user]`) :

    cd %HOMEPATH%

Attention : notez bien le dossier dans lequel vous vous toruvez, c'est votre home.
Dans les lignes qui suivent, je désigne le dossier de la home par `C:\Users\[user]`.
Mais vous devrez utiliser le vrai chemin de votre home.

Télécharger l'installeur :

    php -r "file_put_contents('symfony-1.5.11.phar', file_get_contents('https://symfony.com/installer'));"

Créer un dossier `bin` dans sa home :

    mkdir bin

Déplacer le binaire `symfony-1.5.11.phar` :

    move symfony-1.5.11.phar bin/

Créer le fichier `symfony-1.5.11.bat` dans le dossier `bin` :

    cd bin
    (echo @ECHO OFF & echo php "%~dp0symfony-1.5.11.phar" %*) > symfony-1.5.11.bat

Télécharger le fichier [cacert.pem](https://curl.haxx.se/ca/cacert.pem) puis le déplacer dans le dossier `bin`.

Trouver le fichier `php.ini` utilisé :

    php --ini

Ouvrir le fichier `php.ini` et modifier la ligne :

    ;curl.cainfo =

afin d'obtenir :

    curl.cainfo = "C:\Users\[user]\bin\cacert.pem"

Ajouter le dossier `C:\Users\[user]\bin` dans la variable d'environnement `PATH` du système.

Voir [variables-environnement.md](variables-environnement.md) pour plus d'infos.

Fermer le terminal. Ouvrir un nouveau terminal puis tester le binaire :

    symfony-1.5.11

### Macos et Linux

    sudo mkdir -p /usr/local/bin
    sudo curl -LsS https://symfony.com/installer -o /usr/local/bin/symfony-1.5.11
    sudo chmod a+x /usr/local/bin/symfony-1.5.11

### Commandes utiles

Tester l'installation :

    symfony-1.5.11 -v

La commande précédente est censée afficher quelque chose qui ressemble à :

    Symfony Installer version 1.5.11

Installer Symfony 2.8 :

    symfony-1.5.11 new my_project 2.8

Installer Symfony 3.4 :

    symfony-1.5.11 new my_project 3.4

## Doc

- [symfony/symfony-installer: The Symfony Installer](https://github.com/symfony/symfony-installer)
- [Installing & Setting up the Symfony Framework (Symfony 3.4 Docs)](http://symfony.com/doc/3.4/setup.html)
- [Installing & Setting up the Symfony Framework (Symfony 2.8 Docs)](https://symfony.com/doc/2.8/setup.html)
- [curl - Extract CA Certs from Mozilla](https://curl.haxx.se/docs/caextract.html)

