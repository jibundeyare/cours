# Symfony 5.4 - L'ORM

L'ORM par défaut de Symfony s'appelle Doctrine ORM.

## Les composants

Il y a trois composants qui permettent à un projet Symfony de d'échanger des données avec une BDD :

- l'entity manager
- les repository
- les entités

### L'entity manager

L'entity manager est comme un « setter » pour la BDD.
Il ne permet pas de lire dans la BDD mais il peut écrire dedans (insérer de nouvelles données, mettre à jour des données existantes, supprimer des données existantes).

Note : on dit d'un objet qui est stocké en BDD qu'il est « persisté ».

### Les repository

Les repository sont comme des « getter » pour la BDD.
Il ne permettent pas d'écrire dans la BDD mais ils permettent de lire dedans.

De plus, nous pouvons y rajouter des méthodes pour créer de nouvelles façon de récupérer des données.

Par exemple :

- récupérer tous les articles dont le titre contient un mot clé
- récupérer tous les articles parus avant ou après une date
- récupérer les articles qui ne sont pas publiés
- récupérer l'auteur d'un article à partir de l'id de l'article

Il est important de noter que certaines méthodes renvoient un tableau d'objets (qui peut être vide si aucun élément n'a été trouvé) alors que d'autres méthodes renvoient un seul objet (ou la valeur `null` si aucun élément n'a été trouvé).

### Les entités

Les entités sont des représentations des objets que l'application va manipuler.
Dans Symfony il s'agit ni plus moins que de classes PHP aggrémentées d'annoations pour Doctrine.

Quand nous récupérons des données de la BDD, ces données sont fournies sous forme d'objets (au sens de la POO).
Les entités permettent de définir quels sont les propriétés et les méthodes ces objets.

