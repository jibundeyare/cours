# Symfony installeur

Cet outil permet d'installer Symfony.

Il est utilisable avec les branches `2.x`, `3.x` et `4.x`.

## Installation

### Windows

Ouvrir un terminal et lancer les commandes suivantes :

    php -r "file_put_contents('symfony', file_get_contents('https://symfony.com/installer'));"

Créer un dossier `bin` dans sa home (`C:\Users\[user]`) :

    mkdir bin

Déplacer le binaire `symfony` :

    move symfony bin/

Créer le fichier `symfony.bat` dans le dossier `bin` :

    cd bin
    (echo @ECHO OFF & echo php "%~dp0symfony" %*) > symfony.bat

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

    symfony

### Macos et Linux

    sudo mkdir -p /usr/local/bin
    sudo curl -LsS https://symfony.com/installer -o /usr/local/bin/symfony
    sudo chmod a+x /usr/local/bin/symfony

## Commandes utiles

Tester l'installation :

    symfony -v

Installer Symfony 2.8 :

    symfony new my_project 2.8

Installer Symfony 3.4 :

    symfony new my_project 3.4

## Doc

- [Installing & Setting up the Symfony Framework (Symfony 3.4 Docs)](http://symfony.com/doc/3.4/setup.html)
- [Installing & Setting up the Symfony Framework (Symfony 2.8 Docs)](https://symfony.com/doc/2.8/setup.html)
- [curl - Extract CA Certs from Mozilla](https://curl.haxx.se/docs/caextract.html)
