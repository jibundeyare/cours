# Code sniffer

Ces trois documents forment un ensemble :

- [code-sniffer.md](code-sniffer)
- [code-style.md](code-style.md)
- [php-cs-fixer.md](php-cs-fixer.md)

Code sniffer est un outil qui réécrit votre code en améliorant le code style.

Pour commencer, `squizlabs/php_codesniffer` est indiqué mais par la suite, pour travailler avec le framework Symfony `friendsofphp/php-cs-fixer` est un meilleur choix.

## Installation

Dans un terminal, si vous n'êtes pas déjà dans la dossier racine de votre projet, taper :

    cd [dossier du projet web]

Pour installer le paquet :

    composer require --dev squizlabs/php_codesniffer

## Utilisation

Vérifier le code style d'un fichier PHP :

    vendor/bin/phpcs [nom du fichier]

Vérifier le code style d'un dossier contenant des fichiers PHP :

    vendor/bin/phpcs [nom du dossier]

Lister les codes styles disponibles :

    vendor/bin/phpcs -i

Améliorer le code style d'un fichier PHP :

    vendor/bin/phpcbf [nom du fichier]

Améliorer le code style d'un dossier contenant des fichiers PHP :

    vendor/bin/phpcbf [nom du dossier]

## Doc

- [squizlabs/PHP_CodeSniffer · GitHub](https://github.com/squizlabs/PHP_CodeSniffer)
