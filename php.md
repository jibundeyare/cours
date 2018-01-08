# Php

## Concepts

- la programmation procédurale
- la programmation orientée objet (POO)
- le modèle MVC (Modèle, Vue, Contrôleur)

## Outils

- « composer » permet d'installer des paquets
- « symfony var dumper » permet d'inspecter une variable
- « code sniffer » permet de valider le code style

## Arborescence d'un projet

    my_project/
        config/
            *.yml
        public/
            .htaccess
            *.php
            index.php
        src/
            *.php
        templates/
            *.twig
        var/
            cache/
        vendor/
        composer.json
        composer.lock
        README.md

## Syntaxe

`=` est impératif (c'est un ordre).

`==` est interrogatif (c'est une question).

## Commandes

Afficher la version utilisée :

    php -v

Conseil : si vous utilisez encore php 5, mettez à jour votre système et utilisez php 7.

Afficher la liste des fichier `.ini` utilisés :

    php --ini

Vérifier la syntaxe d'un fichier php :

    php -l [nom du fichier]

Afficher une liste d'info complète :

    php -i

Lancer le serveur web de développement :

    php -S localhost:8000 -t public

## Doc

- [PHP: Hypertext Preprocessor](http://php.net/)
- [PHP: The Right Way](http://www.phptherightway.com/)
- [PHP Best Practices: a short, practical guide for common and confusing PHP tasks](https://phpbestpractices.org/)
- [PHP: Options - Manual](http://php.net/manual/en/features.commandline.options.php)
- [SitePoint – Learn HTML, CSS, JavaScript, PHP, Ruby & Responsive Design](https://www.sitepoint.com/)
