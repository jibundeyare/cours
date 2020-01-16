# Laravel

[Installation - Laravel - The PHP Framework For Web Artisans](https://laravel.com/docs/6.x/)

- configurer la langue (locale)
[Localization - Laravel - The PHP Framework For Web Artisans](https://laravel.com/docs/6.x/localization#configuring-the-locale)

- créer des routes nommées
[Routing - Laravel - The PHP Framework For Web Artisans](https://laravel.com/docs/6.x/routing#basic-routing)

- créer des contrôleurs
[Controllers - Laravel - The PHP Framework For Web Artisans](https://laravel.com/docs/6.x/controllers#single-action-controllers)
`php artisan make:controller FooController`
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
`{{ route('foo') }}`
`{{ route('foo', ['id' => 123]) }}`
`{{ route('foo', ['id' => 123, 'bar' => 42]) }}`

- installer le package laravel/ui avec `composer require laravel/ui --dev`
[JavaScript & CSS Scaffolding - Laravel - The PHP Framework For Web Artisans](https://laravel.com/docs/6.x/frontend)
- installer les packages js et compiler les fichiers CSS et JS avec :
```
npm install
npm run dev
```

- activer browsersync
[Compiling Assets (Mix) - Laravel - The PHP Framework For Web Artisans](https://laravel.com/docs/6.x/mix#browsersync-reloading)
`mix.browserSync('localhost:8000');`
`mix.browserSync('127.0.0.1:8000');`

- activer le watch
[Compiling Assets (Mix) - Laravel - The PHP Framework For Web Artisans](https://laravel.com/docs/6.x/mix#running-mix)
`npm run watch`

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

- ajouter des variables dans les routes et les transmettre aux actions
- définir des contraintes sur les variables de routes
- ajouter des paramètres aux routes et les récupérer dans une actions
- transmettre des variables dans les vues
- afficher des variables dans les vues

- activer la variable de session
- lire des données dans la variable de session
- insérer des données dans la variable de session
- supprimer des données dans la variable de session

- configurer l'accès à la BDD
- créer des fichiers de migration
- exécuter les fichiers de migration
- créer les classes de modèle de données
- lire des données dans la BDD
- injecter des données dans la BDD
- modifier des données dans la BDD
- supprimer des données dans la BDD
- créer des formulaires avec protection CSRF
- créer des routes en mode POST, PUT, DELETE
- récupérer les données d'un formulaire dans une action
- valider les données d'un formulaire dans une action
- afficher les messages d'erreur d'un formulaire dans une vue
- afficher un message flash
