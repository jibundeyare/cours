# MySQL

Ce cours théorique aborde quelques notions élémentaires à connaître à propos de MySQL.

Pour compléter ce cours, il existe un support pratique : [https://github.com/jibundeyare/src-mysql](https://github.com/jibundeyare/src-mysql).

## MariaDB

MariaDB est officiellement le successeur open source et libre de MySQL (acquis par la société Oracle).
MariaDB a été créé par les principaux développeur de MySQL.
MariaDB remplace MySQL dans plusieurs distibutions Linux.

## Nommage des objets (base de données, tables, colonnes, etc)

- les noms (BDD, tables, colonnes, etc) ne doivent comporter aucun espace ` `, ni tiret `-` (tiret du milieu), ni accent. Il est donc préférable de se limiter aux caractères de l'alphabet, aux chiffres et au « underscore » (tiret du bas) `_`.
- pour le nom de la BDD, choisissez le même nom de votre projet, en remplaçant les caractères interdits par des caractères autorisés.
- nommez les tables au singulier.
- nommez les tables qui ont une valeur métier dans la langue de votre commanditaire.
À moins de travailler sur un projet open source où l'anglais est de mise, cela n'a pas de sens de tout traduire en anglais. Par contre nommer en anglais tout ce qui n'est pas métier (par exemple table `user`, table `config`, etc).
- nommez `id` la colonne de l'identifiant primaire de vos tables.
- formez le nom des colonnes qui font référence à une clé étrangère avec « le nom de la table étrangère + underscore + nom de la colonne étrangère ». Exemple : si la colonne ciblée est `event.id` (table `event`, colonne `id`), cela donne `event_id`.

## Interclassement ("collation" en anglais)

L'interclassement de la BDD doit être `utf8mb4_unicode_ci`.

## Backticks (accent grave)

Les backticks permettent d'utiliser des mots clés réservés au langage SQL comme nom de base de données, de table ou de colonne.

Exemple : le mot clé `user` est réservé au langage SQL. Mais en utilisant des backticks, on peut créer une table `user` ou sélectionner des lignes de dedans.

    -- sélection de données de la table `user`
    SELECT *
    FROM `user`

## Sélection de données

    -- sélection de toutes les colonnes de toutes les lignes
    SELECT *
    FROM `user`

    -- sélection de plusieurs colonnes de toutes les lignes
    SELECT id, email, login
    FROM `user`

    -- sélection d'une seule colonne de toutes les lignes
    SELECT email
    FROM `user`

    -- sélection de toutes les colonnes d'une seule ligne
    SELECT *
    FROM `user`
    WHERE `id` = 53

    -- sélection de toutes les colonnes de plusieurs lignes
    SELECT *
    FROM `user`
    WHERE `newsletter` = 1

    -- sélection de plusieurs colonnes de plusieurs lignes
    SELECT id, email
    FROM `user`
    WHERE `newsletter` = 1

## Insert de données

    -- insertion d'une seule ligne
    INSERT INTO `user` (`email`, `login`, `newsletter`, `devices`)
    VALUES ('foo@example.com', 'foo', '1', '1');

    -- insertion de plusieurs lignes
    INSERT INTO `user` (`email`, `login`, `newsletter`, `devices`)
    VALUES
    ('foo@example.com', 'foo', '1', '1'),
    ('bar@example.com', 'bar', '1', '5'),
    ('baz@example.com', 'baz', '1', '2');

## Modifications de données

    -- modification d'une seule colonne d'une seule ligne
    UPDATE `user` SET `email` = 'foo@example.com' WHERE `id` = 53;

    -- modification de plusieurs colonnes d'une seule ligne
    UPDATE `user` SET `email` = 'foo@example.com', `newsletter` = 1 WHERE `id` = 53;

    -- modification d'une seule colonne de toutes les lignes
    -- notez qu'il n'y a pas de WHERE
    UPDATE `user` SET `devices` = 1;

    -- modification de plusieurs colonnes de toutes les lignes
    -- notez qu'il n'y a pas de WHERE
    UPDATE `user` SET `email` = 'foo@example.com', `newsletter` = 1;

    -- modification de toutes les lignes avec un remplacement de texte
    -- notez qu'il n'y a pas de WHERE
    UPDATE `user` SET `email` = REPLACE(`email`, 'example.com', 'example.org');

## Suppression de données

    -- suppression d'une seule ligne
    DELETE FROM `user` WHERE `id` = 53;

    -- suppression de toutes les lignes
    -- notez qu'il n'y a pas de WHERE
    DELETE FROM `user`;

## Relations cardinales entre objets

Il existe trois types de relations cardinales entre objets :

- `many to one`
- `one to many`
- `many to many`

### Relation `many to one`

![Diagramme de classe Foo Bar](img/class-diagram-foo-m-1-bar.png)

Un objet `foo` ne peut être rattaché qu'à un seul objet `bar`.
Mais un objet `bar` peut être rattaché à plusieurs objets `foo`.

Exemple : une voiture de fonction ne peut être rattaché qu'à une seule entreprise.
Mais une entreprise peut avoir plusieurs voitures de fonction.

### Relation `one to many`

![Diagramme de classe Bar Foo](img/class-diagram-bar-1-m-foo.png)

C'est la même relation qu'un `one to many` mais du point de vue de l'autre objet.

Exemple : une entreprise peut avoir plusieurs voitures de fonction.
Et une voiture de fonction ne peut être rattaché qu'à une seule entreprise.

### Relation `many to many`

![Diagramme de classe Foo Baz](img/class-diagram-foo-m-m-baz.png)

Un objet `foo` peut être rattaché à plusieurs objets `baz`.
Et un objet `baz` peut être rattaché à plusieurs objets `foo`.

Exemple : une voiture de fonction peut être réservée (successivement) par plusieurs salariés.
Et un salarié peut réserver (successivement) plusieurs voitures de fonction.

## Sélection de données avec jointure

    -- sélection de tous les utilisateurs et leur ville
    -- relation "many to one"
    SELECT *
    FROM `user`
    INNER JOIN `city` ON `user`.`city_id` = `city`.`id`;

    -- sélection d'un seul utilisateur et sa ville
    -- relation "many to one"
    SELECT *
    FROM `user`
    INNER JOIN `city` ON `user`.`city_id` = `city`.`id`
    WHERE `user`.`id` = 53;

    -- sélection de tous les utilisateurs et de leurs groupes
    -- relation "many to many"
    SELECT *
    FROM `user`
    INNER JOIN `group_user` ON `user`.`id` = `group_user`.`user_id`
    INNER JOIN `group` ON `group_user`.`group_id` = `group`.`id`;

    -- sélection d'un seul utilisateur et de ses groupes
    -- relation "many to many"
    SELECT *
    FROM `user`
    INNER JOIN `group_user` ON `user`.`id` = `group_user`.`user_id`
    INNER JOIN `group` ON `group_user`.`group_id` = `group`.`id`
    WHERE `user`.`id` = 53;

## `EXPLAIN`

`EXPLAIN` permet de savoir comment la requête est exécutée par MySQL.
Cela donne des indications sur la performance de la requête.

ATTENTION : dans un `WHERE`, utiliser la colonne `id` est toujours la meilleure option d'un point de vue performance.

    -- requête qui utilise la colonne id
    EXPLAIN
    SELECT *
    FROM `user`
    WHERE `id` = 1;

    -- requête qui n'utilise pas la colonne id
    EXPLAIN
    SELECT *
    FROM `user`
    WHERE email = 'foo@example.com';

## Générateurs de données de test

- [Mockaroo - Random Data Generator and API Mocking Tool | JSON / CSV / SQL / Excel](https://mockaroo.com/)
- [generatedata.com](http://www.generatedata.com/)
- [Dummy data for MYSQL database](http://filldb.info/)

## Doc

- [Cours et Tutoriels sur le Langage SQL](https://sql.sh/)
- [Learn - MariaDB.org](https://mariadb.org/learn/)
- [mysql - What's the difference between utf8_general_ci and utf8_unicode_ci - Stack Overflow](https://stackoverflow.com/questions/766809/whats-the-difference-between-utf8-general-ci-and-utf8-unicode-ci/766996#766996)

