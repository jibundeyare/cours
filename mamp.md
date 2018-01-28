# MAMP

## Terminal

Pour se rendre le dossier des projets :

    cd /Applications/MAMP/htdocs

Pour se rendre dans le dossier du projet `my_project` :

    cd /Applications/MAMP/htdocs/my_project

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

Les logs d'erreurs MySQL sont stockés dans :

    /Applications/MAMP/logs/mysql_error_log.err

Les logs d'erreurs PHP sont stockés dans :

    /Applications/MAMP/logs/php_error.log
