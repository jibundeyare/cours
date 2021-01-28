# PHP Coding Standards Fixer

Ces trois documents forment un ensemble :

- [code-sniffer.md](code-sniffer.md)
- [code-style.md](code-style.md)
- [php-cs-fixer.md](php-cs-fixer.md)

PHP Coding Standards Fixer est un outil qui réécrit votre code en améliorant le code style.

Pour commencer, `squizlabs/php_codesniffer` est indiqué mais par la suite, pour travailler avec le framework Symfony `friendsofphp/php-cs-fixer` est un meilleur choix.

## Installation

Dans un terminal, depuis n'importe quel dossier :

    composer global require friendsofphp/php-cs-fixer

### Windows

Puis vous devez rajouter ce chemin dans vos variables d'environnement :

    C:\Users\<user>\AppData\Roaming\Composer\vendor\bin

Attention : remplacez `<user>` par votre nom d'utilisateur.

### Linux et MacOS

Puis vous devez rajouter cette ligne dans le fichier `~/.bash_profile` :

    PATH="$PATH:$HOME/.composer/vendor/bin"

## Utilisation

Corriger le code PHP d'un dossier :

    php-cs-fixer fix /chemin/du/dossier

Corriger le code PHP d'un fichier :

    php-cs-fixer fix /chemin/du/fichier.php

## Doc

- [PHP Coding Standards Fixer](https://cs.symfony.com/)

