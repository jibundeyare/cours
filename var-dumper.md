# Var dumper

Var dumper sert à inspecter le contenu d'une variable ou le résultat d'une expression, en phase de développement ou de débogage.

Attention : var dumper ne doit surtout pas être utilisé pour afficher des données aux utilisateurs. Utiliser `echo` ou  `print` pour cela.

## Installation

Dans un terminal, si vous n'êtes pas déjà dans la dossier racine de votre projet, taper :

    cd [dossier du projet web]

Pour installer le paquet :

    composer require symfony/var-dumper ~3.4

## Utilisation

`hello-var-dumper.php` :

    <?php

    require __DIR__.'/vendor/autoload.php';

    $myVar = 'Hello Var Dumper';

    dump($myVar);

## Doc

- [The VarDumper Component (Symfony Docs)](https://symfony.com/doc/current/components/var_dumper.html)
- [PHP: var_dump - Manual](http://php.net/manual/fr/function.var-dump.php)
