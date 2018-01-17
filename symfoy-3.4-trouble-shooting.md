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

Ouvrir l'url suivante dans un navigateur :

    http://localhost:8000/app_dev.php/

## Erreur de connexion à la base de données avec MAMP

Par MAMP n'utilise pas le port par défaut `3608` mais le port `8889`.

Modifier le fichier `app/config/parameters.yml` :

    database_port: null

pour obtenir :

    database_port: 8889
