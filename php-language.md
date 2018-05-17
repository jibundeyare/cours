# PHP language

## Notions

- variables (types de données simples, tableaux, objets)
- structures de contrôle (blocs de type `if`, `else`, `else if` et `switch`)
- boucles (blocs de type `foreach`, `while`, `do while` et `for`)
- fonctions
- classes
- interfaces
- mixins

## Types de données

- chaîne de caractères (string)
- nombre entier (integer)
- nombre à virgule flottant (float)
- booléen (boolean)
- tableau (array)
- objet (object)

## Programmation orientée objet (POO)

Une classe est la définition d'un objet.

Un objet est une instance de classe.

Analogie : on trouve la définition du mot voiture dans le dictionnaire, c'est l'équivalent d'une classe. On trouve de vraies voitures dans la rue, ce sont l'équivalent d'objets (des instances de la définition).

En POO :

- une variable se nomme « attribut » ou « membre »
- une fonction se nomme « méthode »

## Syntaxe

### Bloc PHP

Ne pas fermer le bloc PHP avec `?>` si la page ne contient que du code PHP.

`<?=` et `<?php echo ` sont équivalent

### Signe égal

`=` est impératif (c'est un ordre).

`==` est interrogatif (c'est une question).

### Inclusion

`include` et `require` font la même chose (un copier-coller dynamique de fichier) mais `require` a l'avantage de stopper le programme si le fichier n'est pas trouvé.

### Tableaux

`array()` est l'ancienne notation qui permet de créer un tableau. `[]` est la nouvelle notation (recommandée) qui permet de créer un tableau.

### Intégration de code PHP dans du code HTML

Pour intégrer du code PHP dans du code HTML, il est recommandé d'utiliser une syntaxe alternative dans tous les blocs de type structure de contrôle ou boucle.

Voici un exemple avec une boucle `foreach` :

    <ul>
        <?php foreach ($rows as $row): ?>
        <li><?= $row ?></li>
        <?php endforeach ?>
    </ul>

Voici le même exemple mais codé d'une façon non recommandable :

    <ul>
        <?php foreach ($rows as $row) {
            echo '<li>'.$row.'</li>';
        } ?>
    </ul>

Voici un exemple avec un bloc `if` :

    <?php if ($role == 'admin'): ?>
        <span>Admin</span>
    <?php else: ?>
        <span>User</span>
    <?php endif ?>

Voici le même exemple mais codé d'une façon non recommandable :

    <?php if ($role == 'admin') { ?>
        <span>Admin</span>
    <?php } else { ?>
        <span>User</span>
    <?php } ?>

### Opérateur ternaire

L'opérateur ternaire permet d'exécuter une séquence du type « Si oui, affecter ceci, sinon affecter cela ». Cette séquence peut être écrite avec un bloc `if` et `else` mais l'opérateur ternaire peut être utilisé sur une seule ligne, ce qui est pratique.

Exemple avec un bloc `if` :

    if ($erreur) {
        $message = 'il y a une erreur';
    } else {
        $message = 'tout va bien';
    }

Même exemple avec l'opérateur ternaire :

    $message = $erreur ? 'il y a une erreur' : 'tout va bien';

## Timeout

La fonction `set_time_limit()` permet d'augmenter la durée maximale de temps d'exécution autorisé pour script.
Ce réglage est défini dans le fichier `php.ini` avec la variable `max_execution_time`.
La durée par défaut est de 30 secondes.

Si je veux qu'un script puisse s'exécuter pendant 50 minutes sans faire de timeout, la durée limite de mon script doit être de `5 * 60 secondes = 300 secondes` :

    set_time_limit(300);

## `isset()` VS `!empty()`

`isset()` sert à vérifier que l'utilisateur a envoyé des données.
`!empty()` sert à vérifier qu'un champ obligatoire a bien été renseigné.

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
