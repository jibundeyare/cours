# Laravel 9 - Authentification

## Installation de Breeze

Breeze utilise [Tailwind CSS](https://tailwindcss.com/).

Installation de Breeze :

```bash
composer require laravel/breeze --dev
php artisan breeze:install
php artisan migrate
npm install
npm run dev
```

Les routes suivantes sont disponibles :

[http://localhost:8000/register](http://localhost:8000/register)
[http://localhost:8000/login](http://localhost:8000/login)
[http://localhost:8000/profile](http://localhost:8000/profile)
[http://localhost:8000/logout](http://localhost:8000/logout)

Toutes les routes sont dans le fichier : `routes/auth.php`.

## Trouble shooting

### Le fichier `welcome.blade.php` est manquant

```
ErrorException

file_get_contents(/home/daishi/projects/laravel-9/resources/views/welcome.blade.php): Failed to open stream: No such file or directory
```

La commande `php artisan breeze:install` ne fonctionne que si le fichier `welcome.blade.php` est présent.

[laravel/welcome.blade.php at 9.x · laravel/laravel](https://github.com/laravel/laravel/blob/9.x/resources/views/welcome.blade.php)
[raw.githubusercontent.com/laravel/laravel/9.x/resources/views/welcome.blade.php](https://raw.githubusercontent.com/laravel/laravel/9.x/resources/views/welcome.blade.php)

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

Note : vous avez le droit de renseigner une URL complète comme `https://example.com/foo`.
L'URL ne doit pas nécessairement rediriger vers le même nom de domaine, comme `https://google.com`.

## Obtenir des informations sur l'utilisateur dans un contrôleur

`app/Http/Controllers/FooController.php`

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

```blade
<p> User ID: {{ auth()->user()->id }} </p>
<p> User Name: {{ auth()->user()->name }} </p>
<p> User Email: {{ auth()->user()->email }} </p>

<p> User ID: {{ Auth::user()->id }} </p>
<p> User Name: {{ Auth::user()->name }} </p>
<p> User Email: {{ Auth::user()->email }} </p>

@auth
    L'utilisateur est authentifié
@else
    L'utilisateur n'est pas authentifié
@endauth

@guest
    L'utilisateur n'est pas authentifié
@else
    L'utilisateur est authentifié
@endguest

@if (Auth::check())
    L'utilisateur est authentifié
@endif

@if (!Auth::check())
    L'utilisateur n'est pas authentifié
@endif
```

## La protection des routes par mot de passe

```php
// seuls les utilisateurs authentifiés peuvent accéder à cette route
Route::get('/foo', function () {
    return view('foo');
})->middleware('auth');

// seuls les utilisateurs authentifiés peuvent accéder à cette route
Route::get('/foo', [FooController::class, 'index'])->middleware('auth');
```

## Comment hasher ou vérifier un mot de passe

```php
use Illuminate\Support\Facades\Hash;

// hachage de mot de passe
$hashedPassword = Hash::make('123');

if (Hash::check('123', $hashedPassword)) {
    // les mots de passe correspondent
}
```

## Créer un seeder d'utilisateurs

```bash
php artisan make:seeder UserSeeder
```

`database/seeders/UserSeeder.php`

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

## Déconnexion

```blade
<form action="{{ route('logout') }}" method="POST">
    @csrf
    <a href="{{ route('logout') }}" onclick="event.preventDefault(); this.closest('form').submit();">déconnexion</a>
</form>
```

