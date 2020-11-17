# Composer

Composer est un gestionnaire de paquet. Cet outil permet d'installer / désinstaller des programmes PHP, des frameworks PHP ou des librairies PHP.

Les paquets sont recensés sur le site [Packagist](https://packagist.org/).

## Installation

### Windows

Télécharger et exécuter l'installeur [Composer-Setup.exe](https://getcomposer.org/Composer-Setup.exe).

Attention : quand l'installeur vous demande quelle version de PHP utiliser, choisissez bien la dernière version possible (donc PHP 7+).
Et rappelez-vous que PHP 5.6 est officiellement obsolète depuis le 01/01/2019.

### Macos et Linux

Ouvrir un terminal, exécuter les commandes de la page [Composer](https://getcomposer.org/download/) puis :

    sudo mkdir -p /usr/local/bin
    sudo mv composer.phar /usr/local/bin/composer

Composer vous permet d'installer des outils exécutables en ligne de commande (comme `phpcs` ou `phpunit` par exemple).
Mais pour pouvoir les exécuter, vous devez ajouter la ligne suivante dans votre fichier `~/.bash_profile` :

    PATH="$PATH:$HOME/.composer/vendor/bin"

Au prochain démarrage de votre terminal, les outils du dossier `~/.composer/vendor/bin` seront disponibles.

## Dossiers et fichiers

Le fichier `composer.json` contient les dépendances du projet installées par le développeur.

Le fichier `composer.lock` contient la liste complète des dépendances du projet (les dépendances des dépendances aussi donc !).

Le dossier `vendor/` contient tous les paquets installés par Composer. Ce dossier peut-être supprimé et entièrement reconstitué si le fichier `composer.json` ou  le fichier `composer.lock` existent.

Le fichier `vendor/autoload.php` permet l'autoloading de tous les paquets installés avec Composer.

## Autoloading

L'autoloading est un système qui permet de charger automatiquement un fichier PHP.

Si on veut que notre application puisse charger automatiquement les fichiers PHP du dossier `src/`, il faut ajouter une clé `autoload` dans le fichier `composer.json` :

    "autoload": {
        "psr-4": {"": "src/"}
    }

puis demander à Composer de générer un nouveau fichier d'autoloading :

    composer dump-autoload

et enfin inclure le code d'autoloading dans notre application en y ajoutant la ligne suivante :

    require __DIR__.'../vendor/autoload.php';

Si les fichiers PHP du dossier `src/` sont dans le namespace particulier (`Foo` par exemple), l'autoloading doit être adapté :

    "autoload": {
        "psr-4": {"Foo\\": "src/"}
    }

## Commandes

Tester l'installation :

    composer -v

Chercher un paquet :

    composer search  [mot-clé]

Obtenir des infos sur un paquet non installé :

    composer info -a [nom du paquet]

Installer un paquet :

    composer require [nom du paquet]

Par exemple, installer symfony/var-dumper :

    composer require symfoy/var-dumper

Installer un paquet pour l'environnement de développement :

    composer require --dev [nom du paquet]

Installer une version précise d'un paquet :

    composer require [nom du paquet] [numéro de version]

Par exemple, installer précisément la version `1.3.0` de twig :

    composer require twig/twig 1.30

Installer une version minimum d'un paquet :

    composer require [nom du paquet] ~[numéro de version]

Par exemple, installer la version `1.35` minimum (mais pas la version `2.x`) de twig :

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

## Packagist

Le site [packagist.org](https://packagist.org/) est un annuaire communautaire de tous les packages disponible avec Composer.

C'est donc aussi un moteur de recherche de packages PHP qui peut remplacer la commande `composer search [mot-clé]`.

Si vous créer un package et souhaitez le diffuser, il faudra déclarer son existence sur ce site pour que la communauté PHP puisse le trouver et l'installer avec Composer.

## Doc

- [Composer](https://getcomposer.org/)
- [Download - Composer](https://getcomposer.org/download/)
- [Introduction - Composer](https://getcomposer.org/doc/00-intro.md#globally)
- [How do I install Composer programmatically? - Composer](https://getcomposer.org/doc/faqs/how-to-install-composer-programmatically.md)
- [Basic usage - Composer](https://getcomposer.org/doc/01-basic-usage.md#autoloading)
- [Versions and constraints - Composer](https://getcomposer.org/doc/articles/versions.md)
- [The composer.json Schema - Composer](https://getcomposer.org/doc/04-schema.md#psr-4)
- [Using Symfony Flex to Manage Symfony Applications (Symfony 3.4 Docs)](https://symfony.com/doc/3.4/setup/flex.html)
- [Packagist](https://packagist.org/)

