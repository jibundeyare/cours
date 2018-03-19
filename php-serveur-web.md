# PHP serveur web

PHP fournit un serveur de développement. Il est utile pour pouvoir développer rapidement sans devoir configurer Apache et afficher les messages d'erreur dans le terminal.

## Getting started

Se déplacer dans le dossier du projet :

    cd www/my_project

Lancer le serveur web :

    php -S localhost:8000

Ouvrir l'url pour voir le site dans sont navigateur :

    http://localhost:8000

## Lancer un serveur dans un autre sous-dossier

Lancer le serveur dans le sous-dossier `public` :

    php -S localhost:8000 -t public

Lancer le serveur dans le sous-dossier `web` :

    php -S localhost:8000 -t public

Ouvrir l'url pour voir le site dans sont navigateur :

    http://localhost:8000

## Lancer un serveur avec un autre port

Lancer le serveur avec le port `8080` :

    php -S localhost:8080

Ouvrir l'url pour voir le site dans sont navigateur :

    http://localhost:8080

## Lancer un serveur web accessible depuis une autre machine

Lancer le serveur avec l'adresse ip `0.0.0.0` :

    php -S 0.0.0.0:8000

## Doc

- [PHP: Built-in web server - Manual](https://secure.php.net/manual/en/features.commandline.webserver.php)
