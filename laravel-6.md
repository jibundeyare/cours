# Laravel 6

Un projet « témoin » accompagne ce cours : [jibundeyare/src-laravel-6](https://github.com/jibundeyare/src-laravel-6).

## Tutoriels

- [Laravel 6 CRUD Application Tutorial](https://www.itsolutionstuff.com/post/laravel-6-crud-application-tutorialexample.html)
- [Laravel 6 CRUD Example | Laravel 6 Tutorial For Beginners](https://appdividend.com/2019/09/12/laravel-6-crud-example-laravel-6-tutorial-for-beginners/) (ce tutoriel est moins bien)

## Création d'une application avec le framework Laravel

[Installation - Laravel - The PHP Framework For Web Artisans](https://laravel.com/docs/6.x/)

- configurer la langue (locale)
[Localization - Laravel - The PHP Framework For Web Artisans](https://laravel.com/docs/6.x/localization#configuring-the-locale)

- créer des routes nommées
[Routing - Laravel - The PHP Framework For Web Artisans](https://laravel.com/docs/6.x/routing#basic-routing)
Modifier `routes/web.php`.
```
// une route par action
Route::get('/', 'MainController@index')->name('main.index');
```
```
// une route pour toutes les actions liées à une ressource
Route::resource('foo', 'FooController');
```
Afficher la liste de toutes les routes :
```
php artisan route:list
```


- créer des contrôleurs
[Controllers - Laravel - The PHP Framework For Web Artisans](https://laravel.com/docs/6.x/controllers#single-action-controllers)
```
php artisan make:controller -r FooController
```
- créer des actions
[Controllers - Laravel - The PHP Framework For Web Artisans](https://laravel.com/docs/6.x/controllers#single-action-controllers)

- créer une vue parent
[Views - Laravel - The PHP Framework For Web Artisans](https://laravel.com/docs/6.x/views#creating-views)
[Blade Templates - Laravel - The PHP Framework For Web Artisans](https://laravel.com/docs/6.x/blade)
- créer des vues enfants
[Views - Laravel - The PHP Framework For Web Artisans](https://laravel.com/docs/6.x/views#creating-views)
[Blade Templates - Laravel - The PHP Framework For Web Artisans](https://laravel.com/docs/6.x/blade)

- générer des urls de routes nommées
[Routing - Laravel - The PHP Framework For Web Artisans](https://laravel.com/docs/6.x/routing#named-routes)
- afficher des urls de routes nommées dans un template blade
```
{{ route('foo') }}
{{ route('foo', ['id' => 123]) }}
{{ route('foo', ['id' => 123, 'bar' => 42]) }}
```

- installer le package laravel/ui avec `composer require laravel/ui --dev`
[JavaScript & CSS Scaffolding - Laravel - The PHP Framework For Web Artisans](https://laravel.com/docs/6.x/frontend)
- installer les packages js et compiler les fichiers CSS et JS avec :
```
npm install
npm run dev
```

- activer browsersync
[Compiling Assets (Mix) - Laravel - The PHP Framework For Web Artisans](https://laravel.com/docs/6.x/mix#browsersync-reloading)
```
mix.browserSync('localhost:8000');
mix.browserSync('127.0.0.1:8000');
```

- activer le watch
[Compiling Assets (Mix) - Laravel - The PHP Framework For Web Artisans](https://laravel.com/docs/6.x/mix#running-mix)
```
npm run watch
```

- intéger des fichiers CSS et JS dans des vues
[Blade Templates - Laravel - The PHP Framework For Web Artisans](https://laravel.com/docs/6.x/blade#stacks)

- activer l'authentification
[Authentication - Laravel - The PHP Framework For Web Artisans](https://laravel.com/docs/6.x/authentication#included-routing)
```
composer require laravel/ui --dev
php artisan ui vue --auth
npm install
npm run dev
```
- protéger des pages par mot de passe
[Authentication - Laravel - The PHP Framework For Web Artisans](https://laravel.com/docs/6.x/authentication#included-routing)

- désactiver les inscriptions
Dans `routes/web.php`, remplacer `Auth::routes();` par `Auth::routes(['register' => false]);`

- traduction des vues auth
[Localization - Laravel - The PHP Framework For Web Artisans](https://laravel.com/docs/6.x/localization)
[Multi-Language Routes and Locales with Auth - Laravel Daily](https://laraveldaily.com/multi-language-routes-and-locales-with-auth/)
Créer un fichier de traduction `resources/lang/fr.json`
- routes multi-langue
[Multi-Language Routes and Locales with Auth - Laravel Daily](https://laraveldaily.com/multi-language-routes-and-locales-with-auth/)

## Todo

- rediriger vers une autre page à partir d'une action

- configurer l'accès à la BDD

- créer les classes de modèle de données
[Eloquent: Getting Started - Laravel - The PHP Framework For Web Artisans](https://laravel.com/docs/6.x/eloquent#defining-models)
Ajouter ce code dans votre modèle si la table n'a pas un nom standard (c-à-d le pluriel avec autre chose qu'un `s`) :
```
/**
    * The table associated with the model.
    *
    * @var string
    */
protected $table = 'lieux';
```
```
php artisan make:model -m Foo
```

- créer des fichiers de migration
[Database: Migrations - Laravel - The PHP Framework For Web Artisans](https://laravel.com/docs/6.x/migrations#creating-columns)

- exécuter les fichiers de migration
```
php artisan migrate
```
- lire des données dans la BDD
Hack pour vérifier que l'on a bien récupéré les données :
```
$lieux = Lieu::all();

echo '<pre>';
var_dump($lieux);
echo '</pre>';
exit();
```

- injecter des données dans la BDD
- modifier des données dans la BDD
- supprimer des données dans la BDD

- transmettre des variables dans les vues
[Views - Laravel - The PHP Framework For Web Artisans](https://laravel.com/docs/6.x/views#creating-views)
```
compact('lieux')
// équivalent de
[
    'lieux' => $lieux,
]
```
- afficher des variables dans les vues
[Blade Templates - Laravel - The PHP Framework For Web Artisans](https://laravel.com/docs/6.x/blade#loops)

- ajouter des variables dans les routes et les transmettre aux actions
- définir des contraintes sur les variables de routes
- ajouter des paramètres aux routes et les récupérer dans une actions

- créer des formulaires avec protection CSRF
- créer des routes en mode POST, PUT, DELETE
- récupérer les données d'un formulaire dans une action
- valider les données d'un formulaire dans une action
[Validation - Laravel - The PHP Framework For Web Artisans](https://laravel.com/docs/6.x/validation)
[Creating A Custom Form Request In Laravel - rfmeier.net](https://rfmeier.net/creating-custom-form-request-laravel/)
- afficher les messages d'erreur d'un formulaire dans une vue
```
@if ($errors->any())
    <div class="alert alert-danger">
        <ul>
            @foreach ($errors->all() as $error)
                <li>{{ $error }}</li>
            @endforeach
        </ul>
    </div>
@endif
```
- afficher un message flash

- activer la variable de session
- lire des données dans la variable de session
- insérer des données dans la variable de session
- supprimer des données dans la variable de session

