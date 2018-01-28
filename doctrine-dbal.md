# Doctrine DBAL

Doctrine DBAL est une librairie PHP qui permet de communiquer avec un serveur de base de données (BDD). Cette librairie peut communiquer avec plusieurs types de BDD, dont MySQL.

## Installation

Dans un terminal, si vous n'êtes pas déjà dans la dossier racine de votre projet, taper :

    cd [dossier du projet web]

Pour installer le paquet :

    composer require doctrine/dbal ~2.0

## Utilisation sans framework

Taper le code suivant dans un nouveau fichier :

    <?php

    use Doctrine\DBAL\Configuration;
    use Doctrine\DBAL\DriverManager;

    require __DIR__.'/../vendor/autoload.php';

    $config = new Configuration();
    $connectionParams = array(
        'driver'    => 'pdo_mysql',
        'host'      => '127.0.0.1',
        'dbname'    => 'my_project',
        'user'      => 'user',
        'password'  => '123',
        'charset'   => 'utf8mb4',
    );
    $conn = DriverManager::getConnection($connectionParams, $config);

    // récupération de tous les éléments de la table `item` dans un tableau PHP
    $items = $conn->fetchAll('SELECT * FROM item');

    foreach ($items as $item) {
        echo $item['id'].'<br />';
        echo $item['title'].'<br />';
        echo $task['description'].'<br />';
    }

Attention : veiller à bien adapter les code d'accès à la base de données.

## Doc

- [Welcome to Doctrine DBAL’s documentation! — Doctrine DBAL 2 documentation](http://docs.doctrine-project.org/projects/doctrine-dbal/en/latest/)
