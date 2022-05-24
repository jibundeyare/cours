# PHP language basic

PHP est un langage informatique conçu pour créer des applications web.

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
- nombre à virgule flottante (float)
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

## Affectation

Une affectation est comme une affirmation.
On affirme que telle variable va contenir telle valeur.

Exemple d'affectation :

    $foo = 123;

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

### Un peu de sucre dans votre PHP ? (syntactic sugar)

#### Opérateur ternaire

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

#### Opérateur combiné (spaceship operator en anglais)

L'opérateur combiné permet de comparer l'ordre de deux valeurs.

    // renvoie -1
    $result = 3 <=> 7;

    // renvoie 0
    $result = 11 <=> 11;

    // renvoie 1
    $result = 19 <=> 5;

Cela fonctionne aussi avec des chaînes de caractères.

    // renvoie -1
    $result = 'aaa' <=> 'bbb';

    // renvoie 0
    $result = 'ppp' <=> 'ppp';

    // renvoie 1
    $result = 'zzz' <=> 'aaa';

#### Opérateur de fusion null (null coalescing operator en anglais)

L'opérateur de fusion null permet d'utiliser une variable en prévoyant une valeur par défaut au cas où elle ne serait pas définie.

Dit autrement ça permet de se prémunir contre des erreurs dues à des variables nulles.

    // $message prend la valeur de $_POST['message'] si la clé 'message' existe
    // sinon $message prend la valeur 'foo'
    $message = $_POST['message'] ?? 'foo';

## Conditions

Une condition est comme une question.
On demande si telle variable contient telle valeur.

Exemple de condition :

    // comparaison d'égalité entre la variable $foo et la valeur 123
    if ($foo == 123) {
        echo 'foo contient la valeur 123';
    }

## Typage strict

En ajoutant la ligne suivante au début de vos fichiers, vous signalez à PHP que les type de données doivent être utilisés de façon stricte.

    declare(strict_types = 1);

Exemple : une fonction qui accepte un paramètre de type `int` n'acceptera pas qu'on lui passe un `float` (mais le contraire sera vrai).
Il faudra explicitement convertir le `float` en `int` avant de transmettre la valeur à la fonction.

## Type hinting

Pour en savoir plus, voir la documentation (mais elle est vraiment pas terrible) : [PHP: Type declarations - Manual](https://www.php.net/manual/en/language.types.declarations.php).

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

## Les variables super globales

Les variables super globales sont appelées comme ça car elles sont accessible où que vous soyez dans votre code (dans un `if`, une fonction ou une classe ou autre).

Voici les variables super globales les plus utilisées :

- `$_GET`
- `$_POST`
- `$_SESSION`
- `$_FILES`
- `$_COOKIE`

Toutes les variables super globales sont des tableaux à clé / index alpha-numérique.
Vous pouvez donc utiliser la syntaxe habituelle.

Quelque soit la variable, il est à chaque fois possible d'effectuer les opérations suivantes :

- vérifier si le tableau contient des données avec un simple `if`
- vérifier si une clé existe avec la fonction `isset()`
- vérifier si une valeur associée à une clé est une vide avec la fonction `empty()`

Comme on ne peut pas toujours savoir à l'avance si l'utilisateur nous envoie des données, on doit d'abord tester la présence de données avant d'essayer de les utiliser.
Dans un contexte booléen, un tableau vide équivaut à `false` et un tableau contenant des données équivaut à `true`.
En exploitant cette propriété, on peut utiliser un simple `if` pour voir si notre tableau contient des données :

    if ($_POST) {
        // il y a des données dans la variable $_GET
    }

Pour tester la présence d'une clé en particulier, il faut utiliser la fonction `isset()` :

    if (isset($_POST['login'])) {
        // la clé login existe
    }

Mais en général, on utilise plutôt la fonction `empty()` qui permet de tester qu'une clé existe et que la valeur associée est non vide :

    if (empty($_POST['login'])) {
        // la clé login n'exoste pas ou la valeur associé est vide
    }

### La variable `$_GET`

Cette variable permet de récupérer des données envoyées par l'utilisateur via la barre d'adresse.

Si vous appelez la page `foo.php` avec les paramètres suivants :

[http://localhost:8000/foo.php?param1=lorem&param2=ipsum](http://localhost:8000/foo.php?param1=lorem&param2=ipsum)

Alors vous pourrez récupérer les valeurs des paramètres `param1` et `param2` avec le code suivant :

    $param1 = $_GET['param1'];
    $param2 = $_GET['param2'];

### La variable `$_POST`

Cette variable permet aussi de récupérer les données envoyées par les utilisateurs.
Mais cette fois-ci, les données envoyées via un formulaire.

Si dans votre code HTML vous avez le formulaire suivant :

    <form action="" method="post>
        <input type="text" name="login"><br>
        <input type="password" name="password"><br>
        <button type="submit">ok</button>
    </form>

Alors vous pourrez récupérer les valeurs des paramètres `login` et `password` avec le code suivant :

    $login = $_POST['login'];
    $password = $_POST['password'];

Notez bien que c'est l'attribut `name` dans le code HTML qui détermine le nom des clés alpha-numérique côté serveur.
Si je change `name="login"` en `name="foo"`, alors côté serveur je devrais faire `$foo = $_POST['foo'];` pour récupérer la valeur associée.

## La variable `$_SESSION`

On l'appelle aussi la variable de session.

Cette variable permet d'associer un espace de stockage (temporaire) dédié à chaque client.
C-à-d que PHP va stocker les données de deux clients (navigateurs web) différents dans deux différents fichiers.

Si c'est le client `A` qui effectue une requête, le code `echo $_SESSION['login'];` ne donnera pas le même résultat que si c'est le client `B` qui exécute la même requête.

Encore autrement dit, c'est un mécanisme qui permet à PHP de reconnaître un client et de stocker des données particulières à ce client.

Dans la pratique, on s'en sert pour protéger des pages web par mot de passe et rejeter les clients non authentifiés.

Pour utiliser la variable de session, il faut d'abord la démarrer, sinon `$_SESSION` reste vide :

Dans le fichier `a.php`, essayez :

    <?php
    // a.php
    if (isset($_SESSION['foo'])) {
        echo $_SESSION['foo'];
    } else {
        echo "la clé 'foo' n'est pas définie";
    }

Dans le fichier `b.php`, essayez :

    <?php
    // b.php
    session_start();
    $_SESSION['foo'] = 123;

Enfin dans le fichier `c.php`, essayez :

    <?php
    // c.php
    session_start();
    if (isset($_SESSION['foo'])) {
        echo $_SESSION['foo'];
    } else {
        echo "la clé 'foo' n'est pas définie";
    }

## La variable `$_FILES`

Cette variable permet de récupérer des fichiers envoyés par l'utilisateur au moyen d'un formulaire.

Le formulaire doit avoir l'attribut `enctype="multipart/form-data"` pour que PHP puisse récupérer les données.
Sinon le fichier ne sera pas envoyé et vous allez galérer pendant plusieurs minutes avant de comprendre et finir pas vous sentir bête.
(Mais c'est pas grave voyons !)

Si vous voulez voir des exemples (en PHP brut, ce qui n'est vraiment plus à faire) regardez ici :

- [cours-php/form-input-file.php at master · jibundeyare/cours-php](https://github.com/jibundeyare/cours-php/blob/master/code/form-input-file.php)
- [cours-php/form-input-file-multi.php at master · jibundeyare/cours-php](https://github.com/jibundeyare/cours-php/blob/master/code/form-input-file-multi.php)

## La variable `$_COOKIE`

Cette variable permet de stocker des données chez le client (dans le système de stockage du navigateur).

Ça permet de stocker un panier de commande ou d'autres données qu'on n'a pas envie de stocker sur le serveur parce qu'elle ne nous serviront plus à rien si l'utilisateur ne revient pas.
Mais qui peuvent être précisuese pour l'utilisateur.

Il faut quand même savoir que le réglage par défaut des cookies fait que les données sont supprimées quand le navigateur est fermé.
À moins de donner une durée de vie au cookie.
Auquel cas, les données restent stockées jusqu'à la date limite.
Mais l'utilisateur peut à tout moment choisir de les effacer.

Pour stocker quelque chosre dans le navigateur (et échanger des données JS par exemple) :

    $_COOKIE['foo'] = 'lorem ipsum';

Pour lire des données stockées dans le cookie (par JS par exemple) :

    $foo = $_COOKIE['foo'];

## Doc

## PHP 8

- [What's new in PHP 8 - stitcher.io](https://stitcher.io/blog/new-in-php-8)

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
- <del>[PSR-2: Coding Style Guide - PHP-FIG](https://www.php-fig.org/psr/psr-2/)</del>
- [PSR-12: Extended Coding Style - PHP-FIG](https://www.php-fig.org/psr/psr-12/)

### Autoloading

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

