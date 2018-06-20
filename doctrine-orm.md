# Doctrine ORM

## Les méthodes

- `getResult()` renvoit un tableau d'objets. Renvoit un tableau vide si aucun objet n'est trouvé.
- `getSingleResult()` renvoit un objet. Si aucun objet ou plusieurs objets sont trouvés, une exception est levée.
- `getOneOrNullResult()` renvoit un objet ou la valeur `null` si aucun objet n'est trouvé. Si plusieurs objets sont trouvés, une exception est levée.

- `getScalarResult()` renvoit un tableau de valeurs scalaires (comme des `int`, des `float` ou des `string` par exemple). Renvoit un tableau vide si aucun objet n'est trouvé.
- `getSingleScalarResult()` renvoit une valeur scalaire. Si aucune valeur ou plusieurs valeurs sont trouvées, une exception est levée.

## Doc

- [Welcome to Doctrine 2 ORM's documentation! - Object Relational Mapper (ORM) - Doctrine](https://www.doctrine-project.org/projects/doctrine-orm/en/2.6/index.html)

