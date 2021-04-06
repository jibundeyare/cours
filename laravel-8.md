# Laravel 8

Un projet « témoin » accompagne ce cours : [https://github.com/jibundeyare/src-laravel-8](https://github.com/jibundeyare/src-laravel-8).

## Prérequis

- PHP
- MariaDB ou MySQL
- la commande `composer`

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

Avec `laravel` :

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

créer une vue parent et des vues enfants

- [Views - Laravel - The PHP Framework For Web Artisans](https://laravel.com/docs/8.x/views#creating-and-rendering-views)
- [Blade Templates - Laravel - The PHP Framework For Web Artisans](https://laravel.com/docs/8.x/blade#layouts-using-template-inheritance)

### Affichage d'une variable

Pour afficher une variable, vous pouvez utiliser des accolades `{}`.

    {{ $foo }}

### Intégration de fichiers CSS et JS dans un template Blade

Les fichiers CSS et JS sont compilés (transpilés en réalité) et stockés dans le dossier `public`.

Pour que votre navigateur puisse accéder aux fichiers, vous pouvez ajouter la ligne suivante dans votre template Blade :

    <link rel="stylesheet" href="{{ asset('css/app.css') }} ">

Et voici la ligne pour le JS :

    <script src="{{ asset('js/app.js') }}"></script>

La fonction `asset()` permet de retrouver automatiquement le dossier dans lequel sont stockés les ressources statiques compilées (c-à-d le CSS et le JS).

générer des urls de routes nommées

[Routing - Laravel - The PHP Framework For Web Artisans](https://laravel.com/docs/8.x/routing#named-routes)

afficher des urls de routes nommées dans un template blade

    {{ route('foo') }}
    {{ route('foo', ['id' => 123]) }}
    {{ route('foo', ['id' => 123, 'bar' => 42]) }}

[Compiling Assets (Mix) - Laravel - The PHP Framework For Web Artisans](https://laravel.com/docs/8.x/mix)

[Compiling Assets (Mix) - Laravel - The PHP Framework For Web Artisans](https://laravel.com/docs/8.x/mix#installing-laravel-mix)

installer les packages js et compiler les fichiers CSS et JS avec :

    npm install
    npm run dev

activer browsersync

[Compiling Assets (Mix) - Laravel - The PHP Framework For Web Artisans](https://laravel.com/docs/8.x/mix#browsersync-reloading)

Dans `webpack.mix.js` :

    mix.browserSync('127.0.0.1:8000');

    // ou si 127.0.0.1 ne fonctionne pas
    mix.browserSync('localhost:8000');

activer le watch

[Compiling Assets (Mix) - Laravel - The PHP Framework For Web Artisans](https://laravel.com/docs/8.x/mix#watching-assets-for-changes)

    npm run watch

désactiver les notifications

Dans `webpack.mix.js` :

    mix.disableNotifications();

intéger des fichiers CSS et JS dans des vues

[Blade Templates - Laravel - The PHP Framework For Web Artisans](https://laravel.com/docs/8.x/blade#stacks)

## Le schéma de base de données (BDD)

Vous pouvez créer le schéma de BDD « à la main » en écrivant du code SQL ou en utilisant PhpMyAdmin.
Ou alors vous pouvez utiliser le système de « migration » de Laravel.

Le système de migration consiste à indiquer comment créer le schéma de BDD avec du code PHP.

### Exécution des fichiers de migrations

La commande suivante exécute les fichiers de migrations :

    php artisan migrate

Mais en phase de développement, il est souvent plus pratique de de détruire toutes les tables puis d'exécuter tous les fichiers de migration depuis le début.
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
- `nom` : varchar, 100, nullable
- `prenom` : varchar 100, nullable
- `mot_de_passe` : varchar 100
- `tel` : varchar 10, nullable
- `adresse1` : text, nullable
- `adresse2` : text, nullable
- `localite` : varchar 100, nullable
- `code_postal` : integer, nullable
- `actif` : boolean, default TRUE

Voici le code :

            Schema::create('foo', function (Blueprint $table) {
                $table->id();
                $table->string('email', 100)->unique();
                $table->string('nom', 100);
                $table->string('prenom', 100);
                $table->string('mot_de_passe', 100);
                $table->string('tel', 10)->nullable();
                $table->text('adresse1', 100)->nullable();
                $table->text('adresse2', 100)->nullable();
                $table->string('localite', 100)->nullable();
                $table->integer('code_postal')->nullable();
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

Vous ne devez pas créer de nouvelle table, vous devez modifier une table existante.
De même, il ne faudra pas ajouter de nouvelle colonne, il faudra modifier une colonne existante.

Voici le code de création d'une table nommée `baz` :

            Schema::create('baz', function (Blueprint $table) {
                $table->id();
                $table->string('nom');
                $table->integer('total');
                $table->boolean('actif');
                $table->foreignId('foo_id')
                $table->timestamps();
            });

#### Modifications de colonnes

Le code suivant permet de changer la taille de la colonne `nom`, de rendre nullable la colonne `total` et de définir une valeur par défaut pour la colonne `actif` :

            Schema::table('baz', function (Blueprint $table) {
                $table->string('nom', 190)->change();
                $table->integer('total')->nullable()->change();
                $table->boolean('actif')->default(true);
            });

#### Ajout d'indexes après création d'une table

Le code suivant permet d'ajouter une contrainte d'unicité sur la colonne `nom` et un index sur la colonne `total` :

            Schema::table('baz', function (Blueprint $table) {
                $table->unique('nom');
                $table->index('total');
            });

#### Ajout de contrainte de clé étrangère après création d'une table

Le code suivant permet d'ajouter une contrainte de clé étrangère sur la colonne `foo_id` :

            Schema::table('baz', function (Blueprint $table) {
                $table->foreign('foo_id')->references('id')->on('foo');
            });

