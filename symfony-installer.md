# Symfony installer

Cet outil permet d'installer Symfony.

Note : il existe une version 1.5.11 obsolète de cet outil qui permet d'installer la version de Symfony 2.x et une version Symfony 3.4 qui a une structure de dossier compatible avec Symfony 2.x et incompatible avec Symfony 4.x.

## Installation

### Windows

Télécharger le programme d'installation depuis l'adresse suivante et le lancer : [https://get.symfony.com/cli/setup.exe](https://get.symfony.com/cli/setup.exe).

### MacOS et Linux

Ouvrir un terminal et taper les commandes suivante :

    curl -sS https://get.symfony.com/cli/installer | bash
    sudo mkdir -p /usr/local/bin
    sudo mv ~/.symfony/bin/symfony /usr/local/bin/symfony

### Commandes utiles

Tester l'installation :

    symfony -V

La commande précédente est censée afficher quelque chose qui ressemble à :

    Symfony CLI version 4.2.5 (Wed Jan 16 09:53:39 CET 2019)

Installer la dernière version stable de Symfony en mode web app :

    symfony new --full my_project

Installer la dernière version stable de Symfony en mode minimal (microservice, cli ou API) :

    symfony new my_project

Installer Symfony 3.4 en mode web app :

    symfony new --full --version=3.4 my_project

Installer Symfony 3.4 en mode minimal (microservice, cli ou API) :

    symfony new --version=3.4 my_project

## Doc

- [Download Symfony Framework and Components](https://symfony.com/download)
- [Installing & Setting up the Symfony Framework (Symfony 4.4 Docs)](https://symfony.com/doc/4.4/setup.html)

