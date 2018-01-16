# Code sniffer

Voir [Code style](code-style.md).

## Installation

Dans un terminal, si vous n'êtes pas déjà dans la dossier racine de votre projet, taper :

    cd [dossier du projet web]

Pour installer le paquet :

    composer require --dev squizlabs/php_codesniffer

## Commandes

Vérifier le code style d'un fichier php :

    vendor/bin/phpcs [nom du fichier]

Vérifier le code style d'un dossier contenant des fichiers php :

    vendor/bin/phpcs [nom du dossier]

Lister les codes styles disponibles :

    vendor/bin/phpcs -i

Améliorer le code style d'un fichier php :

    vendor/bin/phpcbf [nom du fichier]

Améliorer le code style d'un dossier contenant des fichiers php :

    vendor/bin/phpcbf [nom du dossier]

## Doc

- [squizlabs/PHP_CodeSniffer · GitHub](https://github.com/squizlabs/PHP_CodeSniffer)
