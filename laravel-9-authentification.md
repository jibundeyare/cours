# Laravel 9 - Authentification

Le framework Laravel peut utiliser différents systèmes d'authentification en fonction des besoins.
Le plus simple de ces systèmes s'appelle Breeze.

## Installation de Breeze

Voici les commandes à taper à la racine de votre projet :

```bash
composer require laravel/breeze --dev
php artisan breeze:install
php artisan migrate
npm install
npm run dev
```

Après installation les routes suivantes sont disponibles :

[http://localhost:8000/register](http://localhost:8000/register)
[http://localhost:8000/login](http://localhost:8000/login)
[http://localhost:8000/profile](http://localhost:8000/profile)
[http://localhost:8000/logout](http://localhost:8000/logout)

Pour voir toutes les routes, regardez dans les fichiers :

- `routes/web.php` qui est le fichier des routes
- `routes/auth.php` qui est un fichier de route spécifique à Breeze

## Trouble shooting

### Le fichier `welcome.blade.php` est manquant

```
ErrorException

file_get_contents(/home/daishi/projects/laravel-9/resources/views/welcome.blade.php): Failed to open stream: No such file or directory
```

La commande `php artisan breeze:install` ne fonctionne que si le fichier `welcome.blade.php` est présent.

Vous pouvez télécharger le fichier `welcome.blade.php` depuis le code source de Laravel ci-dessous (il s'agit du même fichier) :

- [laravel/welcome.blade.php at 9.x · laravel/laravel](https://github.com/laravel/laravel/blob/9.x/resources/views/welcome.blade.php)
- [raw.githubusercontent.com/laravel/laravel/9.x/resources/views/welcome.blade.php](https://raw.githubusercontent.com/laravel/laravel/9.x/resources/views/welcome.blade.php)

## Personnaliser des pages créés par Breeze

Breeze utilise [Tailwind CSS](https://tailwindcss.com/).

Les vues Blade se trouvent dans les dossiers suivants :

- `/resources/views/auth`
- `/resources/views/components`
- `/resources/views/layouts`
- `/resources/views/profile`

Vous pouvez les modifier comme n'importe quelle vue Blade.

## Protéger des routes par mot de passe

Concrètement, une route protégée par mot de passe oblige l'utilisateur à passer par un forumulaire de connexion avant de pouvoir visualiser la page demandée.

Pour protéger une route par mot de passe, vous devez modifier le fichier des routes `routes/web.php`.

Il suffit d'ajouter l'appel de fonction `->middleware('auth')` aux routes que vous voulez protéger :

```php
// seuls les utilisateurs authentifiés peuvent accéder à cette route
Route::get('/foo', function () {
    return view('foo');
})->middleware('auth')->name('foo');

// seuls les utilisateurs authentifiés peuvent accéder à cette route
Route::get('/foo', [FooController::class, 'index'])->middleware('auth')->name('foo.index');
```

## Choisir où l'utilisateur est redirigé après authentification

Ouvrez le fichier suivant : `app/Providers/RouteServiceProvider.php`.

Et adaptez la `HOME` :

```diff-php
      /**
       * The path to the "home" route for your application.
       *
       * Typically, users are redirected here after authentication.
       *
       * @var string
       */
-     public const HOME = '/dashboard';
+     public const HOME = '/foo';
```

Note : vous avez le droit de renseigner une URL complète (comme `https://example.com/foo`) et l'URL ne doit pas nécessairement rediriger vers le même nom de domaine (comme `https://google.com`).

## Obtenir des informations sur l'utilisateur dans un contrôleur

Voici ce que vous pouvez faire un contrôleur `app/Http/Controllers/FooController.php` :

```php
<?php

namespace App\Http\Controllers;

use Illuminate\Http\Request;
use Illuminate\Support\Facades\Auth;

class FooController extends Controller
{
    /**
     * @param  \Illuminate\Http\Request  $request
     * @return \Illuminate\Http\Response
     */
    public function index(Request $request)
    {
        // récupération de l'utilisateur actuellement connecté
        $user = $request->user();

        // récupération de l'utilisateur actuellement connecté
        $user = Auth::user();

        // récupération de l'id de utilisateur actuellement connecté
        $id = Auth::id();

        // vérification de l'authentification de l'utilisateur
        if (Auth::check()) {
            // l'utilisateur est authentifié
        }

        if (!Auth::check()) {
            // l'utilisateur n'est pas authentifié
        }

        return view('foo');
    }
}
```

## Obtenir des informations sur l'utilisateur dans une vue Blade

Voici ce que vous pouvez faire dans une vue Blade :

```blade
{{-- récupération de données de l'utilisateur--}}
<p> User ID: {{ auth()->user()->id }} </p>
<p> User Name: {{ auth()->user()->name }} </p>
<p> User Email: {{ auth()->user()->email }} </p>

{{-- méthode alternative de récupération de données de l'utilisateur--}}
<p> User ID: {{ Auth::user()->id }} </p>
<p> User Name: {{ Auth::user()->name }} </p>
<p> User Email: {{ Auth::user()->email }} </p>

{{-- vérification de l'authentificaion de l'utilisateur--}}
@auth
    L'utilisateur est authentifié
@else
    L'utilisateur n'est pas authentifié
@endauth

{{-- vérification de la non authentificaion de l'utilisateur--}}
@guest
    L'utilisateur n'est pas authentifié
@else
    L'utilisateur est authentifié
@endguest

{{-- méthode alternative de vérification de l'authentificaion de l'utilisateur--}}
@if (Auth::check())
    L'utilisateur est authentifié
@else
    L'utilisateur n'est pas authentifié
@endif

{{-- méthode alternative de vérification de la non authentificaion de l'utilisateur--}}
@if (!Auth::check())
    L'utilisateur n'est pas authentifié
@else
    L'utilisateur est authentifié
@endif
```

## Créer un bouton de déconnexion dans une vue Blade

Pour se déconnecter, il ne suffit pas de créer un lien qui pointe vers la route `logout`.
Il faut créer un formulaire qui envoie les données avec la méthode `POST`.
Ceci permet d'empêcher les attaques de type CSRF.

Vous pouvez rajouter ce formulaire dans votre barre de navigation.

ATTENTION : pensez à vérifier si l'utilisateur est connecté avant de lui proposer de se déconnecter.

Le formulaire suivant permet de créer un lien cliquable qui déclenche l'envoi du formulaire.

```blade
<form action="{{ route('logout') }}" method="post">
    @csrf
    <a href="{{ route('logout') }}" onclick="event.preventDefault(); this.closest('form').submit();">déconnexion</a>
</form>
```

Il est aussi possible d'utiliser un bouton (au lieu d'un lien cliquable) :

```blade
<form action="{{ route('logout') }}" method="post">
    @csrf
    <button type="submit">déconnexion</button>
</form>
```

## Comment hasher ou vérifier un mot de passe

### Hasher un mot de passe

Si vous ajoutez un seeder pour créer des utiliateur ou que vous voulez créer un formulaire de création ou modification d'utilisateur vous devrez hasher son mot de passe avant de le stocker en BDD.

Le code suivant est à placer dans un seeder ou un contrôleur :

```php
use Illuminate\Support\Facades\Hash;

// hachage de mot de passe
$hashedPassword = Hash::make('123');
```

### Vérifier un mot de passe

Le code suivant est à placer dans un contrôleur :

```php
use Illuminate\Support\Facades\Hash;

// hachage de mot de passe
$hashedPassword = Hash::make('123');

if (Hash::check('123', $hashedPassword)) {
    // les mots de passe correspondent
}
```

Le principe de la vérification d'un mot de passe est le suivant :

1. un utilisateur vous envoie un mot de passe clair via un formulaire HTML (on va l'appeler « le mot de passe de la requête »)
2. vous récupérez le mot de passe hashé depuis la BDD (on va l'appeler « le mot de passe de la BDD »)
3. vous hashez le mot de passe de la requête (on va l'appeler « le mot de passe hashé »)
4. vous comparez « le mot de passe de la BDD » et le mot de passe « hashé »
   si les deux coincident, c'est que l'utilisateur qui vous a envoyé « le mot de passe de la requête » connaît « le mot de passe de la BDD »

## Créer un seeder d'utilisateurs

Voici la commande pour créer un nouveau seeder vide :

```bash
php artisan make:seeder UserSeeder
```

La commande créé le fichier `database/seeders/UserSeeder.php`.

Voici un exemple de seeder qui permet de créer un ou plusieurs utilisateurs :

```php
<?php

namespace Database\Seeders;

use App\Models\User;
use Illuminate\Database\Console\Seeds\WithoutModelEvents;
use Illuminate\Database\Seeder;
use Illuminate\Support\Facades\Hash;

class UserSeeder extends Seeder
{
    /**
     * Run the database seeds.
     *
     * @return void
     */
    public function run()
    {
        $userDatas = [
            [
                'name' => 'admin',
                'email' => 'admin@example.com',
                'password' => '123',
            ]
        ];

        foreach ($userDatas as $userData) {
            $user = new User();
            $user->name = $userData['name'];
            $user->email = $userData['email'];
            $user->password = Hash::make($userData['password']);
            $user->save();
        }
    }
}
```

Notez l'utilisation de la fonction de hashage `Hash::make()` avant d'enregistrer le mot de passe.

