# Symfony YAML

Le package `symfony/yaml` permet de lire un fichier YAML et de récupérer ses données sour la forme d'un tableau (array) PHP.

Pour en savoir plus sur le langage YAML, voir [yaml.md](yaml.md).

## Installation

    composer require symfony/yaml ~3

## Utilisation

Ce package est idéal pour stocker des identifiants de connexion dans un fichier YAML.

Ceci permet notamment d'éviter de stocker des informations secrètes dans du code source que l'on pourrait publier sur github.

### Avec la version `3.4.x` et plus

Pour charger le fichier `parameters.yml` se trouvant dans le dossier `config` de la racine du projet, ajouter dans le fichier PHP se trouvant dans le dossier `public` :

    use Symfony\Component\Yaml\Yaml;

    $parameters = Yaml::parseFile(__DIR__.'/../config/parameters.yml');

### Avec la version `3.0.0`

Pour charger le fichier `parameters.yml` se trouvant dans le dossier `config` de la racine du projet, ajouter dans le fichier PHP se trouvant dans le dossier `public` :

    use Symfony\Component\Yaml\Yaml;

    $parameters = Yaml::parse(file_get_contents(__DIR__.'/../config/parameters.yml'));

### Exemple d'utilisation avec Doctrine DBAL

Pour en savoir plus sur Doctrine DBAL, voir [doctrine-dbal.md](doctrine-dbal.md).

Par exemple, vous pouvez créer un fichier `parameters.yml` dans le dossier `config` pour stocker les paramètres de connexion à votre base de données :

    driver: pdo_mysql
    host: 127.0.0.1
    port: 3306
    dbname: my_project
    user: root
    password: ~
    charset: utf8mb4

Attention : le fichier `parameters.yml` ne doit pas être commité avec git. Il doit figurer dans votre fichier `.gitignore`.

Dans ce cas, il est d'usage de créer un fichier `parameters.yml.dist` dans le dossier `config` qui pourra être commité avec git, distribué et servira de gabarit pour créer un nouveau fichier `parameters.yml` :

    driver: pdo_mysql
    host: 127.0.0.1
    port: 3306
    dbname: ~
    user: ~
    password: ~
    charset: utf8mb4

## Doc

- [The Yaml Component (Symfony 3.4 Docs)](https://symfony.com/doc/3.4/components/yaml.html)
- [The Yaml Component (Symfony 3.0 Docs)](https://symfony.com/doc/3.0/components/yaml.html)
