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

