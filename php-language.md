# PHP language

## Notions

- variables (type simple, tableaux, objets)
- structures de contrôle (blocs de type `if`)
- boucles
- fonctions
- classes
- interfaces
- mixins

## Syntaxe

Ne pas fermer le bloc PHP avec `?>` si la page ne contient que du code PHP.

`=` est impératif (c'est un ordre).

`==` est interrogatif (c'est une question).

`<?=` et `<?php echo ` sont équivalent

`include` et `require` font la même chose (un copier-coller dynamique de fichier) mais `require` a l'avantage de stopper le programme si le fichier n'est pas trouvé.

`array()` est l'ancienne notation qui permet de créer un tableau. `[]` est la nouvelle notation (recommandée) qui permet de créer un tableau.

## Types de données

- chaîne de caractères (string)
- nombre entier (integer)
- nombre à virgule flottant (float)
- booléen (boolean)
- tableau (array)
- objet (object)

## Programmation orientée objet (POO)

En POO :

- une variable se nomme « attribut »
- une fonction se nomme « méthode »

## Sécurité

Les fonctions `filter_var()` et `filter_input()` permettent de valider ou de filtrer une donnée en provenance de l'utilisateur.

Exemple de validation d'un email :

    if (filter_var($_POST['email'], FILTER_VALIDATE_EMAIL)) {
        // email valide
    } else {
        // email non valide
    }

ou équivalent :

    if (filter_input(INPUT_POST, 'email', FILTER_VALIDATE_EMAIL)) {
        // email valide
    } else {
        // email non valide
    }

Exemple de filtrage d'un champ texte :

    $comment = filter_var($_POST['email'], FILTER_SANITIZE_SPECIAL_CHARS);

ou équivalent :

    $comment = filter_input(INPUT_POST, 'comment', FILTER_SANITIZE_SPECIAL_CHARS);

## Doc

- [PHP: Hypertext Preprocessor](https://secure.php.net/)
- [PHP: Operators - Manual](https://secure.php.net/manual/en/language.operators.php)
- [operators - Reference — What does this symbol mean in PHP? - Stack Overflow](https://stackoverflow.com/questions/3737139/reference-what-does-this-symbol-mean-in-php?rq=1)
- [PHP: String Functions - Manual](https://secure.php.net/manual/en/ref.strings.php)
- [PHP: Date/Time - Manual](https://secure.php.net/manual/en/book.datetime.php)
- [PHP: date - Manual](http://php.net/manual/en/function.date.php)
- [PHP: number_format - Manual](http://php.net/manual/en/function.number-format.php)
- [PHP: Filter - Manual](https://secure.php.net/manual/en/book.filter.php)
- [PHP: filter_input - Manual](https://secure.php.net/manual/en/function.filter-input.php)
- [PHP: filter_var - Manual](https://secure.php.net/manual/en/function.filter-var.php)
- [PHP: Validate filters - Manual](http://php.net/manual/en/filter.filters.validate.php)
- [PHP: Sanitize filters - Manual](http://php.net/manual/en/filter.filters.sanitize.php)
- [PHP: Other filters - Manual](http://php.net/manual/en/filter.filters.misc.php)
