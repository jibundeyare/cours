# Symfony `APP_SECRET`

Ce paramètre permet de crypter les données qui sont stockés dans les COOKIES.
Cela empêche qu'un autre application puisse utiliser les données des COOKIES pour usurper l'identité d'un utilisateur.

## Le `APP_SECRET` des fichiers `.env*`

### Création du secret avec PHP

Pour regénérer le `APP_SECRET` des vos fichiers `.env*` (`.env.local`, `.env.test.local`, `.env.prod.local`, etc), vous pouvez utiliser la méthode suivante qui génèrera une chaîne de 32 caractères en hexadécimal tirée au hasard.

Dans un terminal, tapez :

    php -a
    $bytes = random_bytes(20);
    echo bin2hex($bytes).PHP_EOL;
    exit

Le résultat devrait ressembler à ceci :

    $ php -a
    Interactive shell

    php > $bytes = random_bytes(20);
    php > echo bin2hex($bytes).PHP_EOL;
    07b75bafb05b176b174730cd8bb1d1a15ed6891b
    php > exit

Vouc pouvez copier la chaîne de caractère `07b75bafb05b176b174730cd8bb1d1a15ed6891b` et l'utiliser comme nouveau `APP_SECRET`.

### Création du secret avec un outil en ligne

Vous pouvez aussi utiliser un outil en ligne : [Symfony 2 Secret Generator - nux.net](http://nux.net/secret).

## Doc

- [Framework Configuration Reference (FrameworkBundle) (Symfony Docs)](https://symfony.com/doc/2.8/reference/configuration/framework.html#secret)
- [Symfony 2 Secret Generator - nux.net](http://nux.net/secret)

