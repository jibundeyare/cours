# Php application

## Notions

- programmation procédurale
- programmation orientée objet (POO)
- architecture MVC (Modèle, Vue, Contrôleur)

## Outils

- « composer » permet d'installer des paquets
- « symfony var dumper » permet d'inspecter une variable
- « code sniffer » permet de valider le code style

## Arborescence d'un projet

    my_project/
        config/             <= vos fichiers de config
            *.yml
        data/               <= vos fichiers de données
            *.sql
        public/             <= document root
            .htaccess       <= config pour apache
            *.php           <= les pages de votre applications
            index.php       <= le point d'entrée de votre application
        src/                <= vos fichiers php
            *.php
        templates/          <= vos templates
            *.php
            *.twig
        var/
            cache/          <= stockage du cache
            logs/           <= stockage des logs
        vendor/             <= paquets gérés par composer
        .gitignore          <= liste des fichiers que git doit ignorer
        composer.json       <= liste des paquets gérés par composer
        composer.lock       <= liste des paquets gérés par composer
        README.md           <= documentation du projet

## Server web de développement

Dans un terminal, si vous n'êtes pas déjà dans la dossier racine de votre projet, taper :

    cd [dossier du projet web]

Pour lancer le serveur web, taper :

    php -S localhost:8000 -t public

Puis dans barre d'adresse d'un navigateur web, taper :

    http://localhost:8000/

## Commandes

Afficher la version utilisée :

    php -v

Conseil : si vous utilisez encore Php 5, mettez à jour votre système et utilisez Php 7.

Afficher la liste des fichier `.ini` utilisés :

    php --ini

Vérifier la syntaxe d'un fichier php :

    php -l [nom du fichier]

Afficher une liste d'info complète :

    php -i

Lancer le serveur web de développement avec le dossier actuel comme document root :

    php -S localhost:8000

Lancer le serveur web de développement avec le sous-dossier `public` comme document root :

    php -S localhost:8000 -t public

## Communication avec la base de données

Je recommande fortement l'utilisation de Doctrine DBAL. Pour en savoir plus, voir [doctrine-dbal.md](doctrine-dbal.md).

## Doc

- [PHP: Hypertext Preprocessor](http://php.net/)
- [PHP: The Right Way](http://www.phptherightway.com/)
- [PHP Best Practices: a short, practical guide for common and confusing PHP tasks](https://phpbestpractices.org/)
- [PHP: Options - Manual](http://php.net/manual/en/features.commandline.options.php)
- [SitePoint – Learn HTML, CSS, JavaScript, PHP, Ruby & Responsive Design](https://www.sitepoint.com/)
