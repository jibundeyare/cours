# Doctrine ORM

## Les méthodes

- `getResult()` renvoit un tableau d'objets. Renvoit un tableau vide si aucun objet n'est trouvé.
- `getSingleResult()` renvoit un objet. Si aucun objet ou plusieurs objets sont trouvés, une exception est levée.
- `getOneOrNullResult()` renvoit un objet ou la valeur `null` si aucun objet n'est trouvé. Si plusieurs objets sont trouvés, une exception est levée.

- `getScalarResult()` renvoit un tableau de valeurs scalaires (comme des `int`, des `float` ou des `string` par exemple). Renvoit un tableau vide si aucun objet n'est trouvé.
- `getSingleScalarResult()` renvoit une valeur scalaire. Si aucune valeur ou plusieurs valeurs sont trouvées, une exception est levée.

## Trouble shooting

### L'erreur `Specified key was too long` (la clé spécifiée est trop longue)

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

## Doc

- [Welcome to Doctrine 2 ORM's documentation! - Object Relational Mapper (ORM) - Doctrine](https://www.doctrine-project.org/projects/doctrine-orm/en/2.6/index.html)
