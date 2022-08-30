# Code style

Ces trois documents forment un ensemble :

- [code-sniffer.md](code-sniffer.md)
- [code-style.md](code-style.md)
- [php-cs-fixer.md](php-cs-fixer.md)

Le code style est la façon dont vous présenter du code. Le code style n'est pas destiné aux machines mais à des personnes. Un code style « propre » facilite la collaboration et la productivité.

Pour commencer, `squizlabs/php_codesniffer` est indiqué mais par la suite, pour travailler avec le framework Symfony `friendsofphp/php-cs-fixer` est un meilleur choix.

## La longueur d'une ligne de code

En général, on conseille de ne pas écrire de ligne de code dépassant 80 caractères de long.

La plupart des langages proposent un moyen d'écrire la suite de son code à la ligne suivante sans qu'il ne casse.

En python par exemple, on peut écrire le code assez lisible :

    if foo == 'foo' and \
       bar == 'bar' and \
       baz == 'baz' and \
       lorem == 'lorem' and \
       ipsum == 'ipsum':
        print('foo bar baz lorem ipsum')

au lieu du beaucoup moins lisible :

    if foo == 'foo' and bar == 'bar' and baz == 'baz' and lorem == 'lorem' and ipsum == 'ipsum':
        print('foo bar baz lorem ipsum')

## Casse typographique

La casse désigne la taille d'un caractère :

- capitale (ou majuscule si vous préférez) : grand caractère
- ou minuscule : petit caractère

| Code      | Casse                 | Usage                                             |
| ----------| --------------------- | ------------------------------------------------- |
| `myVar`   | camel case            | pour les noms de variables et de fonctions        |
| `MyVar`   | pascal case           | pour les noms de classes, interfaces et mixins    |
| `MY_VAR`  | screaming snake case  | pour les constantes                               |
| `my_var`  | snake case            | surtout utilisé en Ruby ou Python                 |
| `my-var`  | kebab case            | utilisé en HTML et CSS                            |

## Nommage des constantes

En général les cconstantes sont nommées en majuscules toutes.
Du coup, pour séparer les mots, on utilise l'underscore.
On utilise donc le « screaming snake case ». La classe non ?

Exemple de code valable à la fois en JS et en PHP :

    const LOREM = 123;
    const FOO_BAR_BAZ = 'foo bar baz';

## Nommage des variables

Quelque soit le langage de programmation, il y a des règles (quasi) universelles de nommage de variables.
Les règles suivantes s'appliquent aux noms des variables mais aussi aux noms des fonctions ou des classes.

### Le nom d'une variable ne peut pas commencer par un chiffre

Exemple de code en Python :

    # mauvais
    0_ma_variable = 123

    # ok
    ma_variable_0 = 123
    v0 = 123

### Le nom d'une variable doit commencer par un caractère minuscule

**Mais le nom d'une classe doit commencer par un caractère majuscule.**

*(Mais les dev microsoft ont tendance à violer cette règle.)*

Exemple de code en Python :

    # mauvais
    Ma_variable = 123

    # ok
    ma_variable = 123

### Le nom d'une variable ne peut comporter que des caractères alphanumériques et l'underscore

Les caractères alphanumériques, c-à-d les lettres de `a` à `z` (en majuscule ou minuscule) et les chiffres de `0` à `9`.
L'underscore c'est tiret du bas `_`.

Ça veut dire qu'il faut éviter tous les accents (`éèêë` par exemple), les autres signes diacritiques (`ç` par exemple) et tous les caractères non alphanumériques (`@#§µ` par exemple).

*(Mais en code css vous pouvez vous permettre d'utiliser le trait d'union (tiret du milieu) `-`.*

Exemple de code en Python :

    # mauvais
    ma-variable = 123
    ma variable = 123
    une_probabilité = 0.123

    # ok
    ma_variable = 123
    une_probabilite = 0.123

### Le singulier et le pluriel

Quand on nomme une variable scalaire, on utilise toujours le singulier.
Mais si la variable contient une collection de données (comme avec les listes, les tableaux ou les objets par exemple), on utilise le pluriel.

Exemple de code en JS :

    // variable nommée au singulier
    // la variable représente un seul utilisateur
    var user = {
        'id': 53,
        'username': 'foo'
    };

    // variable nommée au pluriel
    // la variable contient plusieurs utilisateurs
    var users = [
        {
            'id': 53
            'username': 'foo'
        },
        {
            'id': 54
            'username': 'bar'
        }
    ];

Exemple de code en PHP :

    // variable nommée au singulier
    // la variable représente un seul utilisateur
    $user = [
        'id' => 53,
        'username' => 'foo'
    ];

    // variable nommée au pluriel
    // la variable contient plusieurs utilisateurs
    $users = [
        [
            'id': 53
            'username' => 'foo'
        ],
        [
            'id': 54
            'username' => 'bar'
        ]
    ];

## Indentation

L'indentation est le décalage vers la droite d'une ligne de code.

Exemple :

    ligne non indentée (collée à gauche)
        ligne indentée de 1 cran
            ligne indentée de 2 crans
                ligne indentée de 3 crans
            ligne indentée de 2 crans
        ligne indentée de 1 cran
    ligne non indentée (collée à gauche)

Vous allez être surpris car les règles d'indentation du code sont très simples.

Les choses se compliquent cependant quand vous mélangez du code PHP et du code HTML.

### Code HTML

Au début le code est désindenté (collé tout à gauche).

Mais dès qu'on ouvre une balise HTML, la ligne suivante doit être indentée.
Si d'autres lignes suivent, elles ont la même indentation.

Quand on ferme une balise HTML, la ligne doit être désindentée.

Simple non ?

Exemple :

    <div>
        <p>
            lorem ipsum
        </p>
        <p>
            sit amet
        </p>
    </div>

### Code PHP ou JS

Au début le code est désindenté (collé tout à gauche).

Mais dès qu'on ouvre un bloc (un `if`, une boucle, une fonction, une classe, etc), la ligne suivante doit être indentée.
Si d'autres lignes suivent, elles ont la même indentation.

Quand on ferme le bloc, la ligne doit être désindentée.
Si d'autres lignes suivent, elles ont la même indentation.

Exemple :

    function myFunc(string $foo, array $bars): void
    {
        if ($foo) {
            foreach ($bars as $key => $value) {
                echo $value;
            }

            echo $foo;
        }
    }

Exemple :

    class Foo
    {
        $bar = 123;
        $baz = ['lorem', 'ipsum'];

        public function getBar(): int
        {
            if ($this->bar > 100) {
                return $this->bar;
            }

            return 0;
        }

        public function getBaz(): string
        {
            return $this->baz;
        }
    }

### PHP et HTML

Là, ça se complique un peu...

Mais en gros, l'idée c'est que les balises PHP suivent les règles d'indentation des balises HTML.

Au début le code est désindenté (collé tout à gauche).

Mais dès qu'on ouvre une balise HTML ou un bloc PHP (pas une balise PHP), la ligne suivante doit être indentée.
Si d'autres lignes suivent, elles ont la même indentation.

Quand on ferme une balise HTML ou un bloc PHP, la ligne doit être désindentée.
Si d'autres lignes suivent, elles ont la même indentation.

Exemple :

    <div>
        <?php // cette ligne est indentée parce qu'elle suit une balise HTML ouvrante
        if (isset($foo)) :
          ?> <!-- cette ligne est indentée parce qu'elle suit un bloc PHP ouvrant -->
          <p>
              <?= $foo ?>
          </p>
          <?php // cette ligne n'est pas indentée parce qu'elle ne suit pas une balise HTML ouvrante
        endif;
        ?> <!-- cette ligne n'est pas indentée parce qu'elle ne suit pas un bloc PHP ouvrant -->
    </div>

## L'emplacement des accolades `{` et `}` end PHP

Pour savoir ça, il vaut mieux consulter la norme officielle :

- [PSR-1: Basic Coding Standard - PHP-FIG](http://www.php-fig.org/psr/psr-1/)
- <del>[PSR-2: Coding Style Guide - PHP-FIG](https://www.php-fig.org/psr/psr-2/)</del>
- [PSR-12: Extended Coding Style - PHP-FIG](https://www.php-fig.org/psr/psr-12/)

## Code SQL

Attention : quand on utilise MariaDB avec Windows et MacOS, les noms des bases de données (BDD) et des tables ne sont pas sensibles à la casse **alors qu'ils le sont avec Linux.**

- [Identifier Case-sensitivity - MariaDB Knowledge Base](https://mariadb.com/kb/en/identifier-case-sensitivity/)

Prenez donc l'habiutude de nommer toutes vos BDD, tables, colonnes, indexes, fonctions stockées, etc en caractères bas de casse (en minuscules).
Pour le code SQL (instructions, mot clés), tapez le en majuscule.
Ceci vous aidera à distinguer d'un coup d'œil le code SQL et vos données.

Exemple de code SQL bien « stylé » :

    DROP TABLE IF EXISTS `user`;
    CREATE TABLE IF NOT EXISTS `user` (
      `id` int(10) UNSIGNED NOT NULL AUTO_INCREMENT,
      `firstname` varchar(255) COLLATE utf8mb4_unicode_ci NOT NULL,
      `lastname` varchar(255) COLLATE utf8mb4_unicode_ci NOT NULL,
      `email` varchar(255) COLLATE utf8mb4_unicode_ci NOT NULL,
      `creation_date` timestamp NOT NULL DEFAULT CURRENT_TIMESTAMP,
      `modification_date` timestamp NOT NULL DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP,
      `group_id` int(10) UNSIGNED DEFAULT NULL,
      PRIMARY KEY (`id`),
      KEY `fk_user_group_id` (`group_id`),
    ) ENGINE=InnoDB DEFAULT CHARSET=utf8mb4 COLLATE=utf8mb4_unicode_ci;

## Doc

- [PSR-1: Basic Coding Standard - PHP-FIG](http://www.php-fig.org/psr/psr-1/)
- <del>[PSR-2: Coding Style Guide - PHP-FIG](https://www.php-fig.org/psr/psr-2/)</del>
- [PSR-12: Extended Coding Style - PHP-FIG](https://www.php-fig.org/psr/psr-12/)
- [Coding Standards (Symfony Docs)](http://symfony.com/doc/current/contributing/code/standards.html)
- [PHP Coding Standards Fixer](http://cs.sensiolabs.org/)
- [Identifier Case-sensitivity - MariaDB Knowledge Base](https://mariadb.com/kb/en/identifier-case-sensitivity/)

