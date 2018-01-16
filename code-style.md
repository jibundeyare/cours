# Code style

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

## Doc

- [PSR-1: Basic Coding Standard - PHP-FIG](http://www.php-fig.org/psr/psr-1/)
- [PSR-2: Coding Style Guide - PHP-FIG](http://www.php-fig.org/psr/psr-2/)
- [Coding Standards (Symfony Docs)](http://symfony.com/doc/current/contributing/code/standards.html)
- [PHP Coding Standards Fixer](http://cs.sensiolabs.org/)
