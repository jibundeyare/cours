# Silex

[Silex](https://silex.symfony.com/) est un micro framework PHP basé sur des composants du framework Symfony. Ces deux frameworks sont développés par la société [SensioLabs](https://sensiolabs.com/fr).

## Structure du dossier

Lors de la création d'un nouveau projet, il est recommandé de créer l'arborescence suivante :

    my_project/             <= dossier du projet
        config/             <= vos fichiers de config
            *.yml
        data/               <= vos fichiers de données
            *.sql
        public/             <= document root (anciennement appelé web)
            css/            <= vos feuilles de style
                *.css
            fonts/          <= vos typos
            img/            <= vos images
                *.gif
                *.jpg
                *.png
            js/             <= vos fichiers JavaScript
                *.js
            node_modules/   <= paquets gérés par npm
            sass/           <= vos feuilles de style
                *.sass
                *.scss
            .htaccess       <= config pour apache
            index.php       <= le point d'entrée de votre application
            package.json    <= liste des paquets gérés par npm
            package.lock    <= liste des paquets gérés par npm
        src/                <= vos fichiers PHP
            *.php
        templates/          <= vos templates (anciennement appelé views)
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

Note : le document root est le dossier qui est rendu accessible par le serveur web. Tout ce qui se trouve dans ce dossier et ses sous-dossier est accessible avec un navigateur web.

## Installation

Dans un terminal, si vous n'êtes pas déjà dans la dossier racine de votre projet, taper :

    cd [dossier du projet web]

Pour installer les paquet :

    composer require silex/silex ~2.0
    composer require symfony/debug ~3.4
    composer require symfony/var-dumper ~3.4
    composer require symfony/yaml ~3.4
    composer require doctrine/dbal ~2.0
    composer require jibundeyare/silex-php-view

## Création de l'application

Le fichier `public/index.php` est le point d'entrée de l'application.

Pour créer l'application, insérer le code suivant dans le fichier `public/index.php` :

    <?php

    use Silex\Application;
    use SilexPhpView\ViewServiceProvider;
    use Symfony\Component\Debug\Debug;

    require __DIR__.'/../vendor/autoload.php';

    // Debug::enable();

    $app = new Application();

    // $app['debug'] = true;

    $app->register(new ViewServiceProvider(), [
        'view.path' => __DIR__.'/../templates',
    ]);

    // home
    $app->get('/', function() use($app) {
        $message = 'Hello Silex!';

        return $app['view']->render('home.php', [
            'message' => $message,
        ]);
    });

    $app->run();

## Template de la page d'accueil

Pour créer le template de la page d'accueil, insérer le code suivant dans le fichier `templates/home.php` :

    <!DOCTYPE html>
    <html lang="fr">
      <head>
        <meta charset="utf-8" />
        <title><?= $message ?></title>
      </head>
      <body>
          <h1><?= $message ?></h1>
      </body>
    </html>

## Test de l'application

Depuis le terminal, lancer un serveur web de développement :

    php -S localhost:8000 -t public

puis dans un navigateur web, entrer l'url :

    http://localhost:8000

## Mode debug

Pour activater le mode debug, enlever les slashs `//` devant les lignes `Debug::enable();` et `$app['debug'] = true;` dans le fichier `public/index.php` afin d'obtenir :

    Debug::enable();            // <= modif

    $app = new Application();

    $app['debug'] = true;       // <= modif

Attention : ce mode doit impérativement être désactivé quand l'application est déployée en production. Pour désactiver le mode  debug, il suffit de remettre les slashs `//` supprimés.

## Ajout d'une nouvelle page

Pour ajouter une nouvelle page, il faut ajouter :

- une nouvelle route
- un nouveau contrôleur
- un nouveau template

### Ajout d'une nouvelle route et d'un nouveau contrôleur

Pour ajouter la page `contact`, modifier la fin du fichier `public/index.php` afin d'obtenir :

    // home
    $app->get('/', function() use($app) {
        $message = 'Hello Silex!';

        return $app['view']->render('home.php', [
            'message' => $message,
        ]);
    });

    // contact                                          // <= modif
    $app->get('/contact', function() use($app) {        // <= modif
        return $app['view']->render('contact.php', [    // <= modif
        ]);                                             // <= modif
    });                                                 // <= modif

    $app->run();

La nouvelle route correspond à `'/contact'` et le nouveau contrôleur correspond à `function() use($app) { /* ... */ }`.

## Ajout d'un nouveau template

Pour créer le template de la page `contact`, insérer le code suivant dans le fichier `templates/contact.php` :

    <!DOCTYPE html>
    <html lang="fr">
      <head>
        <meta charset="utf-8" />
        <title>Contact</title>
      </head>
      <body>
        <h1>Contact</h1>
      </body>
    </html>

## Lien vers une page

Il suffit d'insérer la route dans l'attribut `href` d'une balise `a`.

Par exemple, pour créer un lien qui pointe vers la page `contact`  :

    <a href="/contact">la page contact</a>

Ou pour créer un lien qui pointe vers la page d'accueil :

    <a href="/">la page d'accueil</a>

## Ajout d'une feuille de style et de javascript

Tous les fichiers `css` et `js` doivent être stockés dans un sous-dossier de `public`.

### Votre feuille de style

Créer le dossier `public/css` s'il n'existe pas encore.

Créer votre feuille de style `public/css/main.css`.

Pour intégrer votre feuille de style dans vos templates, modifier le début du document HTML afin d'obtenir :

    <head>
      <meta charset="utf-8" />
      <title>Contact</title>
      <link href="/css/main.css" rel="stylesheet">
    </head>

### Votre code javascript

Créer le dossier `public/js` s'il n'existe pas encore.

Créer votre code javascript dans `public/js/main.js`.

Pour intégrer votre code javascript dans vos templates, modifier la fin du document HTML afin d'obtenir :

        <script src="/js/main.js"></script>
      </body>
    </html>

### Installation et intégration de jQuery

Créer le dossier `public/js` s'il n'existe pas encore.

Téléchargez le fichier [jquery-3.2.1.min.js](https://code.jquery.com/jquery-3.2.1.min.js) dans le dossier `public/js`.

Vous devriez avoir le fichier suivant : `public/js/jquery-3.2.1.min.js`.

Pour utiliser jQuery dans vos templates, modifier la fin du document HTML afin d'obtenir :

        <script src="/js/jquery-3.2.1.min.js"></script>
        <script src="/js/main.js"></script>
      </body>
    </html>

Attention :

- votre code javascript doit être intégré après jQuery

### Installation et intégration de Bootstrap

Téléchargez [bootstrap-3.3.7-dist.zip](https://github.com/twbs/bootstrap/releases/download/v3.3.7/bootstrap-3.3.7-dist.zip) et dézipper l'archive dans le dossier `public`.

Vous devriez avoir les trois dossiers suivants :

- `public/bootstrap-3.3.7-dist/css`
- `public/bootstrap-3.3.7-dist/fonts`
- `public/bootstrap-3.3.7-dist/js`

Pour utiliser Bootstrap dans vos templates, modifier le début du document HTML afin d'obtenir :

    <head>
      <meta charset="utf-8">
      <meta http-equiv="X-UA-Compatible" content="IE=edge">
      <meta name="viewport" content="width=device-width, initial-scale=1">
      <title>Titre</title>
      <link href="/bootstrap-3.3.7-dist/css/bootstrap.min.css" rel="stylesheet">
      <link href="/bootstrap-3.3.7-dist/css/bootstrap-theme.min.css" rel="stylesheet">
      <link href="/css/main.css" rel="stylesheet">
    </head>

Attention :

- votre feuille de style doit être intégrée après Bootstrap
- pensez à adapter le titre à la page

Puis modifier la fin du document HTML afin d'obtenir :

        <script src="/js/jquery-3.2.1.min.js"></script>
        <script src="/bootstrap-3.3.7-dist/js/bootstrap.min.js"></script>
        <script src="/js/main.js"></script>
      </body>
    </html>

Attention :

- jQuery doit être intégré avant Bootstrap
- votre code javascript doit être intégré après Bootstrap

## Stockage des identifiants de base de données

### Création du fichier de config

Pour stocker les identifiants dans un fichier à part, insérer le code suivant dans le fichier `config/db.yml.dist` :

    host: ~
    dbname: ~
    user: ~
    password: ~
    driver: pdo_mysql
    charset: utf8mb4

puis copier le fichier `config/db.yml.dist` vers `config/db.yml` et l'adapter avec les identifiants :

    host: 127.0.0.1
    dbname: my_project
    user: my_user
    password: my_password
    driver: pdo_mysql
    charset: utf8mb4

### Lecture du fichier de config

Pour lire le fichier de config, adapter le fichier `public/index.php` afin d'obtenir :

    use Symfony\Component\Debug\Debug;
    use Symfony\Component\Yaml\Yaml;                                    // <= modif

    require __DIR__.'/../vendor/autoload.php';

    Debug::enable();

    $connectionParams = Yaml::parseFile(__DIR__.'/../config/db.yml');   // <= modif

## Autoloading

Pour activer l'autoloading, ajouter le bloc suivant au fichier `composer.json` :

    "autoload": {
        "psr-4": {"": "src/"}
    }

## Skeleton

Voir [jibundeyare/silex-skeleton](https://github.com/jibundeyare/silex-skeleton) pour une version opérationnelle.

Ou voir [silexphp/Silex-Skeleton](https://github.com/silexphp/Silex-Skeleton) qui est le skeleton officiel.

## Doc

- [Homepage - Silex - The PHP micro-framework based on the Symfony Components](https://silex.symfony.com/)
- [The Debug Component (Symfony 3.4 Docs)](https://symfony.com/doc/3.4/components/debug.html)
- [The VarDumper Component (Symfony 3.4 Docs)](http://symfony.com/doc/3.4/components/var_dumper.html)
- [The Yaml Component (Symfony 3.4 Docs)](http://symfony.com/doc/3.4/components/yaml.html)
- [Welcome to Doctrine DBAL’s documentation! — Doctrine DBAL 2 documentation](http://docs.doctrine-project.org/projects/doctrine-dbal/en/latest/)
- [jibundeyare/silex-php-view: A minimal php view service for silex framework](https://github.com/jibundeyare/silex-php-view)
- [Bootstrap · The world's most popular mobile-first and responsive front-end framework.](https://getbootstrap.com/docs/3.3/)
- [jQuery](http://jquery.com/)
- [jibundeyare/silex-skeleton](https://github.com/jibundeyare/silex-skeleton)
- [silexphp/Silex-Skeleton: A skeleton to get started with Silex](https://github.com/silexphp/Silex-Skeleton)
