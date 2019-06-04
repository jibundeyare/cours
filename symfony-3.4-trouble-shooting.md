# Symfony 3.4 trouble shooting

## Erreur lors de l'affichage de la home

Si votre navigateur affiche cette erreur :

```
Warning: Unknown: failed to open stream: No such file or directory in Unknown on line 0

Fatal error: Unknown: Failed opening required 'C:\wamps64\www\my_project\vendor\symfony\symfony\src\Symfony\Bundle\WebServerBundle/Resources/router.php' (include_path='.;C:\php\pear') in Unknown on line 0
```

ou cette erreur :

```
Warning: require(index.php): failed to open stream: No such file or directory in C:\wamps64\www\my_project\vendor\symfony\symfony\src\Symfony\Bundle\WebServerBundle\Resources\router.php on line 42

Fatal error: require(): Failed opening required 'index.php' (include_path='.;C:\php\pear') in C:\wamps64\www\foobarbaz\vendor\symfony\symfony\src\Symfony\Bundle\WebServerBundle\Resources\router.php on line 42
```

vous devez utiliser une méthode alternative pour démarre votre serveur web de développement.

Lancer le serveur web de développement :

    php -S localhost:8000 -t web

Ouvrir l'url suivante dans un navigateur : [http://localhost:8000/app_dev.php/](http://localhost:8000/app_dev.php/)

## Erreur de connexion à la base de données avec MAMP

MAMP n'utilise pas le port par défaut `3608` mais le port `8889`.

Modifier le fichier `app/config/parameters.yml` :

    database_port: null

afin d'obtenir :

    database_port: 8889

## Erreur `Specified key was too long` (la clé spécifiée est trop longue)

Cette erreur arrive quand on essaye de créer un index ou une contrainte (avec MySQL) sur un champ texte, et qu'on utilise un codage de caractères `utf8mb4` avec le moteur de stockage `InnoDB`.
Les index et les contraintes sont limités à une longueur de `767` octets.
Le champ en question ne fait que `255` caractères de long, mais avec `utf8mb4`, `1 caractère == 4 octets`.
Avec un champ de `255` caractères, nous avons donc un index ou une contrainte qui fait `4 * 255 caractères == 1020 octets`.

    Schema-Tool failed with Error 'An exception occurred while execu
      ting 'CREATE TABLE promotion (id INT AUTO_INCREMENT NOT NULL, na
      me VARCHAR(255) NOT NULL, start_date DATE DEFAULT NULL, end_date
      DATE DEFAULT NULL, UNIQUE INDEX UNIQ_C11D7DD15E237E06 (name), P
      RIMARY KEY(id)) DEFAULT CHARACTER SET utf8mb4 COLLATE utf8mb4_un
      icode_ci ENGINE = InnoDB':

      SQLSTATE[42000]: Syntax error or access violation: 1071 Specifie
      d key was too long; max key length is 767 bytes' while executing
      DDL: CREATE TABLE promotion (id INT AUTO_INCREMENT NOT NULL, na
      me VARCHAR(255) NOT NULL, start_date DATE DEFAULT NULL, end_date
      DATE DEFAULT NULL, UNIQUE INDEX UNIQ_C11D7DD15E237E06 (name), P
      RIMARY KEY(id)) DEFAULT CHARACTER SET utf8mb4 COLLATE utf8mb4_un
      icode_ci ENGINE = InnoDB

Pour corriger ce problème, la solution la plus simple est de réduire la taille du champ (`length`).
La longueur maximum est `767 / 4 == 191.75`, c-à-d, à peu près `190` caractères.

    /**
     * @var string
     *
     * @ORM\Column(name="name", type="string", length=190, nullable=false, unique=true)
     */
    private $name;

- [mysql - #1071 - Specified key was too long; max key length is 767 bytes - Stack Overflow](https://stackoverflow.com/questions/1814532/1071-specified-key-was-too-long-max-key-length-is-767-bytes)
