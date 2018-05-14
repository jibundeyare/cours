# MAMP

## Terminal

Pour se rendre le dossier des projets :

    cd /Applications/MAMP/htdocs

Pour se rendre dans le dossier du projet `my_project` :

    cd /Applications/MAMP/htdocs/my_project

## PHP

### Trouver la version de PHP utilisée

Créer un fichier `phpinfo.php` dans le dossier `/Applications/MAMP/htdocs` avec le contenu suivant :

    <?php phpinfo();

Ouvrir l'url `http://localhost:8888/phpinfo.php`. Le numéro de version de PHP s'affiche.

### Config `php.ini`

Le fichier `php.ini` utilisé est dans :

    /Applications/MAMP/bin/php/phpX.Y.Z/conf/php.ini

Remplacez `X.Y.Z` par la version de PHP utilisée.

## PhpMyAdmin

Le login et le password par défaut sont tout les deux `root`.

## Config MySQL

Les fichiers de config MySQL sont recherchés dans cet ordre :

    /etc/my.cnf
    /etc/mysql/my.cnf
    /Applications/MAMP/conf/my.cnf
    ~/.my.cnf

Si le premier n'est pas trouvé, MAMP cherche le suivant et ainsi de suite.

## Logs

Les logs d'erreurs Apache sont stockés dans :

    /Applications/MAMP/logs/apache_error_log

Les logs d'erreurs MySQL sont stockés dans :

    /Applications/MAMP/logs/mysql_error_log.err

Les logs d'erreurs PHP sont stockés dans :

    /Applications/MAMP/logs/php_error.log
