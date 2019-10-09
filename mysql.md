# MySQL

@todo fusionner avec une partie de la doc phpmyadmin
@todo ajouter un lien vers le repo src-mysql
@todo ajouter liens vers vidéos vimeo

Ce cours théorique aborde quelques notions élémentaires à connaître à propos de MySQL.

Pour compléter ce cours, il existe un support pratique : [https://github.com/jibundeyare/src-mysql](https://github.com/jibundeyare/src-mysql).

## MariaDB

MariaDB est officiellement le successeur open source et libre de MySQL (acquis par la société Oracle).
MariaDB a été créé par les principaux développeur de MySQL.
MariaDB remplace MySQL dans plusieurs distibutions Linux.

## Nommage des objets (base de données, tables, colonnes, etc)

Quelques règles à respecter :

- les noms (BDD, tables, colonnes, etc) ne doivent comporter aucun espace ` `, ni tiret `-` (tiret du milieu), ni point `.`, ni accent et être écrit en caractères minuscules.  
  Il faut donc se limiter aux caractères minuscules de l'alphabet, aux chiffres et au « underscore » (tiret du bas) `_`.
- pour le nom de la BDD, choisissez le même nom que votre projet, en remplaçant les caractères interdits par des caractères autorisés.
- nommez les tables au singulier.
- nommez `id` la colonne de clé primaire de vos tables.
- formez le nom des colonnes qui font référence à une clé étrangère avec « le nom de la table étrangère + underscore + nom de la colonne étrangère ».  
  Exemple : si la colonne ciblée est `project.id` (table `project`, colonne `id`), cela donne `project_id`.
- formez le nom des contraintes de clé étrangère avec « fk + underscore + le nom de la table en cours + underscore + le nom de la table étrangère + underscore + nom de la colonne étrangère ».  
  Exemple : si on est dans la table `student` et que la colonne ciblée est `project.id` (table `project`, colonne `id`), cela donne `fk_student_project_id`.

Attention : avec MariaDB la taille maximale pour le nom d'un objet (BDD, table, colonne, etc) est de 64 caractères.

Pour plus d'informations, voir [Identifier Names - MariaDB Knowledge Base](https://mariadb.com/kb/en/library/identifier-names/#maximum-length).

Un dernier conseil :

- nommez les tables qui ont une valeur métier dans la langue de votre client.  
  À moins de travailler sur un projet open source où l'anglais est de mise, cela n'a pas de sens de tout traduire en anglais.  
  Par contre nommez en anglais tout ce qui n'est pas lié au métier (par exemple table `user`, table `config`, etc).

## Interclassement ("collation" en anglais)

L'interclassement de la BDD doit être `utf8mb4_unicode_ci`.

Cette page de stackoverflow explique pourquoi en détail : [mysql - What's the difference between utf8_general_ci and utf8_unicode_ci - Stack Overflow](https://stackoverflow.com/questions/766809/whats-the-difference-between-utf8-general-ci-and-utf8-unicode-ci/766996#766996).

## Backticks (accent grave)

Les backticks permettent d'utiliser des mots clés réservés au langage SQL comme nom de base de données, de table ou de colonne.

Exemple : le mot clé `user` est réservé au langage SQL. Mais en utilisant des backticks, on peut créer une table `user` ou sélectionner des lignes de dedans.

    -- sélection de données de la table `user`
    SELECT *
    FROM `user`

## Les requêtes SQL

### CRUD

Le CRUD désigne les quatre opérations de base qui sont possible avec un objet stocké en BDD :

- Create : créer un objet
- Read : lire les données d'un ou de plusieurs objets
- Update : mettre à jour les données d'un objet
- Delete : supprimer un objet

### Lecture de données

La lecture de données se fait au moyen de l'instruction `SELECT`.
L'instruction `SELECT` attend au minimum deux informations : la ou les colonnes sélectionnées et la table dans laquelle elles se trouvent.
Le symbole `*` étoile veut dire « toutes les colonnes ».
On précise la table avec le mot clé `FROM` suivi du nom de la table.
On peut aussi ajouter une clause `WHERE` qui permet de ne sélectionner que les lignes qui correspondent à certains critères (égalité ou grandeur numérique par exemple).

Quelques exemples :

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

### Création de données

La création de données se fait au moyen de l'instruction `INSERT`.
Cette instruction attend : le nom de la table, le nom des colonnes et les données correspondantes.

Quelques exemples :

    -- insertion d'une seule ligne
    INSERT INTO `user` (`email`, `login`, `newsletter`, `devices`)
    VALUES ('foo@example.com', 'foo', '1', '1');

    -- insertion de plusieurs lignes
    INSERT INTO `user` (`email`, `login`, `newsletter`, `devices`)
    VALUES
    ('foo@example.com', 'foo', '1', '1'),
    ('bar@example.com', 'bar', '1', '5'),
    ('baz@example.com', 'baz', '1', '2');

### Modifications de données

La modification de données se fait au moyen de l'instruction `UPDATE`.
Cette instruction attend au minimum : le nom de la table, un ou des couples de nom de colonne et la donnée correspondante.
On peut aussi ajouter une clause `WHERE` qui permet de ne mettre à jour que les lignes qui correspondent à certains critères (égalité ou grandeur numérique par exemple).

Quelques exemples :

    -- modification d'une seule colonne (la colonne `mail`) pour toutes les lignes
    UPDATE `user` SET `email` = ''

    -- modification d'une seule colonne (la colonne `mail`) d'une seule ligne (celle pour laquelle `id = 53`)
    UPDATE `user` SET `email` = 'foo@example.com' WHERE `id` = 53;

    -- modification de plusieurs colonnes (les colonnes `mail` et `newsletter`) d'une seule ligne (celle pour laquelle `id = 53`)
    UPDATE `user` SET `email` = 'foo@example.com', `newsletter` = 1 WHERE `id` = 53;

    -- modification d'une seule colonne de toutes les lignes
    -- notez qu'il n'y a pas de WHERE
    UPDATE `user` SET `devices` = 1;

    -- modification de plusieurs colonnes pour toutes les lignes
    -- notez qu'il n'y a pas de WHERE
    UPDATE `user` SET `email` = 'foo@example.com', `newsletter` = 1;

    -- modification de toutes les lignes avec un remplacement de texte
    -- notez qu'il n'y a pas de WHERE
    -- notez l'utulisation de la fonction `REPLACE()`
    UPDATE `user` SET `email` = REPLACE(`email`, 'example.com', 'example.org');

### Suppression de données

La suppression  de données se fait au moyen de l'instruction `DELETE`.
Cette instruction attend au minimum : le nom de la table.
On peut aussi ajouter une clause `WHERE` qui permet de ne supprimer que les lignes qui correspondent à certains critères (égalité ou grandeur numérique par exemple).

Quelques exemples :

    -- suppression d'une seule ligne (celle pour laquelle `id = 53`)
    DELETE FROM `user` WHERE `id` = 53;

    -- suppression de toutes les lignes
    -- notez qu'il n'y a pas de WHERE
    DELETE FROM `user`;

Attention : quand on supprime en SQL, il n'est pas possible de récupérer les données. Toutes perte est définitive !

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

## Des requêtes SQL plus complexes

### Sélection de données avec jointure

Ces requêtes correspondent au Read du CRUD.

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

### Optimisation de requête avec `EXPLAIN`

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

## Vidéos

- [phpmyadmin - création d’une table de jointure on Vimeo](https://vimeo.com/336705858) : Cette vidéo montre comment créer une table de jointure pour les relations de type « many to many » avec phpmyadmin.
- [phpmyadmin - création de table on Vimeo](https://vimeo.com/310984229) : Cette vidéo montre comment créer une table avec PhpMyAdmin.
- [phpmyadmin - création de clé étrangère on Vimeo](https://vimeo.com/310982123) : Cette vidéo montre comment créer une clé étrangère pour les relations de type « many to one » ou « one to many » avec PhpMyAdmin.
- [phpmyadmin - création de bdd on Vimeo](https://vimeo.com/309776733) : Cette vidéo montre comment créer une base de données avec PhpMyAdmin.
- [phpmyadmin - suppression de bdd on Vimeo](https://vimeo.com/309777482) : Cette vidéo montre comment supprimer une base de données avec PhpMyAdmin.

## Doc

### Généralités

- [Identifier Names - MariaDB Knowledge Base](https://mariadb.com/kb/en/library/identifier-names/#maximum-length)

### Tutoriels

- [Cours et Tutoriels sur le Langage SQL](https://sql.sh/)
- [SQLZOO](https://zh.sqlzoo.net/)
- [Learn - MariaDB.org](https://mariadb.org/learn/)
- [mysql - What's the difference between utf8_general_ci and utf8_unicode_ci - Stack Overflow](https://stackoverflow.com/questions/766809/whats-the-difference-between-utf8-general-ci-and-utf8-unicode-ci/766996#766996)
- [A Visual Explanation of SQL Joins](https://blog.codinghorror.com/a-visual-explanation-of-sql-joins/)

### Générateurs de données de test

- [Mockaroo - Random Data Generator and API Mocking Tool | JSON / CSV / SQL / Excel](https://mockaroo.com/)
- [generatedata.com](http://www.generatedata.com/)
- [Dummy data for MYSQL database](http://filldb.info/)

