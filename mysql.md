# MySQL

## Conseils

Les noms (BDD, tables, colonnes, etc) ne doivent comporter aucun espace ` `, ni tiret `-` (tiret du milieu), ni accent. Il est donc préférable de se limiter aux caractères de l'alphabet, aux chiffres et au « underscore » (tiret du bas) `_`.

Pour le nom de la BDD, choisissez le même nom que votre projet.

L'interclassement de la BDD doit être `utf8mb4_unicode_ci`.

Nommez les tables au singulier.

Nommez les tables qui ont une valeur métier dans la langue de votre commanditaire.
À moins de travailler sur un projet open source où l'anglais est de mise, cela n'a pas de sens de tout traduire en anglais.
Par contre nommer en anglais tout ce qui n'est pas métier (par exemple table `user`, table `config`, etc).

Nommez `id` la colonne de l''identifiant primaire de vos tables.

Formez le nom des colonnes qui font référence à une clé étrangère avec « le nom de la table étrangère + underscore + nom de la colonne étrangère ».
Exemple : si la colonne ciblée est `event.id` (table `event`, colonne `id`), cela donne `event_id`.

## Doc

- [debian - debconf selections for phpmyadmin unattended installation with no webserver installed and no dbconfig-common - Stack Overflow](https://stackoverflow.com/questions/30741573/debconf-selections-for-phpmyadmin-unattended-installation-with-no-webserver-inst)
- [apache2 - How to solve the phpmyadmin not found issue after upgrading php and apache? - Ask Ubuntu](https://askubuntu.com/questions/387062/how-to-solve-the-phpmyadmin-not-found-issue-after-upgrading-php-and-apache)
