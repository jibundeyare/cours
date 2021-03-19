# Laravel 8 trouble shooting

## Erreur `Index column size too large.` (la colonne d'index est trop grande)

Cette erreur arrive quand on essaye de créer un index ou une contrainte (avec MariaDB ou MySQL) sur un champ `varchar`, et qu'on utilise un codage de caractères `utf8mb4` avec le moteur de stockage `InnoDB`.
Les index et les contraintes sont limités à une longueur de `767` octets.
Le champ en question ne fait que `255` caractères de long, mais avec `utf8mb4`, `1 caractère == 4 octets`.
Avec un champ de `255` caractères, nous avons donc un index ou une contrainte qui fait `4 * 255 caractères == 1020 octets`.

    [Illuminate\Database\QueryException]
      SQLSTATE[HY000]: General error: 1709 Index column size too large. The maximum column size is 767 bytes. (SQL: alter table `foo` add unique `foo_name_unique`(`name`))

Pour corriger ce problème, la solution la plus simple est de réduire la taille du champ `varchar`.
La longueur maximum est `767 / 4 == 191.75`, c-à-d, à peu près `190` caractères.

Par exemple, le code :

    $table->string('email')->unique();

devient :

    $table->string('email', 190)->unique();

- [indexing - How to fix MySql: index column size too large (Laravel migrate) - Stack Overflow](https://stackoverflow.com/questions/42043205/how-to-fix-mysql-index-column-size-too-large-laravel-migrate)

## Je n'arrive pas à affecter les attributs `CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP` aux colonnes `created_at` et `updated_at`

Ces attributs sont utiles pour automatiquement affecter un horodatage lors de la création ou la modification d'un objet dans la BDD.

Si vous utilisez Eloquent pour insérer ou mettre à jour des données, les colonnes `created_at` et `updated_at` seront automatiquement mises à jour.

- [php - Laravel timestamps() doesn't create CURRENT_TIMESTAMP - Stack Overflow](https://stackoverflow.com/questions/34515938/laravel-timestamps-doesnt-create-current-timestamp)
- [php - How Can I Set the Default Value of a Timestamp Column to the Current Timestamp with Laravel Migrations? - Stack Overflow](https://stackoverflow.com/questions/18067614/how-can-i-set-the-default-value-of-a-timestamp-column-to-the-current-timestamp-w)
- [timestamp - Can Laravel's Eloquent update a creation time but not an update time? - Stack Overflow](https://stackoverflow.com/questions/32956879/can-laravels-eloquent-update-a-creation-time-but-not-an-update-time?noredirect=1&lq=1)

Si vous n'utilisez pas Eloquent, il va falloir le faire à la main (en PHP).
Ou demander à la BDD de le faire :

                // @warning remplace $table->timestamps();
                $table->timestamp('created_at')->useCurrent();
                $table->timestamp('updated_at')->nullable()->useCurrentOnUpdate();

Et le tour est joué.

