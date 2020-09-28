# PHP language basic

## Notions

- variables (types de données simples, tableaux, objets)
- constantes
- alternatives (blocs de type `if`, `else`, `else if` et `switch`)
- boucles (blocs de type `foreach`, `while`, `do while` et `for`)
- fonctions
- classes
- interfaces
- mixins

## Types de données

- nombre entier (integer)
- nombre à virgule flottant (float)
- booléen (boolean)
- chaîne de caractères (string)
- valeur nulle (null value)
- tableau (array)
- objet / classe (object / class)

## Valeurs vides

Toutes ces valeurs renvoient `true` si on les utilise avec la fonction `empty()` :

- `0` : zéro (nombre entier)
- `.0` : zéro (nombre à virgule flottante)
- `false`
- `''` ou `""` : une chaîne de caractères vide
- `'0'` ou `"0"` : une chaîne de caractères avec un zéro
- `[]` ou `array()` : un tableau vide
- `null` : la valeur nulle

## Opérateurs

- `=` : affectation
- `==` : égalité
- `>` : plus grand que
- `<` : plus petit que
- `>=` : plus grand ou égal à
- `<=` : plus petit ou égal à
- `!=` : différent de
- `===` : identité (type de données identique et valeur identique)
- `!==` : non identité (type de données différent ou valeur différente)
- `&&` : « et » logique
- `||` : « ou » logique

## Affectation

Une affectation est comme une affirmation.
On affirme que telle variable va contenir telle valeur.

Exemple d'affectation :

    $foo = 123;

## Conditions

Une condition est comme une question.
On demande si telle variable contient telle valeur.

Exemple de condition :

    // comparaison d'égalité entre la variable $foo et la valeur 123
    if ($foo == 123) {
        echo 'foo contient la valeur 123';
    }

## Fonctions

Une fonction doit d'abord être définie.
Quand une fonction a été définie, on peut l'appeler (c-à-d l'utiliser).

Elle peut prendre un paramètre, plusieurs paramètres ou aucun paramètre.
Elle peut renvoyer une valeur ou n'en renvoyer aucune.

Exemple de définition d'une fonction qui prend deux paramètres et renvoit une valeur :

    function foo($a, $b) {
        return $a + $b;
    }

Exemple d'appel :

    // appel de la fonction foo() et affectation de la valeur retournée à la variable $result
    $result = foo(42, 123);

## Syntaxe

### Bloc PHP

Ne pas fermer le bloc PHP avec `?>` si la page ne contient que du code PHP.

`<?=` et `<?php echo ` sont équivalent

### Signe égal

`=` est impératif (c'est un ordre).
Ce symbol sert à affecter (attribuer) une valeur à une variable.

`==` est interrogatif (c'est une question).
Ce symbol sert à vérifier l'égalité entre deux valeurs.
La plupart du temps, on le trouve dans des blocs de type `if`.

`===` est interrogatif aussi.
Ce symbol sert à vérifier que deux valeurs sont égales et que leur type de données est aussi identique.
La plupart du temps, on le trouve aussi dans des blocs de type `if`.

### Inclusion

L'instruction `require()` permet d'insérer le code provenant d'un autre fichier dans le fichier courant.
On peut imaginer que PHP fait un copier-coller à notre place.

Les instructions `require()` et `include()` font la même chose (un copier-coller dynamique de fichier) mais `require()` a l'avantage de stopper le programme si le fichier n'est pas trouvé.

### Tableaux

`[]` est la notation actuelle (recommandée) qui permet de créer un tableau.
`array()` est l'ancienne notation qui permet de créer un tableau.

Exemple :

    // notation actuelle
    $bar = [];
    $bar2 = [42, 'azerty', 3.14, true];

    // ancienne notation
    $foo = array();
    $foo2 = array(42, 'azerty', 3.14, true);

### Intégration de code PHP dans du code HTML

Pour intégrer du code PHP dans du code HTML, il est recommandé d'utiliser une syntaxe alternative dans tous les blocs de type structure de contrôle ou boucle.

Tous les mots clés suivants ont une syntaxe alernative : `if`, `while`, `for`, `foreach`, et `switch`.
Les blocs de ces mots-clés peuvent être terminés avec les mots-clés suivants : `endif;`, `endwhile;`, `endfor;`, `endforeach;` et `endswitch;`.

Voici un exemple avec une boucle `foreach`, codé « comme il faut »:

    <ul>
        <?php foreach ($rows as $row): ?>
        <li><?= $row ?></li>
        <?php endforeach ?>
    </ul>

Voici le même exemple mais codé « pas comme il faut » :

    <ul>
        <?php foreach ($rows as $row) {
            echo '<li>'.$row.'</li>';
        } ?>
    </ul>

Voici un exemple avec un bloc `if`, codé « comme il faut » :

    <?php if ($role == 'admin'): ?>
        <span>Admin</span>
    <?php else: ?>
        <span>User</span>
    <?php endif ?>

Voici le même exemple mais codé « pas comme il faut » :

    <?php if ($role == 'admin') { ?>
        <span>Admin</span>
    <?php } else { ?>
        <span>User</span>
    <?php } ?>

### Opérateur ternaire

L'opérateur ternaire permet d'exécuter une séquence du type « Si oui, renvoyer ceci, sinon renvoyer cela ».
Cette séquence peut être écrite avec un bloc `if` et `else` mais l'opérateur ternaire peut être utilisé sur une seule ligne, ce qui est pratique.

Exemple avec un bloc `if` :

    if ($erreur) {
        $message = 'il y a une erreur';
    } else {
        $message = 'tout va bien';
    }

Même exemple avec l'opérateur ternaire :

    $message = $erreur ? 'il y a une erreur' : 'tout va bien';

L'opérateur permet aussi une simplification du type « S'il y a une valeur la renvoyer, sinon renvoyer autre chose ».

    $erreur = 'il y a une erreur';
    $message = $erreur ?: 'tout va bien';

## Timeout

La fonction `set_time_limit()` permet d'augmenter la durée maximale de temps d'exécution autorisé pour script.
Ce réglage est défini dans le fichier `php.ini` avec la variable `max_execution_time`.
La durée par défaut est de 30 secondes.

Si je veux qu'un script puisse s'exécuter pendant 50 minutes sans faire de timeout, la durée limite de mon script doit être de `5 * 60 secondes = 300 secondes` :

    set_time_limit(300);

## `isset()` VS `!empty()`

`isset()` veut dire « est défini » en anglais; sert à vérifier que l'utilisateur a envoyé des données.

`!empty()` veut dire « non vide » en anglais; sert à vérifier qu'un champ obligatoire a été renseigné.

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

### Généralités

- [PHP: Hypertext Preprocessor](http://php.net/)
- [PHP: Options - Manual](http://php.net/manual/en/features.commandline.options.php)
- [SitePoint – Learn HTML, CSS, JavaScript, PHP, Ruby & Responsive Design](https://www.sitepoint.com/)

### Best practices

- [PHP-FIG — PHP Framework Interop Group - PHP-FIG](https://www.php-fig.org/)
- [PHP: The Right Way](http://www.phptherightway.com/)
- [PHP Best Practices: a short, practical guide for common and confusing PHP tasks](https://phpbestpractices.org/)

### Code style

- [PSR-1: Basic Coding Standard - PHP-FIG](https://www.php-fig.org/psr/psr-1/)
- [PSR-2: Coding Style Guide - PHP-FIG](https://www.php-fig.org/psr/psr-2/)
- [PSR-0: Autoloading Standard - PHP-FIG](https://www.php-fig.org/psr/psr-0/)
- [PSR-4: Autoloader - PHP-FIG](https://www.php-fig.org/psr/psr-4/)

### Syntaxe

- [PHP: Alternative syntax for control structures - Manual](https://secure.php.net/manual/en/control-structures.alternative-syntax.php)
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

### WTF

- [PHP Sadness](http://phpsadness.com/)

### Sécurité

- [OWASP](https://www.owasp.org/index.php/Main_Page)
- [Category:PHP - OWASP](https://www.owasp.org/index.php/Category:PHP#tab=Main)

