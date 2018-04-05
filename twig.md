# Twig

Twig est un moteur de « template ». Un « template » est un fichier dans lequel on définit la façon dont des données vont être affichées.

Dans le modèle « MVC », le template correspond au « V », la vue (View en anglais).

## Avec Symfony 3.4

Twig est préinstallé dans Symfony 3.4.

Les vues globales sont stockées dans le dossier `app/Resources/views/`.

Les vues spécifiques à un bundle sont stockées dans le dossier `Resources/views/` du bundle. Exemple `src/AppBundle/Resources/views/`.

Les vues générées par le générateur de CRUD de Doctrine sont stockées dans le dossier `app/Resources/views/`.

## Avec le framework Silex

### Installation

Dans un terminal, si vous n'êtes pas déjà dans la dossier racine de votre projet, taper :

    cd [dossier du projet web]

Pour installer le paquet :

    composer require twig/twig ~2.0

### Utilisation

Ouvrir le fichier `public/index.php` puis ajouter :

    $app->register(new Silex\Provider\TwigServiceProvider(), [
        'twig.path' => __DIR__.'/../templates',
    ]);

    $app->get('/', function() use($app) {
        $greeting = 'Hello Twig!';

        return $app['twig']->render('hello-twig.html.twig'[
            'greeting' => $greeting,
        ]);
    });

Ouvrir le fichier `templates/hello-twig.html.twig` puis ajouter :

    <!DOCTYPE html>
    <html>
        <head>
            <meta charset="utf-8">
            <title>{{ greeting }}</title>
        </head>
        <body>
            <h1>{{ greeting }}</h1>
        </body>
    </html>

## Sans framework (en PHP brut)

    my_project/
        public/
            index.php
        templates/
            *.html.twig
            index.html.twig
        var/
            cache/

### Installation

Dans un terminal, si vous n'êtes pas déjà dans la dossier racine de votre projet, taper :

    cd [dossier du projet web]

Pour installer le paquet :

    composer require twig/twig ~2.0

### Utilisation

Ouvrir le fichier `public/hello-twig.php` puis ajouter :

    <?php

    require __DIR__.'/../vendor/autoload.php';

    $loader = new Twig_Loader_Filesystem(__DIR__.'/../templates');
    $twig = new Twig_Environment($loader);

    $greeting = 'Hello Twig!';

    $template = $twig->load('hello-twig.html.twig');
    echo $template->render([
        'greeting' => $greeting,
    ]);

Ouvrir le fichier `templates/hello-twig.html.twig` puis ajouter :

    <!DOCTYPE html>
    <html>
        <head>
            <meta charset="utf-8">
            <title>{{ greeting }}</title>
        </head>
        <body>
            <h1>{{ greeting }}</h1>
        </body>
    </html>

## Doc

- [Documentation - Twig - The flexible, fast, and secure PHP template engine](https://twig.symfony.com/doc/2.x/)
- [Installation - Documentation - Twig - The flexible, fast, and secure PHP template engine](https://twig.symfony.com/doc/2.x/installation.html)
- [Twig for Developers - Documentation - Twig - The flexible, fast, and secure PHP template engine](https://twig.symfony.com/doc/2.x/api.html)
- [Twig for Template Designers - Documentation - Twig - The flexible, fast, and secure PHP template engine](https://twig.symfony.com/doc/2.x/templates.html)
- [for - Documentation - Twig - The flexible, fast, and secure PHP template engine](https://twig.symfony.com/doc/2.x/tags/for.html)
- [if - Documentation - Twig - The flexible, fast, and secure PHP template engine](https://twig.symfony.com/doc/2.x/tags/if.html)
- [Filters - Documentation - Twig - The flexible, fast, and secure PHP template engine](https://twig.symfony.com/doc/2.x/filters/index.html)
- [verbatim - Documentation - Twig - The flexible, fast, and secure PHP template engine](https://twig.symfony.com/doc/2.x/tags/verbatim.html)
- [PHP: date - Manual](http://php.net/manual/en/function.date.php)
- [PHP: number_format - Manual](http://php.net/manual/en/function.number-format.php)
