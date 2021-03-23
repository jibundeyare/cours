# Code style

Ces trois documents forment un ensemble :

- [code-sniffer.md](code-sniffer.md)
- [code-style.md](code-style.md)
- [php-cs-fixer.md](php-cs-fixer.md)

Le code style est la façon dont vous présenter du code. Le code style n'est pas destiné aux machines mais à des personnes. Un code style « propre » facilite la collaboration et la productivité.

Pour commencer, `squizlabs/php_codesniffer` est indiqué mais par la suite, pour travailler avec le framework Symfony `friendsofphp/php-cs-fixer` est un meilleur choix.

## Casse typographique

| Code      | Casse                 | Usage                                             |
| ----------| --------------------- | ------------------------------------------------- |
| `myVar`   | camel case            | pour les noms de variables et de fonctions        |
| `MyVar`   | camel case            | pour les noms de classes, interfaces et mixins    |
| `MY_VAR`  | screaming snake case  | pour les constantes                               |
| `my_var`  | snake case            | surtout utilisé en Ruby ou Python                 |
| `my-var`  | kebab case            | utilisé en HTML et CSS                            |

## Indentation

Vous allez être surpris, les règles d'indentation du code sont très simples.

Les choses se compliquent cependant quand vous mélangez du code PHP et du code HTML.

### Code HTML

Au début le code est désindenté (collé tout à gauche).

Mais dès qu'on ouvre une balise HTML, la ligne suivante doit être indentée.
Si d'autres lignes suivent, elles ont la même indentation.

Quand on ferme une balise HTML, la ligne doit être désindentée.

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

Mais dès qu'on ouvre un block (un `if`, une boucle, une fonction, une classe, etc), la ligne suivante doit être indentée.
Si d'autres lignes suivent, elles ont la même indentation.

Quand on ferme le block, la ligne doit être désindentée.
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

Là, ça se complique...
Mais en gros, l'idée c'est que les balises PHP suivent les règles d'indentation des balises HTML.

Au début le code est désindenté (collé tout à gauche).

Mais dès qu'on ouvre une balise HTML ou un block PHP (pas une balise PHP), la ligne suivante doit être indentée.
Si d'autres lignes suivent, elles ont la même indentation.

Quand on ferme une balise HTML ou un block PHP, la ligne doit être désindentée.
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

## Nommage de variable au singulier ou au pluriel

Quand on nomme une variable, on utilise toujours le singulier.
Mais si la variable contient une collection de données du même type (comme avec les tableaux par exemple), on utilise le pluriel.

Exemple de code en JavaScript :

    // variable nommée au singulier
    var profile = {
        'id': 53,
        'username': 'foo'
    };

    // variable nommée au pluriel
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
    $profile = [
        'id' => 53,
        'username' => 'foo'
    ];

    // variable nommée au pluriel
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

## Code SQL

Attention, avec Windows et MacOS, les noms des bases de données (BDD) et des tables ne sont pas sensibles à la casse **alors qu'ils le sont avec Linux.**

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

