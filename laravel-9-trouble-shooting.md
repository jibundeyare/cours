# Laravel 9 trouble shooting

## Les contraintes de clés étrangères ne sont pas appliquées dans la BDD

Si vous utilisez MariaDB ou MySQL, la cause la plus probable est que vos tables utilisent le moteur de stockage `MyISAM` au lieu de `InnoDB`.

Pour régler ce problème, vous devez configurer votre BDD pour qu'elle utilise `InnoDB` par défaut.
(Désolé, je n'ai pas de tuto pour ça, vous devez faire la recherche par vous même.)

Sinon vous pouvez demander à Laravel d'utiliser le moteur de stockage `InnoDB` par défaut lors des requêtes de création de table.

Ouvrez le fichier `config/database.php` et affectez la valeur 'InnoDB' à la clé `engine` (ligne `60`) :

```diff-php
              'prefix_indexes' => true,
              'strict' => true,
-             'engine' => null,
+             'engine' => 'InnoDB',
              'options' => extension_loaded('pdo_mysql') ? array_filter([
                  PDO::MYSQL_ATTR_SSL_CA => env('MYSQL_ATTR_SSL_CA'),
              ]) : [],
```

Pour que ce réglage prenne effet, vous devez détruire votre BDD et la recréer avec la commande suivant :

```bash
php artisan db:wipe && php artisan migrate
```

## Les relations entre les tables n'apparaissent pas dans la fenêtre concepteur de PhpMyAdmin

Voir l'erreur « Les contraintes de clés étrangères ne sont pas appliquées dans la BDD » ci dessus.

## Erreur `1071 Specified key was too long` (`1071 La clé est trop longue`)

Cette erreur arrive quand on essaye de créer un index ou une contrainte (avec MariaDB ou MySQL) sur un champ `varchar`, et qu'on utilise un codage de caractères `utf8mb4` avec le moteur de stockage `InnoDB`.
Les index et les contraintes sont limités à une longueur de `767` octets.
Le champ en question ne fait que `255` caractères de long, mais avec `utf8mb4`, `1 caractère == 4 octets`.
Avec un champ de `255` caractères, nous avons donc un index ou une contrainte qui fait `4 * 255 caractères == 1020 octets`.

```
[Illuminate\Database\QueryException]
  SQLSTATE[42000]: Syntax error or access violation: 1071 Specified key was too long; max key length is 767 bytes (SQL: alter table users add unique users_email_unique(email))
```

Pour corriger ce problème, la solution la plus simple est de réduire la taille du champ `varchar`.
La longueur maximum est `767 / 4 == 191.75`, c-à-d, à peu près `191` caractères.

Pour changer la taille d'un champ, deux cas se présentent :

1. ce n'est pas un champ `string()` et il n'y a pas de paramètre pour choisir la taille
  (c'est le cas avec un champ `morphs()` par exemple)
2. c'est un champ `string()` et vous pouvez changerle taille « manuelement »

### Cas 1 (champ autre que `string()`)

Il n'est pas possible de choisir la taille du champ.

Le seul moyen de corriger le problème est de changer la taille par défaut de tous les champs `varchar`.

Ouvrez le fichier `app/Providers/AppServiceProvider.php` et modifiez la méthode `boot()` tout en bas :

```diff-php
  <?php
  
  namespace App\Providers;
  
+ use Illuminate\Support\Facades\Schema;
  use Illuminate\Support\ServiceProvider;
  
  class AppServiceProvider extends ServiceProvider
  {
      /**
       * Register any application services.
       *
       * @return void
       */
      public function register()
      {
          //
      }
  
      /**
       * Bootstrap any application services.
       *
       * @return void
       */
      public function boot()
      {
-         //
+         Schema::defaultStringLength(191);
      }
  }
```

- [indexing - How to fix MySql: index column size too large (Laravel migrate) - Stack Overflow](https://stackoverflow.com/questions/42043205/how-to-fix-mysql-index-column-size-too-large-laravel-migrate)

### Cas 2 (champ de type `string()`)

Vous pouvez préciser la taille du champ « manuellement ».

Par exemple, le code :

```php
$table->string('foo')->unique();
$table->string('bar')->index();
```

devient :

```php
$table->string('foo', 191)->unique();
$table->string('bar', 191)->index();
```

## Erreur `1709 Index column size too large`

Même cause et même conséquences que l'erreur `1071 La clé est trop longue` ci-dessus.

```
[Illuminate\Database\QueryException]
  SQLSTATE[HY000]: General error: 1709 Index column size too large. The maximum column size is 767 bytes. (SQL: alter table `foo` add unique `foo_name_unique`(`name`))
```

Éviemment, la solution est aussi la même.
Voir ci-dessus.

## Je n'arrive pas à affecter les attributs `CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP` aux colonnes `created_at` et `updated_at`

Ces attributs sont utiles pour automatiquement affecter un horodatage lors de la création ou la modification d'un objet dans la BDD.

Si vous utilisez Eloquent pour insérer ou mettre à jour des données, les colonnes `created_at` et `updated_at` seront automatiquement mises à jour.

- [php - Laravel timestamps() doesn't create CURRENT_TIMESTAMP - Stack Overflow](https://stackoverflow.com/questions/34515938/laravel-timestamps-doesnt-create-current-timestamp)
- [php - How Can I Set the Default Value of a Timestamp Column to the Current Timestamp with Laravel Migrations? - Stack Overflow](https://stackoverflow.com/questions/18067614/how-can-i-set-the-default-value-of-a-timestamp-column-to-the-current-timestamp-w)
- [timestamp - Can Laravel's Eloquent update a creation time but not an update time? - Stack Overflow](https://stackoverflow.com/questions/32956879/can-laravels-eloquent-update-a-creation-time-but-not-an-update-time?noredirect=1&lq=1)

Si vous n'utilisez pas Eloquent, il va falloir le faire à la main (en PHP).
Ou demander à la BDD de le faire :

```php
// @warning remplace $table->timestamps();
$table->timestamp('created_at')->useCurrent();
$table->timestamp('updated_at')->nullable()->useCurrentOnUpdate();
```

Et le tour est joué.

## Mon formulaire génère une erreur `419 Page expired`

Ajoutez l'instruction `@csrf` dans votre formulaire :

```blade
<form action="{{ route('foo') }}" method="post">
    @csrf
</form>
```

