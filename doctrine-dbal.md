# Doctrine DBAL

Doctrine DBAL est une librairie PHP qui permet de communiquer avec un serveur de base de données (BDD). Cette librairie peut communiquer avec plusieurs types de BDD, dont MySQL.

## Installation

Dans un terminal, si vous n'êtes pas déjà dans la dossier racine de votre projet, taper :

    cd [dossier du projet web]

Pour installer le paquet :

    composer require doctrine/dbal ~2.0

## Arborescence d'un projet

Pour en savoir plus, voir [php-application.md](php-application.md).

## Stockage des identifiants de connexion

Pour en savoir plus, voir [symfony-yaml.md](symfony-yaml.md).

## Utilisation sans framework

Créez un fichier nommé `hello-doctrine-dbal.php` dans votre dossier `public` :

    <?php
    // public/hello-doctrine-dbal.php

    // déclaration des classes PHP qui seront utilisées
    use Doctrine\DBAL\Configuration;
    use Doctrine\DBAL\DriverManager;

    // activation de la fonction autoloading de Composer
    require_once __DIR__.'/../vendor/autoload.php';

    // création d'une variable avec une configuration par défaut
    $config = new Configuration();

    // création d'un tableau avec les paramètres de connection à la BDD
    $connectionParams = [
        'driver'    => 'pdo_mysql',
        'host'      => '127.0.0.1',
        'port'      => '3306',
        'dbname'    => 'my_project',
        'user'      => 'user',
        'password'  => '123',
        'charset'   => 'utf8mb4',
    ];

    // connection à la BDD
    // la variable `$conn` permet de communiquer avec la BDD
    $conn = DriverManager::getConnection($connectionParams, $config);

    // stockage d'une requête SQL dans une variable
    $sql = 'SELECT * FROM item';

    // envoi d'une requête SQL à la BDD et récupération du résultat sous forme de tableau PHP dans la variable `$items`
    $items = $conn->fetchAll($sql);

    // parcours de chacun des éléments du tableau `$items`
    foreach ($items as $item) {
        // à chaque itération de la boucle, la variable `$item` contient une ligne de la table
        // chaque clé alpha-numérique représente une colonne de la table
        echo $item['id'].'<br />';
        echo $item['name'].'<br />';
        echo $item['description'].'<br />';
        echo '<br />';
    }

Attention : veiller à bien adapter les code d'accès à la base de données.

## Utilisation avec Twig et le package `symfony/yaml`

Créez un fichier nommé `hello-doctrine-dbal.php` dans votre dossier `public` :

    <?php
    // public/hello-doctrine-dbal.php

    // déclaration des classes PHP qui seront utilisées
    use Doctrine\DBAL\Configuration;
    use Doctrine\DBAL\DriverManager;
    use Symfony\Component\Yaml\Yaml;

    // activation de la fonction autoloading de Composer
    require_once __DIR__.'/../vendor/autoload.php';

    // instanciation du chargeur de templates
    $loader = new Twig_Loader_Filesystem(__DIR__.'/../templates');

    // instanciation du moteur de template
    $twig = new Twig_Environment($loader);

    // chargement des coordonnées de connexion à la BDD depuis un fichier YAML
    $connectionParams = Yaml::parseFile(__DIR__.'/../config/parameters.yml');

    // création d'une variable avec une configuration par défaut
    $config = new Configuration();

    // connection à la BDD
    // la variable `$conn` permet de communiquer avec la BDD
    $conn = DriverManager::getConnection($connectionParams, $config);

    // stockage d'une requête SQL dans une variable
    $sql = 'SELECT * FROM item';

    // envoi d'une requête SQL à la BDD et récupération du résultat sous forme de tableau PHP dans la variable `$items`
    $items = $conn->fetchAll($sql);

    // affichage du rendu d'un template
    echo $twig->render('hello-doctrine-dbal.html.twig', [
        // transmission de données au template
        'items' => $items,
    ]);

Créez un fichier nommé `hello-doctrine-dbal.html.twig` dans votre dossier `templates` :

    {# templates/hello-doctrine-dbal.html.twig #}
    <!DOCTYPE html>
    <html lang="en">
    <head>
        <meta charset="UTF-8">
        <title>Document</title>
    </head>
    <body>

        {# parcours de chacun des éléments du tableau `items` #}
        {% for item in items %}
            {# à chaque itération de la boucle, la variable `item` contient une ligne de la table #}
            {# chaque clé alpha-numérique représente une colonne de la table #}
            {{ item.id }}<br />
            {{ item.name }}<br />
            {{ item.description }}<br />
            <br />
        {% endfor %}

    </body>
    </html>

Créez un fichier nommé `parameters.yml` dans votre dossier `config` :

    # config/parameters.yml
    driver: pdo_mysql
    host: 127.0.0.1
    port: 3306
    dbname: my_database
    user: my_user
    password: my_password
    charset: utf8mb4

## Les méthodes

Plusieurs méthodes (comprendre fonctions dans le sens POO) sont utilisables.

Pour lire des données (`SELECT`) : `fetchAll()`, `fetchAssoc()` ou `executeQuery()`.

Pour écrire des données (`INSERT`, `UPDATE` et `DELETE`) : `insert()`, `update()`, `delete()` ou `executeUpdate()`.

Rappel : si ces opérations ne sont pas claires, voir le concept de CRUD dans [mariadb.md](mariadb.md).

## `SELECT` (ou read, le R de CRUD) de plusieurs lignes

### Méthode `fetchAll()`

La méthode `fetchAll()` récupère toutes les données d'un seul coup de la BDD et les stock sous forme de tableau PHP.

Cette méthode est adaptée si :

- les données n'occupent pas trop de place dans la RAM
- la requête ne prend pas de paramètres

    // stockage d'une requête SQL dans une variable
    $sql = 'SELECT * FROM item';

    // envoi d'une requête SQL à la BDD et récupération du résultat sous forme de tableau PHP dans la variable `$items`
    $items = $conn->fetchAll($sql);

    // parcours de chacun des éléments du tableau `$items`
    foreach ($items as $item) {
        // à chaque itération de la boucle, la variable `$item` contient une ligne de la table
        // chaque clé alpha-numérique représente une colonne de la table
        echo $item['id'].'<br />';          // affichage de la colonne `id`
        echo $item['name'].'<br />';        // affichage de la colonne `name`
        echo $item['description'].'<br />'; // affichage de la colonne `description`
        echo '<br />';
    }

### Méthode `executeQuery()`

La méthode `executeQuery` ne récupère les données que ligne par ligne depuis la BDD, du coup l'utilisation de la RAM est minime.

Cette méthode est adaptée si :

- les données occupent beaucoup de place dans la RAM
- la requête prend des paramètres

#### Sans paramètres

    // stockage d'une requête SQL dans une variable
    $sql = 'SELECT * FROM item';

    // crée une requête préparée à partir du code SQL et renvoie un pointeur sur le résultat
    $stmt = $conn->executeQuery($sql);

    // la méthode `rowCount()` permet de savoir combien de lignes le résultat comporte
    echo 'results : '.$stmt->rowCount().'<br />';
    echo '<br />';

    // boucle `while` qui récupère les résultats ligne par ligne
    while ($item = $stmt->fetch()) {
        // à chaque itération de la boucle, la variable `$item` contient une ligne de la table
        // chaque clé alpha-numérique représente une colonne de la table
        echo $item['id'].'<br />';          // affichage de la colonne `id`
        echo $item['name'].'<br />';        // affichage de la colonne `name`
        echo $item['description'].'<br />'; // affichage de la colonne `description`
        echo '<br />';
    }

#### Avec paramètres

    // créer une requête préparée à partir du code SQL et renvoie un pointeur sur le résultat
    $stmt = $conn->executeQuery('SELECT * FROM item WHERE active = :active ORDER BY name :column_order', [
        'active' => true,
        'column_order' => 'desc'
    ]);

    // la méthode `rowCount()` permet de savoir combien de lignes le résultat comporte
    echo 'results : '.$stmt->rowCount().'<br />';
    echo '<br />';

    // boucle `while` qui récupère les résultats ligne par ligne
    while ($item = $stmt->fetch()) {
        // à chaque itération de la boucle, la variable `$item` contient une ligne de la table
        // chaque clé alpha-numérique représente une colonne de la table
        echo $item['id'].'<br />';          // affichage de la colonne `id`
        echo $item['name'].'<br />';        // affichage de la colonne `name`
        echo $item['description'].'<br />'; // affichage de la colonne `description`
        echo '<br />';
    }

## `SELECT` (ou read, le R de CRUD) d'une seule ligne

    // envoi d'une requête SQL à la BDD et récupération du premier résultat sous forme de tableau PHP dans la variable `$item`
    $item = $conn->fetchAssoc('SELECT * FROM item WHERE id = :id', [
        'id' => 123,
    ]);

    if ($item) {
        // affichage des données de chaque colonne
        // chaque clé alpha-numérique représente une colonne de la table
        echo $item['id'].'<br />';          // affichage de la colonne `id`
        echo $item['name'].'<br />';        // affichage de la colonne `name`
        echo $item['description'].'<br />'; // affichage de la colonne `description`
        echo '<br />';
    }

## `INSERT` (ou create, le C de CRUD)

### Méthode `insert()`

Cette méthode est la plus simple pour insérer des données.

    // exécution de la requête et récupération du nombre de lignes affectées dans la variable `$count`
    // requête générée : `INSERT INTO item (name, description) VALUES ('foo', 'bar baz')`
    $count = $conn->insert('item', [
        'name' => 'foo',
        'description' => 'bar baz',
    ]);

    // récupération de l'id de la dernière ligne créée par la BDD dans la variable `$lastInsertId`
    $lastInsertId = $conn->lastInsertId();

    // affichage de nombre de lignes affectées
    echo 'inserted : '.$count.'<br />';
    echo '<br />';

### Méthode `executeUpdate()`

Cette méthode permet :

- d'exécuter des requêtes plus complexes
- de savoir combien de lignes ont été affectées par l'opération

    // exécution de la requête et récupération du nombre de lignes affectées dans la variable `$count`
    $count = $conn->executeUpdate('INSERT INTO item (name, description) VALUES (:name, :description)', [
        'name' => 'foo',
        'description' => 'bar baz',
    ]);

    // récupération de l'id de la dernière ligne créée par la BDD dans la variable `$lastInsertId`
    $lastInsertId = $conn->lastInsertId();

    // affichage de nombre de lignes affectées
    echo 'inserted : '.$count.'<br />';
    echo '<br />';

## `UPDATE` (le U de CRUD)

### Méthode `update()`

Cette méthode est la plus simple pour mettre des données à jour.

    // exécution de la requête et récupération du nombre de lignes affectées dans la variable `$count`
    // requête générée : `UPDATE item SET name = 'foo', description = 'bar baz' WHERE id = 123`
    $count = $conn->update('item', [
        'name' => 'foo',
        'description' => 'bar baz',
    ], ['id' => 123]);

    // affichage de nombre de lignes affectées
    echo 'updated : '.$count.'<br />';
    echo '<br />';

### Méthode `executeUpdate()`

Cette méthode permet :

- d'exécuter des requêtes plus complexes
- de savoir combien de lignes ont été affectées par l'opération

    // exécution de la requête et récupération du nombre de lignes affectées dans la variable `$count`
    $count = $conn->executeUpdate('UPDATE item SET name = :name, description = :description WHERE id = :id', [
        'name' => 'foo',
        'description' => 'bar baz',
        'id' => 123,
    ]);

    // affichage de nombre de lignes affectées
    echo 'updated : '.$count.'<br />';
    echo '<br />';

## `DELETE` (le D de CRUD)

### Méthode `delete()`

Cette méthode est la plus simple pour mettre de données à jour.

    // exécution de la requête et récupération du nombre de lignes affectées dans la variable `$count`
    // requête générée : `DELETE FROM item WHERE id = 123`
    $count = $conn->delete('item', ['id' => 123]);

    // affichage de nombre de lignes affectées
    echo 'updated : '.$count.'<br />';
    echo '<br />';

### Méthode `executeUpdate()`

Cette méthode permet :

- d'exécuter des requêtes plus complexes
- de savoir combien de lignes ont été affectées par l'opération

    // exécution de la requête et récupération du nombre de lignes affectées dans la variable `$count`
    $count = $conn->executeUpdate('DELETE FROM item WHERE id = :id', ['id' => 123]);

    // affichage de nombre de lignes affectées
    echo 'updated : '.$count.'<br />';
    echo '<br />';

## Doc

- [DBAL Documentation - Database Abstraction Layer (DBAL) - Doctrine](https://www.doctrine-project.org/projects/doctrine-dbal/en/latest/)
- [Data Retrieval And Manipulation - Database Abstraction Layer (DBAL) - Doctrine](https://www.doctrine-project.org/projects/doctrine-dbal/en/latest/reference/data-retrieval-and-manipulation.html#data-retrieval-and-manipulation)
- [How to Use Doctrine DBAL (The Symfony CookBook)](https://symfony.com/doc/2.6/cookbook/doctrine/dbal.html)

