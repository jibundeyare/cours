# Laravel 8

Un projet « témoin » accompagne ce cours : [https://github.com/jibundeyare/src-laravel-8](https://github.com/jibundeyare/src-laravel-8).

## Prérequis

- PHP
- MariaDB ou MySQL
- la commande `composer`
- la commande `npm`

## Quelques concepts généraux

Un peu de théorie ne fait pas de mal.
Prenez le temps de lire au moins les deux pages suivantes, (conseillées dans la section « Next Steps » de l'installation [Installation - Laravel - The PHP Framework For Web Artisans](https://laravel.com/docs/8.x/installation#next-steps)) :

- [Request Lifecycle](https://laravel.com/docs/8.x/lifecycle)
- [Directory Structure](https://laravel.com/docs/8.x/structure)

## La commande `artisan`

La commande `artisan` est outil qui fait partie de l'écosystème Laravel.
Elle permet notamment de créer un nouveau projet ou de démarrer un serveur web.

Pour connaître la liste des commandes de `artisan`, (depuis un dossier projet) vous pouvez taper :

    php artisan list

Et pour avoir des infos sur une commande, (depuis un dossier projet) vous pouvez taper :

    php artisan une-commande --help

Par exemple, pour avoir des infos sur la commande `make:controller`, vous pouvez taper :

    php artisan make:controller --help

### **(Optionnel)** Installation globale de la commande `artisan`

Une installation globale veut dire que la commande est disponible depuis n'importe quel dossier en tapant seulement `artisan` (pas la peine de taper `php artisan`).

Vous pouvez 'installer globalement la commande avec `composer` :

    composer global require laravel/installer

Fermez votre terminal puis lancez-en un autre.
Testez en tapant la commande `artisan`.

Si vous obtenez une erreur du type `command not found`, vérifiez que vous avez bien réalisé toutes les étapes de la procédure d'installation de Composer dans [composer.md](composer.md).

## Création d'une application avec le framework Laravel

[Installation - Laravel - The PHP Framework For Web Artisans](https://laravel.com/docs/8.x#installation-via-composer)

Vous pouvez installer créer un nouveau projet Laravel avec `composer` ou avec `artisan`.

Imaginons que nous voulons créer un projet qui s'appelle `foo`.
Les commandes suivantes vont créer un sous-dossier nommé `foo` dans le dossier courant, qui contiendra tous les fichiers du framework Laravel.

Avec `composer` :

    composer create-project laravel/laravel foo

Ou avec `laravel` :

    laravel new foo

Installez les dépendances du front-end :

    cd foo
    npm install

Lancez le serveur web de développement :

    php artisan serve

Maintenant pour tester l'instalation ouvrez le lien suivant et savourez : [http://127.0.0.1:8000/](http://127.0.0.1:8000/)

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

## Le déboggage

Laravel fournit deux fonctions utiles pour le déboggage :

- la fonction `dump()` qui affiche le contenu d'une variable avec jolie coloration syntaxique
- la fonction `dd()` (pour « dump and die ») qui fait comme `dump()` mais stop l'exécution juste après

Exemple :

```php
foreach ($users as $user) {
    dump($user);
}
exit()
```

Ou encore :

```php
dd($foo);
```

Il est aussi possible de faire du logging en utilisant la classe `Illuminate\Support\Facades\Log`.
Par défaut, le logging va enregistrer les informations dans le fichier `storage/logs/laravel.log`.
Ceci est idéal pour débogger une application en production car on veut évuter que les utilisateurs voient les messages.

Voici la liste des méthodes disponibles pour enregistrer des données dans le fichier `storage/logs/laravel.log` :

```php
use Illuminate\Support\Facades\Log;

// ...

Log::emergency($message);
Log::alert($message);
Log::critical($message);
Log::error($message);
Log::warning($message);
Log::notice($message);
Log::info($message);
Log::debug($message);
```

Exemple d'utilisation :

```php
use Illuminate\Support\Facades\Log;

// ...

Log::debug($foo);
```

Ceci va enregistrer le contenu de la variable `$foo` dans le fichier `storage/logs/laravel.log`.

Exemple de données du fichier `storage/logs/laravel.log` :

```
[2022-03-04 11:22:19] local.DEBUG: {"id":1,"nom":"foo","description":"","created_at":null,"updated_at":null}
```

## Les routes nommées

[Routing - Laravel - The PHP Framework For Web Artisans](https://laravel.com/docs/8.x/routing#basic-routing)

Les routes sont des URL associées à des fonctions.
Elles définissent les URLs mises à disposition par l'application et leurs fonctionnalités.

Par exemple :

- l'URL `/` affichera la page d'accueil
- l'URL `/about` affichera la page « à propos »
- l'URL `/product/{id}` affichera la page d'un produit (choisi grâce au paramètre `{id}` dans la barre d'adresse avec `http://example.com/product/123` par exemple)
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

### Création d'une route pour la page d'accueil

La création de routes se fait dans le fichier `routes/web.php`.

Vous pouvez supprimer la route par défaut :

    Route::get('/', function () {
        return view('welcome');
    });

Et ajouter une route qui sera associée à la fonction `index()` de la classe `MainController` :

    use App\Http\Controllers\MainController;

    // ...

    // ajouter la route '/' associée avec l'action MainController::index()
    // MainController est une classe et index est une méthode de cette classe
    // cette route est nommée 'main.index'
    Route::get('/', [MainController::class, 'index'])->name('main.index');

### Création d'une route pour une autre page

Vous pouvez ajouter d'autres routes qui seront associées à d'autres fonctions de la classe `MainController`:

    use App\Http\Controllers\MainController;

    // ...

    // ajouter la route '/about' associée avec l'action MainController::about()
    // MainController est une classe et about est une méthode de cette classe
    // cette route est nommée 'main.about'
    Route::get('/about', [MainController::class, 'about'])->name('main.about');

### Création de routes accessible avec d'autres méthodes HTTP

    Route::get('/about', [MainController::class, 'about'])->name('main.about');

Dans le code ci-dessus, le mot clé `get` veut dire que cette route n'est accessible qu'avec la méthode HTTP `GET`.

Mais il existe d'autres méthodes si l'on veut que la route soir accessible avec d'autres méthodes HTTP :

    Route::get('/foo', [MainController::class, 'foo'])->name('main.foo');
    Route::post('/foo', [MainController::class, 'foo'])->name('main.foo');
    Route::put('/foo', [MainController::class, 'foo'])->name('main.foo');
    Route::patch('/foo', [MainController::class, 'foo'])->name('main.foo');
    Route::delete('/foo', [MainController::class, 'foo'])->name('main.foo');
    Route::options('/foo', [MainController::class, 'foo'])->name('main.foo');

Si nous voulons qu'une route soit accessible avec plusieurs méthodes HTTP, nous pouvons utiliser la méthode `match()` :

    Route::match(['get', 'post'], '/foo', [MainController::class, 'foo'])->name('main.foo')

Si c'est absolument nécessaire, il est aussi possible de rendre une route accessible avec n'importe quelle méthode HTTP :

    Route::any('/foo', [MainController::class, 'foo'])->name('main.foo')

### Création d'une route pour une ressource

Dans la jargon de Laravel, une ressource est un objet qui peut être manipulé via un CRUD et stocké en BDD.

Par exemple :

- pour une boutique en ligne : un produit du catalogue avec son nom, sa marque, sa couleur, son prix, etc
- pour un blog : un article avec son titre, son texte, son auteur, sa date de publication, etc
- pour un site de location de voiture : un client avec son nom et prénom, son numéro de téléphone, son email, etc

Le résultat est que Laravel va automatiquement créer des routes pour pouvoir créer, modifier, supprimer et afficher un objet.

Voici comment ajouter des routes par défaut associées à une ressource de type 'foo' :

    use App\Http\Controllers\FooController;

    // ...

    // ajouter des routes par défaut associées avec le contrôleur FooController
    Route::resource('foo', FooController::class);

Et voici un tableau des routes par défaut qui seront associées à la ressource de type 'foo' :

| Verb      | URI             | Action  | Route Name  |
|-----------|-----------------|---------|-------------|
| GET       | /foo            | index   | foo.index   |
| GET       | /foo/create     | create  | foo.create  |
| POST      | /foo            | store   | foo.store   |
| GET       | /foo/{foo}      | show    | foo.show    |
| GET       | /foo/{foo}/edit | edit    | foo.edit    |
| PUT/PATCH | /foo/{foo}      | update  | foo.update  |
| DELETE    | /foo/{foo}      | destroy | foo.destroy |

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

## Choisir la vue qui sera utilisée par le contrôleur

Il est possible de choisir n'importe quelle vue, pourvu qu'elle soit présente dans le dossier `resources/views` ou un de ses sous-dossiers.

Vous pouvez ouvrir le fichier `app/Http/Controllers/MainController.php` et remplacer la vue par défaut par celle de votre choix :

        public function index()
        {
            // affichage de la vue resources/views/main/index.blade.php
            return view('main.index');
        }

## Transmission de variables à une vue

Pour transmettre des variables à une vue, vous devez rajouter un tableau en paramètre dans l'appel de la fonction `view()` :

        // création d'une variable en PHP
        $message = 'Hello Laravel!';

        // affichage de la vue resources/views/main/index.blade.php
        // envoi de la variable $message dans la vue
        return view('main.index', [
            'message' => $message,
        ]);

Remarque : la variable PHP se nomme `$message`. Mais comme nous avons choisi la clé `message` dans le tableau, dans la vue aussi la variable s'appellera `$message`.

Voir la page suivante pour en savoir plus sur la façon de transmettre des variables à une vue : [Views - Laravel - The PHP Framework For Web Artisans](https://laravel.com/docs/8.x/views#creating-and-rendering-views)

## Un contrôleur de ressource

Nous allons créer un contrôleur pour une ressource de type 'foo' :

    php artisan make:controller -r FooController

- créer des actions
[Controllers - Laravel - The PHP Framework For Web Artisans](https://laravel.com/docs/8.x/controllers#single-action-controllers)

## Les vues

### Affichage d'une variable

Pour afficher une variable, vous pouvez utiliser des accolades `{}`.

    {{ $foo }}

Remarque : bien sûr ceci ne fonctionne que si la variable `foo` a été transmise à la vue lors de l'appel de la fonction `view()` dans le contrôleur.

### Générer les urls de routes nommées

[Routing - Laravel - The PHP Framework For Web Artisans](https://laravel.com/docs/8.x/routing#named-routes)

Pour afficher l'url des routes nommées dans une vue, vous devez utiliser la fonction `route()` :

    {{ route('foo') }}
    {{ route('foo', ['id' => 123]) }}
    {{ route('foo', ['id' => 123, 'bar' => 42]) }}

Ces blocs vont générer les urls correspondant à la route nommée.
Le tableau avec les couples clé valeur représente des paramètres qui seront rattachés à l'url.

Vous pouvez l'utiliser pour créer un lien clickable par exemple :

    <a href="{{ route('foo') }}">Aller vers la page foo</a>

### Création d'une vue simple

Vous pouvez créer le dossier `resources/views/main` puis le fichier `resources/views/main/index.blade.php` :

    <!DOCTYPE html>
    <html lang="en">
    <head>
        <meta charset="UTF-8">
        <meta http-equiv="X-UA-Compatible" content="IE=edge">
        <meta name="viewport" content="width=device-width, initial-scale=1.0">
        <title>Accueil</title>
    </head>
    <body>
        <h1>Accueil</h1>
        <p>
            Vous êtes sur la page d'accueil.
        </p>
    </body>
    </html>

Testez maintenant votre vue dans votre navigateur : [http://127.0.0.1:8000/](http://127.0.0.1:8000/)

### Intégration de fichiers CSS et JS dans une vue Blade

Les fichiers CSS et JS se trouvent dans les dossiers suivants :

- `resources/css`
- `resources/js`

Pour que ces fichiers deviennent accessibles via le serveur web, vous devez les compiler et les stocker dans le dossier `public` avec la commande suivante :

    npm run dev

Vous devrez relancer cette commande chaque fois que vous modifierez vos fichiers CSS ou JS.

Dernière chose, vous devez les intégrer dans vos vues en ajoutant la ligne suivante pour le CSS dans votre vue Blade :

    <link rel="stylesheet" href="{{ asset('css/app.css') }} ">

Et voici la ligne pour le JS :

    <script src="{{ asset('js/app.js') }}" defer></script>

La fonction `asset()` permet de retrouver automatiquement le dossier dans lequel sont stockés les ressources statiques compilées (c-à-d le CSS et le JS).

Voici à quoi devrait ressemble votre balide `head` après modification :

    <head>
        <meta charset="UTF-8">
        <meta http-equiv="X-UA-Compatible" content="IE=edge">
        <meta name="viewport" content="width=device-width, initial-scale=1.0">
        <title>Accueil</title>
        <link rel="stylesheet" href="{{ asset('css/app.css') }} ">
        <script src="{{ asset('js/app.js') }}" defer></script>
    </head>

Pour en savoir plus sur l'intégration de code CSS et de code JS, voir la page suivante : [Compiling Assets (Mix) - Laravel - The PHP Framework For Web Artisans](https://laravel.com/docs/8.x/mix)

### Utilisation de fichier CSS et JS spécifique pour certaines pages

Parfois, il est nécessaire de pouvoir utiliser un fichier CSS ou JS seulement sur une page en particulier.

Pour pouvoir faire ça, vous devrez d'abord créer votre fichier CSS ou votre fichier JS.

Puis vous devrez préciser dans le fichier `webpack.mix.js` quels sont les fichiers à compiler.

Le code ci-dessous montre comment compiler les fichier `resources/css/foo.css` et `resources/js/foo.js` en plus des fichiers par défaut :

    mix.js('resources/js/app.js', 'public/js')
        .postCss('resources/css/app.css', 'public/css', [
            //
        ])
        .js('resources/js/foo.js', 'public/js')
        .postCss('resources/css/foo.css', 'public/css', [
            //
        ]);

Il ne vous reste plus qu'à insérer les balises `link` et `script` dans la vue de votre page :

    <link rel="stylesheet" href="{{ asset('css/foo.css') }}">
    <script src="{{ asset('js/foo.js') }}" defer></script>

### Complilation automatique des fichiers CSS et JS

Si vous en avez assez recompilation vos fichier à chaque fois, vous pouver lancer la recomplilation automatique :

    npm run watch

Et si vous trouvez qu'il y a trop de notifications, vous pouvez les désactiver en ajoutant le code suivant à la fin du fichier `webpack.mix.js` (présent à la racine du projet) :

    mix.disableNotifications();

### Intégration d'images dans une vue Blade

Pour intégrer une image dans votre vue, vous devez d'abord créer un dossier `public/img` puis la copier dedans.

Ensuite vous pouvez utiliser la fonction `asset()` comme ci-dessous pour composer l'url de l'image :

    <img src="{{ asset('img/foo.jpg') }}" alt="Description de l'image">

Le code `asset('img/foo.jpg')` va générer l'url `http://127.0.0.1:8000/img/foo.jpg` tant que vous travaillez sur votre poste.
Quand vous mettrez votre code en production sur le serveur `example.com`, l'url de l'image deviendra `http://example.com/img/foo.jpg`.

### Création d'une vue parent

La méthode préférée pour créer vos vues blades est de créer une vue parent et des vues enfants grâce à la fonctionnalité de l'héritage.

Voir la page suivante pour en savoir plus sur la façon de créer une vue parent et des vues enfant : [Blade Templates - Laravel - The PHP Framework For Web Artisans](https://laravel.com/docs/8.x/blade#layouts-using-template-inheritance)

Vous pouvez créer le fichier `resources/views/base.blade.php` :

    <!DOCTYPE html>
    <html lang="en">
    <head>
        <meta charset="UTF-8">
        <meta http-equiv="X-UA-Compatible" content="IE=edge">
        <meta name="viewport" content="width=device-width, initial-scale=1.0">
        <title>Mon site web - @yield('title')</title>

        @section('css')
        <link rel="stylesheet" href="{{ asset('css/app.css') }} ">
        @show

        @section('js')
        <script src="{{ asset('js/app.js') }}" defer></script>
        @show
    </head>
    <body>
        <header>
            @section('banner')
                <img src="{{ asset('img/banner.jpg') }}" alt="Description de la bannière par défaut">
            @show
        </header>

        <div class="container">
            @yield('content')
        </div>

        <footer>
            @section('footer')
                Copyright 2022
            @show
        </footer>
    </body>
    </html>

Les instructions `@yield()` sont des instructions que nous pourrons remplacer par le contenu de notre choix dans des vues enfant.

Les blocs `@section() @show` sont des blocs qui contiennent une valeur par défaut.
Si les vues enfants ne remplacent pas ces contenus, ce sont les valeurs par défaut qui seront affichées.

### Création d'une vue enfant

Vous pouvez remplacer le contenu du fichier `resources/views/main/index.blade.php` avec :

    @extends('base')

    @section('title', 'Accueil')

    @section('css')
        @parent
        <link rel="stylesheet" href="{{ asset('css/homepage.css') }} ">
    @endsection

    @section('content')
        <h1>Accueil</h1>
        <p>
            Vous êtes sur la page d'accueil
        </p>
    @endsection

L'instruction `@extends()` précise quelle vue sera utilisée comme vue parent. Ici c'est la vue `resources/views/base.blade.php` qui est ciblée.

Les blocs `@section()` permettent de remplacer le contenu d'un bloc `@section()` ou d'une instruction `@yield()` qui se trouve dans la vue parent.

Si le bloc remplace un bloc `@section()` dans la vue parent, il est possible d'utiliser le mot clé `@parent` à l'intérieur du bloc pour afficher la valeur par défaut spécifiée dans la vue parent.

Si le bloc remplace une instruction `@yield()` dans la vue parent, il n'y a pas de valeur par défaut.

### Activer browsersync (le rechargement automatique de la page)

[Compiling Assets (Mix) - Laravel - The PHP Framework For Web Artisans](https://laravel.com/docs/8.x/mix#browsersync-reloading)

Dans `webpack.mix.js` :

    mix.browserSync('127.0.0.1:8000');

    // ou si 127.0.0.1 ne fonctionne pas
    mix.browserSync('localhost:8000');

intéger des fichiers CSS et JS dans des vues

[Blade Templates - Laravel - The PHP Framework For Web Artisans](https://laravel.com/docs/8.x/blade#stacks)

## Le schéma de base de données (BDD)

Vous pouvez créer le schéma de BDD « à la main » en écrivant du code SQL ou en utilisant PhpMyAdmin.
Ou alors vous pouvez utiliser le système de « migration » de Laravel.

Le système de migration consiste à indiquer comment créer le schéma de BDD avec du code PHP.

Pour cette partie, la page suivante vous donnera toutes les infos utiles : [Database: Migrations - Laravel - The PHP Framework For Web Artisans](https://laravel.com/docs/8.x/migrations).

### Exécution des fichiers de migrations

La commande suivante exécute les fichiers de migrations :

    php artisan migrate

Mais en phase de développement, il est souvent plus pratique de détruire toutes les tables puis d'exécuter tous les fichiers de migration depuis le début.
Pour faire ça, vous pouvez utiliser la commande suivante :

    php artisan db:wipe && php artisan migrate

Pour savoir quel fichier de migration a été exécuté, utilisez la commande suivante :

    php artisan migrate:status

Alternativement, vous pouvez aussi examiner la table `migrations` dans la BDD.

### Revenir à une version précédente de la BDD avec les fichiers de migration

Il est parfois nécessaire de pouvoir revenir à une version précédente de la BDD si vous rencontrez un bug compliqué en production et que vous voulez rapidement rendre l'application de nouveau fonctionnelle.

La commande suivante vous permet d'annuler le dernier fichier de migration exécuté :

    php artisan migrate:rollback --step 1

Si vous devez annuler plusieurs fichiers de migrations, spécifiez-en le nombre après l'option `step`.

Exemple qui annule les `3` derniers fichiers de migration :

    php artisan migrate:rollback --step 3

### Le fichier de migration

La commande suivante permet de générer un fichier de migration pour la table `foo` :

    php artisan make:migration create_foo_table

Un nouveau fichier apparaît : `database/migrations/2021_03_18_171447_create_foo_table.php`

La fonction `up()` sert à mettre à jour la BDD.
C'est là qu'il faut indiquer comment créer le schéma de BDD.

        /**
         * Run the migrations.
         *
         * @return void
         */
        public function up()
        {
            Schema::create('foo', function (Blueprint $table) {
                $table->id();
                $table->timestamps();
            });
        }

La fonction `id()` créé une colonne de type `UNSIGNED BIG INTEGER` avec une contrainte d'unicité et un auto-incrément.
C-à-d une colonne de type « primary key ».

La fonction `timestamps()` créé deux colonne (`created_at` et `updated_at`) dans lesquelle peuvent être stockées la date de création et de modification de l'objet.

Le fonctio `down()` sert à remettre la BDD dans un état antérieur.
Cette fonction permet de « downgrader » (par opposition avec « upgrader ») votre BDD, ce qui est utile si la nouvelle version de votre application contient des bugs.

        /**
         * Reverse the migrations.
         *
         * @return void
         */
        public function down()
        {
            Schema::dropIfExists('foo');
        }

### Création d'une table

La section suivante liste tous les types de données que vous pouvez utiliser pour créer une colonne : [Database: Migrations - Available Column Types - Laravel - The PHP Framework For Web Artisans](https://laravel.com/docs/8.x/migrations#available-column-types).
Par exemple, les fonctions `id()` et `timestamps()` en font partie.

La section suivante liste tous les modificateurs de colonne : [Database: Migrations - Column Modifiers - Laravel - The PHP Framework For Web Artisans](https://laravel.com/docs/8.x/migrations#column-modifiers).
Par exemple, ces modificateurs permettent rendre une colonne nullable ou de définir une valeur par défaut.

La section suivante liste tous les indexes que l'on peut rajouter sur à une colonne : [Database: Migrations - Creating Indexes - Laravel - The PHP Framework For Web Artisans](https://laravel.com/docs/8.x/migrations#creating-indexes)
Les indexes permettent d'accélerer la recherche (`index()`) ou d'appliquer des contraintes (`unique()`).

Nous allons ajouter les colonnes suivantes à la table `foo` :

- `email` : varchar 100, unique
- `nom` : varchar, 100
- `description` : text, nullable
- `rang` : integer, nullable
- `actif` : boolean, default TRUE

Voici le code :

            Schema::create('foo', function (Blueprint $table) {
                $table->id();
                $table->string('email', 100)->unique();
                $table->string('nom', 100);
                $table->text('description')->nullable();
                $table->integer('rang')->nullable();
                $table->boolean('actif')->default(true);
                $table->timestamps();
            });

### Création d'une table avec une colonne « clé étrangère »

La section suivante indique comment manipuler les contraintes de clé étrangères (aussi appelées contraintes d'intégrités) : [Database: Migrations - Laravel - The PHP Framework For Web Artisans](https://laravel.com/docs/8.x/migrations#foreign-key-constraints).
Les contraintes de clé étrangère permettent de s'assurer qu'un objet ne sera supprimé s'il est référencé quelque part ou qu'un identifiant ne sera pas utilisé s'il ne correspond à aucun objet existant.

Si la table `bar` doit faire référence à la clé primaire de la table `foo`, il faut créer une colonne `foo_id` dans la table `bar` et ajouter une contrainte de clé étrangère dessus.
Voici le code :

            Schema::create('bar', function (Blueprint $table) {
                $table->id();
                $table->foreignId('foo_id')->references('id')->on('foo');
                $table->timestamps();
            });

*Attention : la colonne à laquelle il est fait référence doit être une clé primaire, sinon la contrainte de clé étrangère ne fonctionnera pas.*

### Modification d'une table

À un moment où un autre, il se peut que vous ayez besoin de modifier la structure de votre table.

Cette fois-ci, vous ne devez pas créer de nouvelle table, vous devez modifier une table existante.

Pour modifier une colonne, de façon générale il faut ajouter l'appel à cette fonction `change()` comme on va le voir.

Il n'est pas possible de modifier un index mais vous pouvez le renommer avec `$table->renameIndex('from', 'to')`.
Si vous voulez modifier un index, il faudra le supprimer et le recréer.

Pour supprimer une colonne ou un index, vous trouverez toutes les fonctions ci-dessous :

- [Dropping Columns](https://laravel.com/docs/8.x/migrations#dropping-columns)
- [Dropping Indexes](https://laravel.com/docs/8.x/migrations#dropping-indexes)
- [Dropping Foreign Keys](https://laravel.com/docs/8.x/migrations#dropping-foreign-keys)

Partons du code de création d'une table nommée `baz` :

            Schema::create('baz', function (Blueprint $table) {
                $table->id();
                $table->string('nom');
                $table->integer('total');
                $table->boolean('actif');
                $table->timestamps();
            });

#### Suppression d'une table

Suppression de la table `foo` :

            Schema::dropIfExists('foo');

ATTENTION : s'il y a des contraintes de clé étrangère, cette action peut échouer.

#### Ajout ou modification de colonnes

Le code ci-dessous permet de :

- changer la taille de la colonne `nom`
- rendre nullable la colonne `total`
- définir une valeur par défaut pour la colonne `actif`
- ajouter une colonne et une contrainte de clé étrangère

Voici le code de modification :

            Schema::table('baz', function (Blueprint $table) {
                // modification de colonnes
                $table->string('nom', 190)->change();
                $table->integer('total')->nullable()->change();
                $table->boolean('actif')->default(true)->change();

                // ajout d'une colonne et d'une contrainte de clé étrangère
                $table->foreignId('foo_id')
            });

Et voici le code qui fait l'opération inverse :

            Schema::table('baz', function (Blueprint $table) {
                // suppression d'une colonne et d'une contrainte de clé étrangère
                $table->dropForeign(['foo_id']);
                $table->dropIndex(['foo_id']);
                $table->dropColumn('foo_id');

                // modification de colonnes
                $table->string('nom')->change();
                $table->integer('total')->change();
                $table->boolean('actif')->change();
            });

#### Suppression de colonnes

Suppression de la colonne `description` de la table `baz` :

            Schema::table('baz', function (Blueprint $table) {
                $table->dropColumn('description');
            });

#### Ajout d'indexes après création d'une table

Le code suivant permet d'ajouter une contrainte d'unicité sur la colonne `nom` et un index sur la colonne `total` :

            Schema::table('baz', function (Blueprint $table) {
                $table->unique('nom');
                $table->index('total');
            });

#### Suppression d'indexes

Le code suivant permet de supprimer des indexes :

            Schema::table('baz', function (Blueprint $table) {
                $table->dropUnique(['nom']);
                $table->dropIndex(['total']);
            });

#### Ajout de contrainte de clé étrangère après création d'une table

Le code suivant permet d'ajouter une contrainte de clé étrangère sur la colonne `foo_id` :

            Schema::table('baz', function (Blueprint $table) {
                $table->foreignId('foo_id')->references('id')->on('foo');
            });

#### Suppression de contrainte de clé étrangère

Le code suivant permet de supprimer des clés étrangères :

            Schema::table('baz', function (Blueprint $table) {
                $table->dropForeign(['foo_id']);
                $table->dropColumn('foo_id');
            });

### Exemple de relations cardinales

#### Relation `one to one`

- un `foo` ne peut avoir qu'un seul `bar`
- un `bar` ne peut avoir qu'un seul `foo`

La création de la table `foo` :

            Schema::create('foo', function (Blueprint $table) {
                $table->id();
                $table->timestamps();
            });

La création de la table `bar` :

            Schema::create('bar', function (Blueprint $table) {
                $table->id();
                $table->timestamps();
            });

L'ajout de la colonne et de la contrainte de clé étrangère :

            Schema::table('foo', function (Blueprint $table) {
                $table->foreignId('bar_id')->unique()->references('id')->on('bar');
            });

Note : nous aurions aussi pu ajouter la contrainte à la table `bar` au lieu de la table `foo`. Cela ne fait aucune différence du point de vue des fonctionnalités. Le côté où il y a la contrainte (ici la table `foo`) est appelé le coté « possédant ».

La suppression de la colonne et de la contrainte de clé étrangère :

            Schema::table('foo', function (Blueprint $table) {
                $table->dropForeign(['bar_id']);
                $table->dropColumn('bar_id');
            });

La suppression des tables :

            Schema::dropIfExists('foo');
            Schema::dropIfExists('bar');

#### Relation `one to many`

- un `foo` peut avoir plusieurs `bar`
- un `bar` ne peut avoir qu'un seul `foo`

La création de la table `foo` :

            Schema::create('foo', function (Blueprint $table) {
                $table->id();
                $table->timestamps();
            });

La création de la table `bar` :

            Schema::create('bar', function (Blueprint $table) {
                $table->id();
                $table->timestamps();
            });

L'ajout de la colonne et de la contrainte de clé étrangère :

            Schema::table('bar', function (Blueprint $table) {
                $table->foreignId('foo_id')->references('id')->on('foo');
            });

La suppression de la colonne et de la contrainte de clé étrangère :

            Schema::table('bar', function (Blueprint $table) {
                $table->dropForeign(['foo_id']);
                $table->dropColumn('foo_id');
            });

La suppression des tables :

            Schema::dropIfExists('foo');
            Schema::dropIfExists('bar');

#### Relation `many to one`

- un `foo` ne peut avoir qu'un seul `bar`
- un `bar` peut avoir plusieurs `foo`

La création de la table `foo` :

            Schema::create('foo', function (Blueprint $table) {
                $table->id();
                $table->timestamps();
            });

La création de la table `bar` :

            Schema::create('bar', function (Blueprint $table) {
                $table->id();
                $table->timestamps();
            });

L'ajout de la colonne et de la contrainte de clé étrangère :

            Schema::table('foo', function (Blueprint $table) {
                $table->foreignId('bar_id')->references('id')->on('bar');
            });

La suppression de la colonne et de la contrainte de clé étrangère :

            Schema::table('foo', function (Blueprint $table) {
                $table->dropForeign(['bar_id']);
                $table->dropColumn('bar_id');
            });

La suppression des tables :

            Schema::dropIfExists('foo');
            Schema::dropIfExists('bar');

#### Relation `many to many`

Pour la relation `many to many` nous sommes obligés de créer une table de jointure.

- un `foo` peut avoir plusieurs `bar`
- un `bar` peut avoir plusieurs `foo`

La création de la table `foo` :

            Schema::create('foo', function (Blueprint $table) {
                $table->id();
                $table->timestamps();
            });

La création de la table `bar` :

            Schema::create('bar', function (Blueprint $table) {
                $table->id();
                $table->timestamps();
            });

La création de la table de jointure `bar_foo` :

            Schema::create('bar_foo', function (Blueprint $table) {
                $table->foreignId('bar_id')->references('id')->on('bar');
                $table->foreignId('foo_id')->references('id')->on('foo');
            });

ATTENTION : les tables de jointure ne doivent pas avoir de clé primaire (fonction `id()`) ou de système d'horodatage (fonction `timestamps()`).

NOTE : une convention est de nommer les tables de jointure en utilisant le nom des tables reliées et en les mettant dans l'ordre alphabétique (d'où le nom `bar_foo`).

La suppression des tables :

            Schema::dropIfExists('bar_foo');
            Schema::dropIfExists('foo');
            Schema::dropIfExists('bar');

