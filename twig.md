# Twig

Twig est un moteur de « template ». Un « template » est un fichier dans lequel on définit la façon dont des données vont être affichées.

Dans le modèle « MVC », le template correspond au « V », la vue (View en anglais).

## Sans framework (en PHP brut)

    my_project/
        public/
            index.php
        templates/
            partials/
                _*.html.twig
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

### Configuration du cache

Le cache permet de stocker le rendu PHP du code Twig.
C'est une optimization qui doit être appliquée quand le code est en production.

Modifier la partie `new Twig_Environment($loader);` du fichier `public/hello-twig.php` :

    <?php

    $twig = new Twig_Environment($loader, [
        'cache' => __DIR__.'/../var/cache',
    ]);

## Avec Symfony 3.4

Twig est préinstallé dans Symfony 3.4.

Les vues globales sont stockées dans le dossier `app/Resources/views/`.

Les vues spécifiques à un bundle sont stockées dans le dossier `Resources/views/` du bundle. Exemple `src/AppBundle/Resources/views/`.

Les vues générées par le générateur de CRUD de Doctrine sont stockées dans le dossier `app/Resources/views/`.

## Avec le framework Silex

### Structure du dossier

Voir [silex.md](silex.md).

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

## Syntaxe

### Notions de base

Tous les blocs de code twig sont encadrés d'accolades `{}`.

Il y a trois types de blocs de code twig :

1. `{# #}` : pour les commentaires
2. `{{ }}` : pour afficher une variable
3. `{% %}` : pour effectuer une action

### Les commentaires

Noter un commentaire :

    {# ceci est un commentaire #}

Le bloc accepte les commentaires multilignes.

### Afficher une variable

Pour afficher une variable, il faut utiliser deux paires d'accolades `{{ }}`.

Afficher la variable `foo` :

    {{ foo }}

Afficher la valeur associée à la clé alphanumérique `foo` du tableau `bar` :

    {{ bar.foo }}

Afficher l'attribut `foo` de la variable de type objet `bar` :

    {{ bar.foo }}

Afficher la valeur renvoyée par la méthode `foo` de la variable de type objet `bar` :

    {{ bar.foo() }}

NB La notation est la même pour accéder à une clé d'un tableau ou à un attribut d'un objet, on utilise le point `.`.

### Les boucles

Boucler sur le tableau `items` qui contient des chaînes de caractères :

    {% for item in items %}
        {{ item }}<br />
    {% endfor %}

Boucler sur le tableau `items` qui contient des tableaux avec des clé alphanumériques :

    {% for item in items %}
        {{ item.id }}<br />
        {{ item.name }}<br />
    {% endfor %}

Boucler sur le tableau `items` qui contient des objets :

    {% for item in items %}
        {{ item.id }}<br />
        {{ item.name }}<br />
    {% endfor %}

Boucler sur le tableau `items` qui contient des objets et appeler ses méthodes :

    {% for item in items %}
        {{ item.getId() }}<br />
        {{ item.getName() }}<br />
    {% endfor %}

Récupérer « un à un » les éléments d'une requête SQL :

    {% for item in items.fetch() %}
        {{ item.id }}<br />
        {{ item.name }}<br />
    {% endfor %}

NB La notation est la même pour accéder à une clé d'un tableau ou à un attribut d'un objet, on utilise le point `.`.

### Les structures conditionnelles (blocs `if`)

Afficher la variable `foo` si la variable  `bar` est égal à `true` :

    {% if bar %}
        {{ foo }}
    {% endif %}

Afficher la variable `foo` si l'attribut `bar` de la variable  `baz` est égal à `true` :

    {% if baz.bar %}
        {{ foo }}
    {% endif %}

Afficher la variable `foo` si l'attribut `bar` de la variable  `baz` est égal à `true` :

    {% if baz.bar %}
        {{ foo }}
    {% endif %}

### L'héritage (ou l'extension) de templates

Avec Twig, il n'est pas nécessaire d'utiliser `include()` pour composer un template à partir de plusieurs morceaux.

L'idée est de créer un template initial sur lequel tous les autres templates seront basés.
Ceci permet de ne pas répéter de code et de pouvoir se passer des `include()` de PHP.

Définir un template parent `templates/base.html.twig` que les templates enfants pourront étendre :

    <!DOCTYPE html>
    <html>
        <head>
            <meta charset="utf-8">
            <meta name="viewport" content="width=device-width, initial-scale=1, shrink-to-fit=no">
            <title>{% block title %}{% endblock %}</title>

            {% block css_head %}
            <link rel="stylesheet" href="https://stackpath.bootstrapcdn.com/bootstrap/4.1.0/css/bootstrap.min.css" integrity="sha384-9gVQ4dYFwwWSjIDZnLEWnxCjeSWFphJiwGPXr1jddIhOegiu1FwO5qRGvFXOdJZ4" crossorigin="anonymous">
            <link rel="stylesheet" href="/css/main.css" />
            {% endblock %}

            {% block js_head %}
            <script src="https://code.jquery.com/jquery-3.3.1.slim.min.js" integrity="sha384-q8i/X+965DzO0rT7abK41JStQIAqVgRVzpbzo5smXKp4YfRvH+8abtTE1Pi6jizo" crossorigin="anonymous"></script>
            <script src="https://cdnjs.cloudflare.com/ajax/libs/popper.js/1.14.0/umd/popper.min.js" integrity="sha384-cs/chFZiN24E4KMATLdqdvsezGxaGsi4hLGOzlXwp5UZB1LY//20VyM2taTB4QvJ" crossorigin="anonymous"></script>
            <script src="https://stackpath.bootstrapcdn.com/bootstrap/4.1.0/js/bootstrap.min.js" integrity="sha384-uefMccjFJAIv6A+rW+L4AHf99KvxDjWSu1z9VI8SKNVmz4sk7buKt/6v9KI65qnm" crossorigin="anonymous"></script>
            <script src="/js/main.js"></script>
            {% endblock %}
        </head>
        <body>
            {% block body %}{% endblock %}

            {% block css_body %}{% endblock %}

            {% block js_body %}{% endblock %}
        </body>
    </html>

Créer un template enfant `templates/foo.html.twig` qui hérite du template `templates/base.html.twig` :

    {% extends 'base.html.twig' %}

    {% block title %}Foo{% endblock %}

    {% block body %}
        <h1>Foo</h1>
        <p>Vous êtes sur la page Foo</p>
    {% endblock %}

Créer un template enfant `templates/bar.html.twig` qui hérite du template `templates/base.html.twig`, et qui surcharge  les blocs `css_head` et `js_head` :

    {% extends 'base.html.twig' %}

    {% block title %}Foo{% endblock %}

    {# surcharge du bloc css_head #}
    {# intégration de materialize au lieu de bootstrap #}
    {% block css_head %}
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/materialize/1.0.0-beta/css/materialize.min.css">
    <link rel="stylesheet" href="/css/main.css" />
    {% endblock %}

    {# surcharge du bloc js_head #}
    {# intégration de materialize au lieu de bootstrap #}
    {% block js_head %}
    <script src="https://cdnjs.cloudflare.com/ajax/libs/materialize/1.0.0-beta/js/materialize.min.js"></script>
    <script src="/js/main.js"></script>
    {% endblock %}

    {% block body %}
        <h1>Foo</h1>
        <p>Vous êtes sur la page Foo</p>
    {% endblock %}

### Les includes

NB Il est rare d'avoir besoin de la fonction `include()` avec Twig.

Comme en PHP, il est possible de faire des includes.
La fonctionnalité `include()` est utile si plusieurs templates enfants utilisent un même bloc de code.
On parle alors de template `partial` (partiel).

Définir le bloc de code commun dans le fichier `templates/partials/_items.html.twig` :

    {% if items %}
		<ul>
				{% for item in items %}
            <li>{{ item }}</li>
				{% endfor %}
		</ul>
    {% endif %}

Utiliser le template `partial` dans le template `templates/foo.html.twig` :

    {% include('partials/_items.html.twig') %}

### Les filtres

#### Échappement de variables

L'échappement de variable se fait avec le filtre  `escape()`.
C'est l'équivalent de `htmlentities()`.

Mais `escape()` ne se limite pas à l'échappement de code HTML.
On peut aussi échapper des variables pour :

- du code HTML
- du code JS
- du code CSS
- des URL
- des attributs HTML

Échapper la variable `foo` qui peut être dangereuse dans du HTML :

    <p>{{ foo|escape }}</p>

Ou plus court :

    <p>{{ foo|e }}</p>

Échapper la variable `foo` qui peut être dangereuse dans du JS :

    var foo = '{{ foo|e('js') }}';

Échapper la variable `foo` qui peut être dangereuse du CSS :

    body {
      color: {{ foo|e('css') }};
    }

Échapper la variable `foo` qui peut être dangereuse dans une URL :

    <a href="{{ foo|e('url') }}>foo</a>

Échapper la variable `foo` qui peut être dangereuse dans un attribut HTML :

    <div id="{{ foo|e('html_attr') }}></div>

#### Formatage de dates

Il est possible de manipuler le format d'affichage des dates avec le filtre `date()`.

Afficher la date `create_date` au format `JJ/MM/AAAA` :

    {{ create_date|date("d/m/Y") }}

Afficher la date au format `MM/JJ/AAAA` :

    {{ create_date|date("m/d/Y") }}

### `verbatim` ou comment afficher du code twig (ou du code comprenant des accolades `{}`)


    {% verbatim %}
        {% for item in items %}
            {{ item }}
        {% endfor %}
    {% endverbatim %}

## Doc

- [Documentation - Twig - The flexible, fast, and secure PHP template engine](https://twig.symfony.com/doc/2.x/)

### Back-end

- [Installation - Documentation - Twig - The flexible, fast, and secure PHP template engine](https://twig.symfony.com/doc/2.x/installation.html)
- [Twig for Developers - Documentation - Twig - The flexible, fast, and secure PHP template engine](https://twig.symfony.com/doc/2.x/api.html)

### Front-end

- [Twig for Template Designers - Documentation - Twig - The flexible, fast, and secure PHP template engine](https://twig.symfony.com/doc/2.x/templates.html)
- [for - Documentation - Twig - The flexible, fast, and secure PHP template engine](https://twig.symfony.com/doc/2.x/tags/for.html)
- [if - Documentation - Twig - The flexible, fast, and secure PHP template engine](https://twig.symfony.com/doc/2.x/tags/if.html)
- [verbatim - Documentation - Twig - The flexible, fast, and secure PHP template engine](https://twig.symfony.com/doc/2.x/tags/verbatim.html)
- [Filters - Documentation - Twig - The flexible, fast, and secure PHP template engine](https://twig.symfony.com/doc/2.x/filters/index.html)
- [escape - Documentation - Twig - The flexible, fast, and secure PHP template engine](https://twig.symfony.com/doc/2.x/filters/escape.html)

### Localisation / Internationalisation

- [PHP: date - Manual](http://php.net/manual/en/function.date.php)
- [PHP: number_format - Manual](http://php.net/manual/en/function.number-format.php)
