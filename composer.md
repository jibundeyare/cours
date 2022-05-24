# Composer

Composer est un gestionnaire de paquet. Cet outil permet d'installer / désinstaller des programmes PHP, des frameworks PHP ou des librairies PHP.

Les paquets sont recensés sur le site [Packagist](https://packagist.org/).

## Installation

### Windows

Télécharger et exécuter l'installeur [Composer-Setup.exe](https://getcomposer.org/Composer-Setup.exe).

Attention : quand l'installeur vous demande quelle version de PHP utiliser, choisissez bien la dernière version possible (donc PHP 7+).
Et rappelez-vous que PHP 5.6 est officiellement obsolète depuis le 01/01/2019.

Composer vous permet d'installer des outils exécutables en ligne de commande (comme `phpcs` ou `phpunit` par exemple).
Mais pour pouvoir les exécuter, vous devez ajouter la ligne suivante dans votre variable d'environnement :

```
%USERPROFILE%\AppData\Roaming\Composer\vendor\bin
```

Pour en savoir un peu plus sur les variables d'environnement, voir [variables-environnement.md](variables-environnement.md).

Au prochain démarrage de votre terminal, les outils du dossier `~/.composer/vendor/bin` seront disponibles.

### Macos et Linux

Ouvrir un terminal, exécuter les commandes de la page [Composer](https://getcomposer.org/download/) puis :

```bash
$ sudo mkdir -p /usr/local/bin
$ sudo mv composer.phar /usr/local/bin/composer
```

Composer vous permet d'installer des outils exécutables en ligne de commande (comme `phpcs` ou `phpunit` par exemple).
Mais pour pouvoir les exécuter, vous devez ajouter la ligne suivante dans votre fichier `~/.profile` :

```bash
PATH="$PATH:$HOME/.composer/vendor/bin"
```

Au prochain démarrage de votre terminal, les outils du dossier `~/.composer/vendor/bin` seront disponibles.

## Dossiers et fichiers

Le fichier `composer.json` contient les dépendances du projet installées par le développeur.

Le fichier `composer.lock` contient la liste complète des dépendances du projet (les dépendances des dépendances aussi donc !).

Le dossier `vendor/` contient tous les paquets installés par Composer. Ce dossier peut-être supprimé et entièrement reconstitué si le fichier `composer.json` ou  le fichier `composer.lock` existent.

Le fichier `vendor/autoload.php` permet l'autoloading de tous les paquets installés avec Composer.

## Autoloading

L'autoloading est un système qui permet de charger automatiquement un fichier PHP.

Si on veut que notre application puisse charger automatiquement les fichiers PHP du dossier `src/`, il faut ajouter une clé `autoload` dans le fichier `composer.json` :

```json
"autoload": {
    "psr-4": {
        "": "src/"
    }
}
```

puis demander à Composer de générer un nouveau fichier d'autoloading :

```bash
$ composer dump-autoload
```

et enfin inclure le code d'autoloading dans notre application en y ajoutant la ligne suivante :

```php
require_once __DIR__.'../vendor/autoload.php';
```

Si les fichiers PHP du dossier `src/` sont dans un namespace particulier (`Foo` par exemple), l'autoloading doit être adapté :

```json
"autoload": {
    "psr-4": {
        "Foo\\": "src/"
    }
}
```

NB Ajoutez bien une virgule `,` après la dernière accolade fermante `}` si le bloc `autoload` n'est pas le dernier.

## Forcer la version de PHP

Si la version de PHP sur votre poste de dev et sur la prod sont différentes, vous pouvez :

1. faire un peu d'admin sys et installer les deux versions sur votre poste de dev
2. ou faire croire à Composer que vous utilisez une version spécifique de PHP.

Nous allons voir comment réaliser la deuxième solution.

Dans le fichier `composer.json`, rajoutez le bloc ci-dessous :

```json
"config": {
    "platform": {
        "php": "X.Y.Z"
    }
}
```

Et spécifiez la version de PHP que vous voulez à la place de `X.Y.Z`.

Exemple :

```json
"config": {
    "platform": {
        "php": "7.4.20"
    }
}
```

NB Ajoutez bien une virgule `,` après la dernière accolade fermante `}` si le bloc `config` n'est pas le dernier.

Après ça lancez la commande `composer update` pour mettre à jour vos packages par rapport à la contrainte de version de PHP.
Si Composer vous dit qu'aucun package répondant aux contraintes n'est installable, dans le fichirer `composer.json` downgradez le numéro de version majeur du package qui pose problème et relancez `composer update`.
Downgradez jusqu'à ce que ça passe.

```diff-json
  "require": {
-     "symfony/var-dumper": "^6.0",
+     "symfony/var-dumper": "^5.0",
-     "symfony/yaml": "^6.0"
+     "symfony/yaml": "^5.0"
  }
```

## Commandes

Tester l'installation :

```bash
$ composer -v
```

Chercher un paquet :

```bash
$ composer search  [mot-clé]
```

Obtenir des infos sur un paquet non installé :

```bash
$ composer info -a [nom du paquet]
```

Installer un paquet :

```bash
$ composer require [nom du paquet]
```

Par exemple, installer symfony/var-dumper :

```bash
$ composer require symfoy/var-dumper
```

Installer un paquet pour l'environnement de développement :

```bash
$ composer require --dev [nom du paquet]
```

Installer une version précise d'un paquet :

```bash
$ composer require [nom du paquet] [numéro de version]
```

Par exemple, installer précisément la version `1.3.0` de twig :

```bash
$ composer require twig/twig 1.30
```

Installer une version minimum d'un paquet :

```bash
$ composer require [nom du paquet] ~[numéro de version]
```

Par exemple, installer la version `1.35` minimum (mais pas la version `2.x`) de twig :

```bash
$ composer require twig/twig ~1.35
```

Supprimer un paquet installé :

```bash
$ composer remove [nom du paquet]
```

Installer les paquets requis (« required ») dans le fichier `composer.json` ou  le fichier `composer.lock` :

```bash
$ composer install
```

Mettre à jour tous les paquets installés :

```bash
$ composer update
```

Mettre à jour un paquet installé :

```bash
$ composer update [nom du paquet]
```

Générer un nouveau fichier d'autoloading :

```bash
$ composer dump-autoload
```

Mettre à jour composer :

```bash
$ composer self-update
```

## Symfony Flex

Symfony Flex est un plugin Composer qui permet d'installer Symfony et des Bundles.

Cet outil est utilisable à partir de Symfony 3.4.

## Packagist

Le site [packagist.org](https://packagist.org/) est un annuaire communautaire de tous les packages disponible avec Composer.

C'est donc aussi un moteur de recherche de packages PHP qui peut remplacer la commande `composer search [mot-clé]`.

Si vous créer un package et souhaitez le diffuser, il faudra déclarer son existence sur ce site pour que la communauté PHP puisse le trouver et l'installer avec Composer.

## Trouble shooting

### `composer install` refuse d'installer les composants

Vous venez de cloner un repository git et vous lancez la commande `composer install`.
Mais au lieu d'installer les packages, composer se plaint de ne rien pouvoir faire car les packages ne sont pas compatibles : `Your lock file does not contain a compatible set of packages.`.

Exemple de message d'erreur :

```bash
$ composer install
Installing dependencies from lock file (including require-dev)
Verifying lock file contents can be installed on current platform.
Your lock file does not contain a compatible set of packages. Please run composer update.

  Problem 1
    - laminas/laminas-code is locked to version 3.4.1 and an update of this package was not requested.
    - laminas/laminas-code 3.4.1 requires php ^7.1 -> your php version (8.0.6) does not satisfy that requirement.
  Problem 2
    - laminas/laminas-code 3.4.1 requires php ^7.1 -> your php version (8.0.6) does not satisfy that requirement.
    - friendsofphp/proxy-manager-lts v1.0.5 requires laminas/laminas-code ~3.4.1|^4.0 -> satisfiable by laminas/laminas-code[3.4.1].
    - friendsofphp/proxy-manager-lts is locked to version v1.0.5 and an update of this package was not requested.
```

Ne paniquez pas, les indications sont dans le message d'erreur : `Please run composer update.`.

Faites donc un petit `composer update`, ça devrait régler l'affaire.

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

