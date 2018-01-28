# Composer

Composer est un gestionnaire de paquet. Cet outil permet d'installer / désinstaller des programmes PHP, des frameworks PHP ou des librairies PHP.

Les paquets sont recensés sur le site [Packagist](https://packagist.org/).

## Installation

### Windows

Télécharger et exécuter l'installeur [Composer-Setup.exe](https://getcomposer.org/Composer-Setup.exe).

### Macos et Linux

Ouvrir un terminal, exécuter les commandes de la page [Composer](https://getcomposer.org/download/) puis :

    sudo mkdir -p /usr/local/bin
    sudo mv composer.phar /usr/local/bin/composer

## Dossiers et fichiers

Le fichier `composer.json` contient les dépendances du projet installées par le développeur.

Le fichier `composer.lock` contient la liste complète des dépendances du projet (les dépendances des dépendances aussi donc !).

Le dossier `vendor/` contient tous les paquets installés par composer. Ce dossier peut-être supprimé et entièrement reconstitué si le fichier `composer.json` ou  le fichier `composer.lock` existent.

Le fichier `vendor/autoload.php` permet l'autoloading de tous les paquets installés avec composer.

## Autoloading

Si on veut que composer puisse charger tout seul les fichiers PHP du dossier `src/`, il faut ajouter une clé `autoload` dans le fichier `composer.json` :

    "autoload": {
        "psr-4": {"App\\": "src/"}
    }

puis demander à composer de générer un nouveau fichier d'autoloading :

    composer dump-autoload

## Commandes

Tester l'installation :

    composer -v

Chercher un paquet :

    composer search  [mot clé]

Obtenir des infos sur un paquet non installé :

    composer info -a [nom du paquet]

Installer un paquet :

    composer require [nom du paquet]

Installer un paquet pour l'environnement de développement :

    composer require --dev [nom du paquet]

Par exemple, installer remg/generator-bundle :

    composer require --dev remg/generator-bundle

Installer une version particulière d'un paquet :

    composer require [nom du paquet] [numéro de version]

Par exemple, installer précisément la version 1.3.0 de twig :

    composer require twig/twig 1.35

Installer une version minimum d'un paquet :

    composer require [nom du paquet] ~[numéro de version]

Par exemple, installer la version 1.35 minimum (mais pas la version 2.x) de twig :

    composer require twig/twig ~1.35

Supprimer un paquet installé :

    composer remove [nom du paquet]

Installer les paquets requis (« required ») dans le fichier `composer.json` ou  le fichier `composer.lock` :

    composer install

Mettre à jour tous les paquets installés :

    composer update

Mettre à jour un paquet installé :

    composer update [nom du paquet]

Générer un nouveau fichier d'autoloading :

    composer dump-autoload

Mettre à jour composer :

    composer self-update

## Symfony Flex

Symfony Flex est un plugin Composer qui permet d'installer Symfony et des Bundles.

Cet outil est utilisable à partir de Symfony 3.4.

## Doc

- [Composer](https://getcomposer.org/)
- [Download - Composer](https://getcomposer.org/download/)
- [Introduction - Composer](https://getcomposer.org/doc/00-intro.md#globally)
- [How do I install Composer programmatically? - Composer](https://getcomposer.org/doc/faqs/how-to-install-composer-programmatically.md)
- [Basic usage - Composer](https://getcomposer.org/doc/01-basic-usage.md#autoloading)
- [Versions and constraints - Composer](https://getcomposer.org/doc/articles/versions.md)
- [The composer.json Schema - Composer](https://getcomposer.org/doc/04-schema.md#psr-4)
- [Using Symfony Flex to Manage Symfony Applications (Symfony 3.4 Docs)](https://symfony.com/doc/3.4/setup/flex.html)
