# Doctrine DBAL

## Installation

Dans un terminal, taper :

Dans un terminal, si vous n'êtes pas déjà dans la dossier racine de votre projet, taper :

    cd [dossier du projet web]

Pour installer le paquet :

    composer require doctrine/dbal ~2.0

## Utilisation sans framework

Taper le code suivant dans un nouveau fichier nommé `items.php` :

    <?php

    require __DIR__.'/../vendor/autoload.php';

    $config = new \Doctrine\DBAL\Configuration();

    $connectionParams = array(
        'driver'    => 'pdo_mysql',
        'host'      => '127.0.0.1',
        'dbname'    => 'my_project',
        'user'      => 'user',
        'password'  => '123',
        'charset'   => 'utf8mb4',
    );

    $conn = \Doctrine\DBAL\DriverManager::getConnection($connectionParams, $config);

    $items = $conn->fetchAll('SELECT * FROM item');

    foreach ($items as $item) {
        echo $item['id'].'<br />';
        echo $item['title'].'<br />';
        echo $task['description'].'<br />';
    }

Attention : veiller à bien adapter les code d'accès à la base de données.

## Doc

- [Welcome to Doctrine DBAL’s documentation! — Doctrine DBAL 2 documentation](http://docs.doctrine-project.org/projects/doctrine-dbal/en/latest/)
