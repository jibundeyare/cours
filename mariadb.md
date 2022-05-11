
# MariaDB

Pour information je propose un ensemble de scripts pour faciliter la gestion de virtual hosts et de base de données avec Apache, Mariadb, PHP et PhpMyAdmin sur Debian : [https://github.com/jibundeyare/install-scripts](https://github.com/jibundeyare/install-scripts).

Ce cours théorique aborde quelques notions élémentaires à connaître à propos de MySQL.

Pour compléter ce cours, il existe un support pratique : [https://github.com/jibundeyare/src-mariadb](https://github.com/jibundeyare/src-mariadb).

## Qu'est-ce que MariaDB ?

MariaDB est un service de base de données (BDD).

C'est le successeur officiel, open source et libre de MySQL (qui a été racheté par la société Oracle).
MariaDB a été créé par les développeur principaux de MySQL.
MariaDB remplace MySQL dans la plupart des distibutions Linux.

## Concepts importants

Voici un schéma de la structure d'une BDD :

![mariadb-structure.png](img/mariadb-structure.png)

### Base de données (BDD)

Ça porte à confusion mais on appelle BDD, à la fois le service (c-à-d MariaDB), et le contenant auquel le service donne accès.

Au final, ce qu'il faut comprendre c'est que une BDD n'est rien d'autre qu'un contenant.

Notez qu'on peut stocker plusieurs BDD dans MariaDB.

### Table

Une table est aussi un contenant.

En général il y a plusieurs tables par BDD.

Une table ressemble fortement à un tableau Excel.

### Colonne

Une colonne est aussi un contenant.

Et comme avec les tableaux Excel, une table contient une ou plusieurs colonnes.

Ces colonnes permettent de définir le type de donnée qu'une colonne peut stocker.

Voir la section « Types de donnée » de [algorithmique.md](algorithmique.md) pour avoir une idée des différents types de donnée qui existent.

Pour récapituler, du plus grand au plus petit contenant, on a :

1. le service (MariaDB dans notre cas)
2. les BDD
3. les tables
4. les colonnes

### Index

Une BDD peut contenir beaucoup de données.
Mais rechercher une donnée en particulier, c-à-d rechercher aiguille dans une botte de foin, peut prendre beaucoup de temps.
C'est pourquoi, pour accélérer la recherche, on a inventé les indexes.

L'utilisation d'un index accélère les opérations de recherche, de tri, de jointure ou d'agrégation.

### Contraintes

Les contraintes permettent de :

- rendre obligatoire l'insertion de données dans une colonne (contrainte `NOT NULL`)
- s'assurer qu'il n'y a pas de doublons (contrainte d'unicité, c-à-d donnée unique dans la table)
- s'assurer qu'une donnée entrée dans une colonne est valide (contrainte d'intégrité)

### Clés primaires

Une clé primaire est une colonne dont les données sont indexées (donc rapide à retrouver) et soumises à la contrainte d'unicité (il ne peut y avoir de doublon).
En général cette colonne s'appelle `id`.
Il est recommandé d'utiliser le type de données `unsigned int`.

Cela permet d'utiliser cette colonne pour retrouver rapidement des données tout en s'assurant qu'on ne pourra pas les confondre avec d'autres.

Exemple : si dans une table nommée `users`, j'ai deux homonymes nommés `Bob Dupont`, la clé primaire permettra de savoir à quel `Bob Dupont` on a affaire :

    # table users
    id  nom     prénom
    53  Dupont  Bob
    54  Dupont  Bob

### Clés étrangères

Une clé étrangère est une colonne qui fait référence à la colonne clé primaire d'une autre table.
La clé étrangère n'est pas soumise à la contrainte d'unicité mais par contre et elle est soumise à la contrainte d'intégrité.
Si cette colonne fait référence à la clé primaire de la table `foo`, en général cette colonne s'appelle `foo_id`.

Cela permet d'utiliser cette colonne pour retrouver rapidement des données tout en s'assurant qu'il y aura toujours une données associé dans l'autre table.

On dit que les clés étrangères permettent de faire des jointures.

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

L'interclassement permet de trier correctement les lettre et chiffres dans un ordre croissant ou décroissant.

L'interclassement de la BDD doit être `utf8mb4_unicode_ci`.

Cette page de stackoverflow explique pourquoi en détail : [mysql - What's the difference between utf8_general_ci and utf8_unicode_ci - Stack Overflow](https://stackoverflow.com/questions/766809/whats-the-difference-between-utf8-general-ci-and-utf8-unicode-ci/766996#766996).

## Backticks (accent grave)

Les backticks permettent d'utiliser des mots clés réservés au langage SQL comme nom de base de données, de table ou de colonne.

Exemple : le mot clé `user` est réservé au langage SQL. Mais en utilisant des backticks, on peut créer une table `user` ou sélectionner des lignes de dedans.

    -- sélection de données de la table `user`
    SELECT *
    FROM `user`

## Modélisation ou conception de BDD

### Les méthodes

Pour modéliser votre base de données (c-à-d dessiner son schéma), il existe deux méthodes très connues :

1. MERISE qui est une méthode française très utilisée en France
2. UML qui est une méthode américaine très utilisée dans le reste du monde (dont la France !)

L'idéal est de connaître les deux.

### Les outils

Pour modéliser votre base de données (c-à-d dessiner son schéma), il existe bon nombre d'outils payants ou gratuits.

Voici quelques outils gratuits :

- presque gratuits : du papier et un crayon !
- [phpMyAdmin](https://www.phpmyadmin.net/) en mode « designer »
- [MySQL :: MySQL Workbench](https://www.mysql.com/products/workbench/)
- [GitHub - ondras/wwwsqldesigner: WWW SQL Designer, your online SQL diagramming tool](https://github.com/ondras/wwwsqldesigner)  
  [WWW SQL Designer](https://ondras.zarovi.cz/sql/demo/)

Personnellement, je vous recommande le papier et le crayon.

### Merise

Voici les documents que vous devez produire pour faire une analyse avec la méthode Merise :

1. un dictionnaire des données : une liste complète des données avec le nom des colonnes, le type et la taille des données
2. un modèle conceptuel des données (MCD) : un schéma indiquant le nom des entités, leurs attributs et les relations avec les cardinalités
3. un modèle physique des données (MPD) : un schéma qui reprend le MCD mais qui indique les relations avec des clés étrangères et des tables de jointures
4. un modèle logique des données (MLD) : une liste des tables et de leurs colonnes ; cette liste est prête à être transformée en code SQL

Pour un cours complet, voir :

- [bases de données relationnelles avec MERISE](https://ineumann.developpez.com/tutoriels/merise/initiation-merise/)
- [Analyse des données - Méthode MERISE](https://sqlpro.developpez.com/cours/modelisation/merise/)

### Exemple d'analyse

#### Analyse UML (brouillon) de la BDD Student [https://github.com/jibundeyare/src-mariadb](https://github.com/jibundeyare/src-mariadb) :

![Analyse de la BDD Student](img/mariadb-analyse.excalidraw.png)

#### Analyse Merise

Le dictionnaire de données : [mariadb-merise-dictionnaire-donnees.ods](files/mariadb-merise-dictionnaire-donnees.ods).

Le MCD :

![MCD](img/mariadb-merise-mcd.excalidraw.png)

Le MPD :

![MPD](img/mariadb-merise-mpd.excalidraw.png)

## Relations cardinales entre objets

Il existe quatre types de relations cardinales entre objets :

- `one to one`
- `many to one`
- `one to many`
- `many to many`

Pour déterminer le type de relation cardinale entre deux types d'objets (des `foo` et `bar` par exemple), vous devez vous poser la question suivante pour chaque objet :

> Chaque `foo` peut avoir combien de `bar` ?

> Chaque `bar` peut avoir combien de `foo` ?

Dans les descriptions ci-dessous, je vais prendre l'exemple de voitures pour illustrer le propos.

**Rappelez-vous que la relation cardinale dépend du point vue et essayez de ne pas vous emmêler les pinceaux.**

### Relation `one to one`

![Relation one to one](img/relation-one-to-one.png)

Chaque objet `foo` ne peut être rattaché qu'à un seul objet `bar`.
Et chaque objet `bar` ne peut être rattaché qu'à un seul objet `foo`.

Exemple : chaque conducteur ne peut avoir qu'un seul permis de conduire.
Et chaque permis de condiure ne peut être rattaché qu'à un seul condicteur.

### Relation `many to one`

![Relation many to one](img/relation-many-to-one.png)

C'est la même relation que `one to many` mais du point de vue de l'autre objet.

Exemple : chaque voiture de fonction ne peut être rattaché qu'à une seule entreprise.
Mais chaque entreprise peut avoir plusieurs voitures de fonction.

### Relation `one to many`

![Relation one to many](img/relation-one-to-many.png)

Chaque objet `foo` peut être rattaché à plusieurs objets `bar`.
Mais chaque objet `bar` ne peut être rattaché qu'à un seul objet `foo`.

Exemple : chaque entreprise peut avoir plusieurs voitures de fonction.
Mais chaque voiture de fonction ne peut être rattaché qu'à une seule entreprise.

### Relation `many to many`

![Relation many to many](img/relation-many-to-many.png)

Chaque objet `foo` peut être rattaché à plusieurs objets `baz`.
Et chaque objet `baz` peut être rattaché à plusieurs objets `foo`.

Exemple : chaque voiture de location peut être réservée (successivement) par plusieurs clients.
Et chaque client peut réserver (successivement) plusieurs voitures de location.

## Création ou implémentation d'un schéma de BDD avec PhpMyAdmin

Une fois que vous avez votre schéma, il faut l'implémenter (le créer pour de vrai).

Ces vidéos vous montrent toutes les étapes pour créer une BDD, des tables et les lier entre elles :

- [phpmyadmin - création de bdd on Vimeo](https://vimeo.com/309776733) : Cette vidéo montre comment créer une base de données avec PhpMyAdmin.
- [phpmyadmin - suppression de bdd on Vimeo](https://vimeo.com/309777482) : Cette vidéo montre comment supprimer une base de données avec PhpMyAdmin.
- [phpmyadmin - création de table on Vimeo](https://vimeo.com/310984229) : Cette vidéo montre comment créer une table avec PhpMyAdmin.
- [phpmyadmin - création de clé étrangère on Vimeo](https://vimeo.com/310982123) : Cette vidéo montre comment créer une clé étrangère pour les relations de type « many to one » ou « one to many » avec PhpMyAdmin.
- [phpmyadmin - création d’une table de jointure on Vimeo](https://vimeo.com/336705858) : Cette vidéo montre comment créer une table de jointure pour les relations de type « many to many » avec phpmyadmin.

## Données de test (fixtures)

Quand votre BDD est créé, vous pouvez commencer à ajouter du contenu dedans.

Si vous n'avez pas de données réelles, il existe des outils qui vous permettent de générer des données de test.
Ces données de test s'appellent des fixtures ou des seeds.

Voici quelques outils en ligne qui permettent de générer des données de test (fixtures) :

- [Mockaroo - Random Data Generator and API Mocking Tool | JSON / CSV / SQL / Excel](https://mockaroo.com/)
- [generatedata.com](http://www.generatedata.com/)
- [Dummy data for MYSQL database](http://filldb.info/)

### CRUD

Le CRUD désigne les quatre opérations de base qui permettent de manipuler des données :

- Create : créer un objet (opération en écriture)
- Read : lire les données d'un ou de plusieurs objets (opération en lecture)
- Update : mettre à jour les données d'un objet (opération en écriture)
- Delete : supprimer un objet (opération en écriture)

Certains rajoutent une cinquième opération et parlent de CRUDS :

- Search : rechercher les données d'un ou de plusieurs objets (opération en lecture)

Mais le « search » peut-être assimilié à un « read ».

Chaque opération correspond à un type de requête SQL.

| Type d'opération | Requête SQL |
|------------------|-------------|
| Create           | INSERT      |
| Read ou Search   | SELECT      |
| Update           | UPDATE      |
| Delete           | DELETE      |

## Les requêtes SQL

Les requêtes SQL permettent de manipuler les données.

### Quelques requêtes utiles

Ces requêtes sont un peu l'équivalent de `ls` et `cd` dans un terminal.
Elles permettent de voir la structure d'une BDD.

Pour lister les BDD visibles :

    SHOW DATABASES;

Pour choisir une BDD (example avec une BDD nommée `foo`) :

    USE foo;

Pour lister les tables (vous devez être dans une BDD) :

    SHOW TABLES;

Et enfin pour listes les colonnes d'une table (example avec une table nommée `bar`, et vous devez aussi être dans une BDD) :

    SHOW COLUMNS FROM bar;

### Lecture de données

C'est le Read du CRUD.

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

C'est le Create du CRUD.

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

C'est le Update du CRUD.

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

C'est le Delete du CRUD.

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

## Des requêtes SQL avec jointure

Pour comprendre les requêtes avec jointure, un support visuel peut aider :

- [A Visual Explanation of SQL Joins](https://blog.codinghorror.com/a-visual-explanation-of-sql-joins/)
- [sql rejoint le diagramme de venn](https://webdevdesigner.com/q/sql-joins-as-venn-diagram-94788/)

Vous pouvez aussi voir comment les données sont agglomérées en examinant le fichier excel suivant : [mariadb-jointures.ods](files/mariadb-jointures.ods)

Le diagramme de Venn suivant montre les différentes partie de la jointure :

![Diagramme de Venn](img/mariadb-diagramme-venn.excalidraw.png)

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

## Doc

### Généralités

- [Identifier Names - MariaDB Knowledge Base](https://mariadb.com/kb/en/library/identifier-names/#maximum-length)

### Sécurité

- [Les dangers de MySQL et MariaDB](https://sqlpro.developpez.com/tutoriel/dangers-mysql-mariadb/)

### Tutoriels

- [Cours et Tutoriels sur le Langage SQL](https://sql.sh/)
- [SQLZOO](https://zh.sqlzoo.net/)
- [Learn - MariaDB.org](https://mariadb.org/learn/)
- [mysql - What's the difference between utf8_general_ci and utf8_unicode_ci - Stack Overflow](https://stackoverflow.com/questions/766809/whats-the-difference-between-utf8-general-ci-and-utf8-unicode-ci/766996#766996)

### Mot de passe `root` perdu

- [How To Reset Your MySQL or MariaDB Root Password | DigitalOcean](https://www.digitalocean.com/community/tutorials/how-to-reset-your-mysql-or-mariadb-root-password)

