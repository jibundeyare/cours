# Twig 3.x

Twig est un moteur de « template ». Un « template » est un fichier dans lequel on définit la façon dont des données vont être affichées.

Dans le modèle « MVC », le template correspond au « V », la vue (View en anglais).

## Version 2 et version 3

Depuis novembre 2019, Twig est passé en version 3.
Changement notable, le nom des classes à charger en PHP est différent.
Si vous voulez retrouver les anciens noms de classes, voir [twig-2.x.md](twig-2.x.md).

## Sans framework (en PHP brut)

### Arborescence d'un projet

Si votre document root est le sous-dossier `public` votre dossier de projet (recommandé) :

    dossier-projet/
        public/
            index.php
            *.php
        templates/
            index.html.twig
            *.html.twig
        vendor/
        composer.json
        composer.lock

Si votre document root est votre dossier de projet (non recommandé) :

    dossier-projet/
        templates/
            index.html.twig
            *.html.twig
        vendor/
        composer.json
        composer.lock
        index.php
        *.php

### Installation

Dans un terminal, si vous n'êtes pas déjà dans la dossier racine de votre projet, taper :

```bash
cd [dossier du projet web]
```

Pour installer le paquet :

```bash
composer require twig/twig ~3.0
```

### Utilisation

Ouvrir le fichier `public/hello-twig.php` puis ajouter :

```php
<?php

use Twig\Environment;
use Twig\Loader\FilesystemLoader;

// activation du système d'autoloading de Composer
require_once __DIR__.'/../vendor/autoload.php';

// instanciation du chargeur de templates
$loader = new FilesystemLoader(__DIR__.'/../templates');

// instanciation du moteur de template
$twig = new Environment($loader);

// traitement des données
$greeting = 'Hello Twig!';

// affichage du rendu d'un template
echo $twig->render('hello-twig.html.twig', [
    // transmission de données au template
    'greeting' => $greeting,
]);
```

Ouvrir le fichier `templates/hello-twig.html.twig` puis ajouter :

```html
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
```

### Tester

Dans le terminal, depuis le dossier racine du projet, lancer un serveur web de développement :

```bash
php -S localhost:8000 -t public
```

Dans un navigateur, ourvir l'url [http://localhost:8000/hello-twig.php](http://localhost:8000/hello-twig.php).

### Configuration

#### Pour la phase de dev

Il est recommandé d'activer :

- le mode debug
- le mode de variables strictes

Le mode debug permet d'utiliser la fonction `dump()` dans un template Twig pour inspecter le contenu d'une variable.

Le mode de variables strictes permet d'afficher une erreur si vous utilisez une variable qui n'a pas été initialisée (c-à-d non transmise au template Twig).

Modifier la partie `new Environment($loader)` et charger l'extension de debug `Twig\\Extension\\DebugExtension` juste après :

```php
use Twig\Extension\DebugExtension;

// ...

// instanciation du moteur de template
$twig = new Environment($loader, [
    // activation du mode debug
    'debug' => true,
    // activation du mode de variables strictes
    'strict_variables' => true,
]);

// chargement de l'extension DebugExtension
$twig->addExtension(new DebugExtension());
```

Maintenant il est possible d'inspecter le contenu d'une variable dans un template Twig.

Exemple :

```twig
{{ dump(foo) }}
```

#### Pour la prod

Il est recommandé de désactiver la configuration de la phase de dev et d'activer :

- le cache

Le cache permet de stocker le rendu PHP des templates Twig dans un dossier et de le recharger lors de la prochaine demande.
C'est une optimisation qui doit être appliquée quand le code est en production.

Créer le dossier `var/cache` à la racine du projet.
Voir l'arborescence d'un projet dans [php-application.md](php-application.md).

Modifier la partie `new Environment($loader)` :

```php
// instanciation du moteur de template
$twig = new Environment($loader, [
    // activation du cache
    'cache' => __DIR__.'/../var/cache',
]);
```

NB Pensez à désactiver ou supprimer le chargement de l'extension de debug `Twig\Extension\DebugExtension`.

## Avec Symfony 3.4 et plus

Twig est préinstallé avec Symfony 3.4 et plus.

Les vues sont stockées dans le dossier `templates/`.

## Syntaxe de templates Twig

### Notions de base

Il n'y a que trois types de blocs de code Twig :

1. `{# #}` : pour les commentaires
2. `{{ }}` : pour afficher une variable
3. `{% %}` : pour effectuer une action

### Les commentaires

Noter un commentaire :

```twig
{# ceci est un commentaire #}
```

Le bloc accepte les commentaires multi-lignes.

### Afficher une variable

Pour afficher une variable, il faut utiliser deux paires d'accolades `{{ }}`.

NB Le filtre `escape('html')` (c-à-d `htmlentities()`) est automatiquement appliqué dès qu'une variable est affichée.

Afficher la variable `foo` :

```twig
{{ foo }}
```

Afficher la valeur associée à la clé alpha-numérique `foo` du tableau `bar` :

```twig
{{ bar.foo }}
```

Afficher l'attribut `foo` de la variable de type objet `bar` :

```twig
{{ bar.foo }}
```

Afficher la valeur renvoyée par la méthode `foo` de la variable de type objet `bar` :

```twig
{{ bar.foo() }}
```

NB La notation est la même pour accéder à une clé d'un tableau ou à un attribut d'un objet, on utilise le point `.`.

### Déclaration et initialisation d'une variable

Il est possible de déclarer et d'initialiser une variable en Twig comme en PHP avec l'opérateur `=`.

Exemple de déclaration et d'affectation d'une chaîne de caractères à la variable `greeting` :

```twig
{% set greeting = 'Hello' %}
```

Exemple de déclaration et d'affectation de la valeur de la variable `app.request.host` à la variable `myHost` :

```twig
{% set myHost = app.request.host %}
```

### Concaténer des chaînes de caractères

Il possible de concaténer des chaînes de caractères en Twig avec l'opérateur `~` comme on le ferait en PHP avec l'opérateur `.`.

Exemple de concaténation des deux chaînes de caractères `'foo'` et `'bar'` :

```twig
{{ 'foo' ~ 'bar' }}
```

Exemple de concaténation d'une chaîne de caractères et d'une variable :

```twig
{{ 'http://' ~ app.request.host }}
```

Exemple de concaténation d'une chaîne de caractères et de la variable `app.request.host`, et de l'utilisation d'un filtre d'échappement :

```twig
{{ ('http://' ~ app.request.host)|e }}
```

### Interpolation de chaînes de caractères

Il est possible de faire de l'interpolation de chaînes de caractèresen Twig.
Ceci requiert d'utiliser des doubles quotes `"`.

Exemple d'interpolation de la variable `app.request.host` dans une chaîne de caractères :

```twig
{{ "http://#{app.request.host}" }}
```

Exemple d'interpolation de la variable `app.request.host` dans une chaîne de caractères et de l'utilisation d'un filtre d'échappement :

```twig
{{ "http://#{app.request.host}"|e }}
```

### Les structures conditionnelles (blocs `if`)

Le `if` est identique en PHP et en Twig.
La seule différence est qu'il n'y a pas de parenthèses.

Si la variable  `foo` est égal à `true`, afficher la variable `bar` :

```twig
{% if foo %}
    {{ bar }}
{% endif %}
```

Si l'attribut `bar` de la variable  `foo` est égal à `true`, afficher la variable `baz`  :

```twig
{% if foo.bar %}
    {{ baz }}
{% endif %}
```

Si la clé ou l'attribut `bar` du tableau ou de l'objet `foo` existe, afficher la valeur de `foo.bar` :

```twig
{% if foo.bar is defined %}
    {{ foo.bar }}
{% endif %}
```

NB La syntaxe Twig `foo.bar is defined` est équivalente à la syntaxe PHP `isset($foo['bar'])`.

Hormis les tests habituels (`==`, `>=`, `<=`, etc) il est aussi possible de tester si une variable est définie, vide, nulle, etc.
Tous les tests possibles sont listés ici : [Tests - Documentation - Twig - The flexible, fast, and secure PHP template engine](https://twig.symfony.com/doc/2.x/tests/index.html).

### Les boucles

La syntaxe Twig pour les boucle est différente de PHP.

En PHP, pour parcourir tous les éléments d'un tableau, on utilise `foreach ($items as $item)`.
`$items` désigne le tableau et `$item` un élément différent du tableau à chaque tour.

Avec Twig, pour faire la même chose, on utilise `for item in items`.
`items` désigne le tableau et `$item` un élément différent du tableau à chaque tour.
Sauf que l'ordre est différent : avec Twig on a d'abord l'élément, puis le tableau.
En français, on dirait : « pour chaque item du tableau items ».

Boucler sur le tableau `items` qui contient des chaînes de caractères :

```twig
{% for item in items %}
    {{ item }}<br />
{% endfor %}
```

Boucler sur le tableau `items` qui contient des tableaux avec des clé alpha-numériques :

```twig
{% for item in items %}
    {{ item.id }}<br />
    {{ item.name }}<br />
{% endfor %}
```

Boucler sur le tableau `items` qui contient des objets :

```twig
{% for item in items %}
    {{ item.id }}<br />
    {{ item.name }}<br />
{% endfor %}
```

Boucler sur le tableau `items` qui contient des objets et appeler ses méthodes :

```twig
{% for item in items %}
    {{ item.getId() }}<br />
    {{ item.getName() }}<br />
{% endfor %}
```

Récupérer « un à un » les éléments du résultat d'une requête SQL exécutée avec la méthode `executeQuery()` de `doctrine/dbal` :

```twig
{% for item in items %}
    {{ item.id }}<br />
    {{ item.name }}<br />
{% endfor %}
```

NB La notation est toujours la même : on utilise `for item in items`.

### L'héritage (ou l'extension) de templates

Avec Twig, il n'est pas nécessaire d'utiliser `include()` pour composer un template à partir de plusieurs morceaux.

L'idée est de créer un template initial sur lequel tous les autres templates seront basés.
Ceci permet de ne pas répéter de code et de pouvoir se passer des `include()` de PHP.

Définir un template parent `templates/base.html.twig` que les templates enfants pourront étendre :

```twig
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
```

Créer un template enfant `templates/foo.html.twig` qui hérite du template `templates/base.html.twig` :

```twig
{% extends 'base.html.twig' %}

{% block title %}Foo{% endblock %}

{% block body %}
    <h1>Foo</h1>
    <p>Vous êtes sur la page Foo</p>
{% endblock %}
```

Créer un template enfant `templates/bar.html.twig` qui hérite du template `templates/base.html.twig`, et qui surcharge  les blocs `css_head` et `js_head` :

```twig
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
```

### Les includes

NB Il est rare d'avoir besoin de la fonction `include()` avec Twig.

Comme en PHP, il est possible de faire des includes.
La fonctionnalité `include()` est utile si plusieurs templates enfants utilisent un même bloc de code.
On parle alors de template `partial` (partiel).

Définir le bloc de code commun dans le fichier `templates/partials/_items.html.twig` :

```twig
{% if items %}
<ul>
    {% for item in items %}
    <li>{{ item }}</li>
    {% endfor %}
</ul>
{% endif %}
```

Utiliser le template `partial` dans le template `templates/foo.html.twig` :

```twig
{% include('partials/_items.html.twig') %}
```

### Les filtres

#### Échappement de variables

Twig est sécurisé par défaut contre les injections de code JavaScript.
Quand on affiche la variable `foo` avec `{{ foo }}`, Twig applique automatiquement la fonction PHP `htmlentities()` à la variable `foo`.
Il n'est donc pas nécessaire d'échapper les variables avant affichage.

Mais il est cependant possible d'échapper une variable de façon explicite avec le filtre  `escape()`.
C'est l'équivalent de `htmlentities()`.

Mais `escape()` ne se limite pas à l'échappement de code HTML.
On peut aussi échapper des variables pour :

- du code HTML
- du code JS
- du code CSS
- des URL
- des attributs HTML

Échapper la variable `foo` qui peut être dangereuse dans du HTML :

```twig
<p>{{ foo|escape('html') }}</p>
```

Ou plus court :

```twig
<p>{{ foo|e('html') }}</p>
```

NB Le filtre `escape('html')` (c-à-d `htmlentities()`) est automatiquement appliqué dès qu'une variable est affichée.

Échapper la variable `foo` qui peut être dangereuse dans du code JavaScript :

```twig
var foo = '{{ foo|e('js') }}';
```

Échapper la variable `foo` qui peut être dangereuse du CSS :

```twig
body {
  color: {{ foo|e('css') }};
}
```

Échapper la variable `foo` qui peut être dangereuse dans une URL :

```twig
<a href="{{ foo|e('url') }}>foo</a>
```

Échapper la variable `foo` qui peut être dangereuse dans un attribut HTML :

```twig
<div id="{{ foo|e('html_attr') }}></div>
```

#### Forcer l'affichage d'une variable sans appliquer aucun filtre

Attention, ceci peut être dangereux.
À n'utiliser que si vous savez ce que vous faites.

Afficher la variable `foo` sans appliquer aucun filtre :

```twig
{{ foo|raw }}
```

### Formatage de nombres

#### Sans Symfony

Ajouter le `use` en début de fichier :

```php
use Twig\Extension\CoreExtension;
```

Juste après la partie `new Environment($loader)`, ajouter la config pour afficher deux chiffres après la virgule, la virgule comme séparateur après la virgule et l'espace comme séparateur de milliers :

```php
$twig->getExtension(CoreExtension::class)->setNumberFormat(2, ',', ' ');
```

#### Avec Symfony

Dans le fichier `config/packages/twig.yaml`, ajouter la config pour afficher deux chiffres après la virgule, la virgule comme séparateur après la virgule et l'espace comme séparateur de milliers :

```twig
number_format:
    decimals:             2
    decimal_point:        ','
    thousands_separator:  ' '
```

#### Utilisation

Puis utiliser le filtre `number_format` dans les templates Twig :

```twig
{{ foo|number_format }}
```

### Formatage de dates

Il est possible de manipuler le format d'affichage de dates (objet de type `DateTime`) avec le filtre `date()`.

Afficher la date stockée dans la variable `myDate` au format `JJ/MM/AAAA` :

```twig
{{ myDate|date("d/m/Y") }}
```

Afficher la date stockée dans la variable `myDate` au format `MM/JJ/AAAA` :

```twig
{{ myDate|date("m/d/Y") }}
```

Afficher la date stockée dans la variable `myDate` au format `JJ/MM/AAAA HH:MM:SS` :

```twig
{{ myDate|date("d/m/Y H:i:s") }}
```

### Formatage de durée

Il est possible de manipuler le format d'affichage de durées (objet de type `DateInterval`) avec le filtre `date()`.

Afficher la durée stockée dans la variable `connexion_duration` au format `J` (en nombre de jours):

```twig
{{ connexion_duration|date("%a") }}
```

Afficher la durée stockée dans la variable `connexion_duration` au format `JJ MM AAAA` (en nombre de jours, mois et années) :

```twig
{{ connexion_duration|date("%D %M %Y") }}
```

Afficher la durée stockée dans la variable `connexion_duration` au format ` HHh MMm SSs JJ MM AAAA` (en nombre d'heures, minutes, secondes, jours, mois et années) :

```twig
{{ connexion_duration|date("%Hh %Im %Ss %D %M %Y") }}
```

### Localisation de dates

La localisation permet d'afficher le nom des mois et des jours en français.

Attention : cette fonctionnalité nécessite l'installation et l'activation du module `php-intl`.

#### Utilisation de l'extension

Dans un template Twig, utiliser le filtre `localizeddate()` :

```twig
{{ myDate|localizeddate('full', 'full') }}
```

Le premier paramètre permet de formater la date, le deuxième permet de formater l'heure.

Voici toutes les valeurs possibles à la place de `full` :

- `none`
- `short`
- `medium`
- `long`
- `full`

### Afficher du code Twig sans le faire interpréter

Si vous voulez afficher du code Twig au lieu de le faire interpréter, vous pouvez utiliser la balise `verbatim`.

Afficher du Twig sans le faire interpréter :

```twig
{% verbatim %}
    {% for item in items %}
        {{ item }}
    {% endfor %}
{% endverbatim %}
```

### Génération d'URL

Au lieu d'écrire les URL à la main, vous pouvez utiliser la fonction `path()` qui peut générer les URL de votre application.

Si dans un de vos contrôleurs, vous avez une route nommée `main_index` par exemple, le code suivant va créer un lien qui pointera vers cette page :

```twig
<a href="{{ path('main_index') }}">la page d'accueil</a>
```

Si votre application possède une page de visualisation d'un objet stocké en BDD (un student par exemple), le nom de la route sera probablement quelque chose comme `student_show`.
Le code suivant permet de générer une URL qui pointe vers cette page, en utilisant l'id de l'objet :

```twig
<a href="{{ path('student_show', { id: student.id }) }}">voir le student {{ student.firstname }} {{ student.lastname }}</a>
```

Pour en savoir plus, voir : [Creating and Using Templates (Symfony 3.4 Docs)](https://symfony.com/doc/3.4/templating.html#linking-to-pages).

## Doc

- [Documentation - Twig - The flexible, fast, and secure PHP template engine](https://twig.symfony.com/doc/3.x/)

### Back-end

- [Installation - Documentation - Twig - The flexible, fast, and secure PHP template engine](https://twig.symfony.com/doc/3.x/installation.html)
- [Twig for Developers - Documentation - Twig - The flexible, fast, and secure PHP template engine](https://twig.symfony.com/doc/3.x/api.html)

### Front-end

- [Twig for Template Designers - Documentation - Twig - The flexible, fast, and secure PHP template engine](https://twig.symfony.com/doc/3.x/templates.html)
- [if - Documentation - Twig - The flexible, fast, and secure PHP template engine](https://twig.symfony.com/doc/3.x/tags/if.html)
- [Tests - Documentation - Twig - The flexible, fast, and secure PHP template engine](https://twig.symfony.com/doc/3.x/tests/index.html)
- [for - Documentation - Twig - The flexible, fast, and secure PHP template engine](https://twig.symfony.com/doc/3.x/tags/for.html)
- [Filters - Documentation - Twig - The flexible, fast, and secure PHP template engine](https://twig.symfony.com/doc/3.x/filters/index.html)
- [escape - Documentation - Twig - The flexible, fast, and secure PHP template engine](https://twig.symfony.com/doc/3.x/filters/escape.html)
- [verbatim - Documentation - Twig - The flexible, fast, and secure PHP template engine](https://twig.symfony.com/doc/3.x/tags/verbatim.html)

### Formatage de nombres

- [number_format - Documentation - Twig - The flexible, fast, and secure PHP template engine](https://twig.symfony.com/doc/3.x/filters/number_format.html)
- [PHP: number_format - Manual](http://php.net/manual/en/function.number-format.php)

### Formatage de dates

- [date - Documentation - Twig - The flexible, fast, and secure PHP template engine](https://twig.symfony.com/doc/3.x/filters/date.html)
- [date - Documentation - Twig - The flexible, fast, and secure PHP template engine](https://twig.symfony.com/doc/3.x/functions/date.html)
- [PHP: date - Manual](http://php.net/manual/en/function.date.php)
- [PHP: DateInterval::format - Manual](https://secure.php.net/DateInterval.format)

### Localisation de dates

- [Twig Extensions — Twig-extensions latest documentation](http://twig-extensions.readthedocs.io/en/latest/index.html)
- [The Intl Extension — Twig-extensions latest documentation](https://twig-extensions.readthedocs.io/en/latest/intl.html)
- [php - Localize dates in twigs using Symfony 2 - Stack Overflow](https://stackoverflow.com/questions/9480325/localize-dates-in-twigs-using-symfony-2)
- [php - How to render a DateTime object in a Twig template - Stack Overflow](https://stackoverflow.com/questions/8318914/how-to-render-a-datetime-object-in-a-twig-template/23424315#23424315)

### Debug

- [dump - Documentation - Twig - The flexible, fast, and secure PHP template engine](https://twig.symfony.com/doc/2.x/functions/dump.html)

