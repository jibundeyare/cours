# Laravel 8

Un projet « témoin » accompagne ce cours : [https://github.com/jibundeyare/src-laravel-8](https://github.com/jibundeyare/src-laravel-8).

## Prérequis

- PHP
- MariaDB ou MySQL
- la commande `composer`

## Installation de `artisan`

La commande `artisan` est outil qui fait partie de l'écosystème Laravel.
Il permet notamment de créer un nouveau projet ou de démarrer un serveur web.

Vous pouvez l'installer avec `composer` :

    composer global require laravel/installer

Vous connaître la liste des commandes de `artisan`, (depuis un dossier projet) vous pouvez taper :

    php artisan list

Et pour avoir des infos sur une commande, (depuis un dossier projet) vous pouvez taper :

    php artisan une-commande --help

Par exemple, pour avoir des infos sur la commande `ui:auth`, vous pouvez taper :

    php artisan ui:auth --help

## Quelques concepts généraux

Un peu de théorie ne fait pas de mal.
Prenez le temps de lire au moins les deux pages suivantes, (conseillées dans la section « Next Steps » de l'installation [Installation - Laravel - The PHP Framework For Web Artisans](https://laravel.com/docs/8.x/installation#next-steps)) :

- [Request Lifecycle](https://laravel.com/docs/8.x/lifecycle)
- [Directory Structure](https://laravel.com/docs/8.x/structure)

## Création d'une application avec le framework Laravel

[Installation - Laravel - The PHP Framework For Web Artisans](https://laravel.com/docs/8.x#installation-via-composer)

Vous pouvez installer créer un nouveau projet Laravel avec `composer` ou avec `artisan`.

Avec `composer` :

    composer create-project laravel/laravel foo

Avec `artisan` :

    laravel new foo

Maintenant testez l'instalation :

    cd foo
    php artisan serve

Ouvrez le lien suivant et savourez : [http://127.0.0.1:8000/](http://127.0.0.1:8000/)

*Important : à partir d'ici, tous les chemins indiqués seront des chemins relatifs au dossier d'installation du framework.*

## Configuration

### La langue (locale)

[Localization - Laravel - The PHP Framework For Web Artisans](https://laravel.com/docs/8.x/localization#configuring-the-locale)

Dans le fichier `config/app.php`, changez la ligne :

    'timezone' => 'UTC',

par :

    'timezone' => 'Europe/Paris',

Puis la ligne :

    'locale' => 'en',

par :

    'locale' => 'fr',

Puis la ligne :

    'faker_locale' => 'en_US',

par :

    'faker_locale' => 'fr_FR',

### L'accès à la base de données (BDD)

- [Configuration - Laravel - The PHP Framework For Web Artisans](https://laravel.com/docs/8.x/configuration)
- [Database: Getting Started - Laravel - The PHP Framework For Web Artisans](https://laravel.com/docs/8.x/database)

La configuration de l'accès à la BDD se fait dans le fichier `.env`.

Imaginons que vous ayez les codes d'accès suivants à une BDD MariaDB :

- host : 127.0.0.1
- port : 3306
- database : laravel_8
- username : foo
- password : 123

Vous pouvez adaptez les lignes suivantes à vos besoins :

    DB_CONNECTION=mysql
    DB_HOST=127.0.0.1
    DB_PORT=3306
    DB_DATABASE=laravel
    DB_USERNAME=root
    DB_PASSWORD=

en :

    DB_CONNECTION=mysql
    DB_HOST=127.0.0.1
    DB_PORT=3306
    DB_DATABASE=laravel_8
    DB_USERNAME=foo
    DB_PASSWORD=123

Rappel : MariaDB est le successeur de MySQL. Actuellement les deux BDD sont compatibles.

## Les routes nommées

[Routing - Laravel - The PHP Framework For Web Artisans](https://laravel.com/docs/8.x/routing#basic-routing)

Les routes sont les URL qui seront mises à disposition par l'application.

Par exemple :

- la page d'accueil sera la route `/`
- la page « à propos » sera la route `/about`
- la page de contact sera la route `/contact`
- ...

Dans l'application, au lieu de désigner les routes par leur URL, on pourra leur donner un nom.
D'où le concept de « route nommée ».

À tout moment, vous pouver utiliser la commande suivante pour afficher la liste des routes :

    php artisan route:list

Une URL peut être constituée d'une partie fixe et d'une partie variable.
La partie variable peut être récupérée par l'application et réutilisée (pour requêter la BDD par exemple).

Par exemple :

- l'URL `/` est consitutée uniquement d'une partie fixe
- l'URL `/contact` est consitutée uniquement d'une partie fixe
- l'URL `/product/{id}` est consitutée uniquement d'une partie fixe `/product/` et d'une partie variable `{id}`  
  cela veut dire que les URL `/product/123`, `/product/42` ou `/product/abc` seront interprêtées comme si c'était l'URL `/product/{id}`

La création de routes se fait dans le fichier `routes/web.php`.

Vous pouvez supprimer la route par défaut :

    Route::get('/', function () {
        return view('welcome');
    });

Et ajouter une route qui sera associée à une action :

    use App\Http\Controllers\MainController;

    // ...

    // ajouter la route '/' associée avec l'action MainController::index()
    // MainController est une classe et index est une méthode de cette classe
    // cette route est nommée 'main.index'
    Route::get('/', [MainController::class, 'index'])->name('main.index');

Ou encore ajouter des routes par défaut associées à une ressource de type 'foo' :

    use App\Http\Controllers\FooController;

    // ...

    // ajouter des routes par défaut associées avec le contrôleur FooController
    Route::resource('foo', FooController::class);

Voici un tableau des routes par défaut qui sont associées à une ressource de type 'foo' :

| Verb      | URI             | Action  | Route Name  |
|-----------|-----------------|---------|-------------|
| GET       | /foo            | index   | foo.index   |
| GET       | /foo/create     | create  | foo.create  |
| POST      | /foo            | store   | foo.store   |
| GET       | /foo/{foo}      | show    | foo.show    |
| GET       | /foo/{foo}/edit | edit    | foo.edit    |
| PUT/PATCH | /foo/{foo}      | update  | foo.update  |
| DELETE    | /foo/{foo}      | destroy | foo.destroy |

Remarque : dans ce contexte, une ressource est un ensemble de données que l'on veut pouvoir stocker en BDD.
Par exemple :

- pour une boutique en ligne : un produit du catalogue avec son nom, sa marque, sa couleur, son prix, etc
- pour un blog : un article avec son titre, son texte, son auteur, sa date de publication, etc
- pour un site de location de voiture : un client avec son nom et prénom, son numéro de téléphone, son email, etc

### Les redirections

Il est possible de configurer des redirection directement dans le fichier `routes/web.php` :

    // redirection de la page « à propos » vers la page « contact »
    Route::redirect('/about', '/contact');

## Les contrôleurs

[Controllers - Laravel - The PHP Framework For Web Artisans](https://laravel.com/docs/6.x/controllers#single-action-controllers)

Les contrôleurs sont le cerveau de votre application.
Ce sont ces composants qui vont déterminer « ce que l'application fait ».

## Un contrôleur simple

Nous allons créer un premier contrôleur qui affichera la page d'accueil :

    php artisan make:controller MainController

Vous pouvez ouvrir le fichier `app/Http/Controllers/MainController.php` et ajouter le code suivant à la place du placeholder `//` :

        public function index()
        {
            return view('welcome');
        }

## Un contrôleur de ressource

Nous allons créer un contrôleur pour une ressource de type 'foo' :

    php artisan make:controller -r FooController

- créer des actions
[Controllers - Laravel - The PHP Framework For Web Artisans](https://laravel.com/docs/8.x/controllers#single-action-controllers)

## Les vues

- créer une vue parent
[Views - Laravel - The PHP Framework For Web Artisans](https://laravel.com/docs/8.x/views#creating-and-rendering-views)
[Blade Templates - Laravel - The PHP Framework For Web Artisans](https://laravel.com/docs/8.x/blade)
- créer des vues enfants
[Views - Laravel - The PHP Framework For Web Artisans](https://laravel.com/docs/8.x/views#creating-and-rendering-views)
[Blade Templates - Laravel - The PHP Framework For Web Artisans](https://laravel.com/docs/8.x/blade)

- générer des urls de routes nommées
[Routing - Laravel - The PHP Framework For Web Artisans](https://laravel.com/docs/8.x/routing#named-routes)
- afficher des urls de routes nommées dans un template blade

```
{{ route('foo') }}
{{ route('foo', ['id' => 123]) }}
{{ route('foo', ['id' => 123, 'bar' => 42]) }}
```

[Compiling Assets (Mix) - Laravel - The PHP Framework For Web Artisans](https://laravel.com/docs/8.x/mix)

[Compiling Assets (Mix) - Laravel - The PHP Framework For Web Artisans](https://laravel.com/docs/8.x/mix#installing-laravel-mix)
- installer les packages js et compiler les fichiers CSS et JS avec :

```
npm install
npm run dev
```

- activer browsersync
[Compiling Assets (Mix) - Laravel - The PHP Framework For Web Artisans](https://laravel.com/docs/8.x/mix#browsersync-reloading)

Dans `webpack.mix.js` :

```
mix.browserSync('127.0.0.1:8000');

// ou si 127.0.0.1 ne fonctionne pas
mix.browserSync('localhost:8000');
```

- activer le watch
[Compiling Assets (Mix) - Laravel - The PHP Framework For Web Artisans](https://laravel.com/docs/8.x/mix#watching-assets-for-changes)

```
npm run watch
```

- intéger des fichiers CSS et JS dans des vues
[Blade Templates - Laravel - The PHP Framework For Web Artisans](https://laravel.com/docs/8.x/blade#stacks)

