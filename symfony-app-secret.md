# Symfony `APP_SECRET`

## Le `APP_SECRET` du fichier `.env`

Pour regénérer le `APP_SECRET` de votre fichier `.env` (ou de votre `.env.local`), vous pouvez utiliser la méthode suivante qui génèrera une chaîne de 32 caractères en hexadécimal tirée au hasard.

Dans un terminal, tapez :

    php -a
    $bytes = random_bytes(16);
    echo bin2hex($bytes).PHP_EOL;
    exit

Le résultat devrait ressembler à ceci :

    $ php -a
    Interactive shell

    php > $bytes = random_bytes(16);
    php > echo bin2hex($bytes).PHP_EOL;
    9c56d22b9c9b92a9a13fb8a10d3aa008
    php > exit

Vouc pouvez copier la chaîne de caractère `9c56d22b9c9b92a9a13fb8a10d3aa008` et l'utiliser comme nouveau `APP_SECRET`.

**Sinon vous pouvez aussi utiliser un outil en ligne (qui génère 40 caractères et non 32) : [Symfony 2 Secret Generator - nux.net](http://nux.net/secret).**

