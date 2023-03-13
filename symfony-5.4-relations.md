# Symfony 5.4 - Les relations

## Le côté possédant / le côté inverse

Quand il y a une relation entre deux entités, on dit qu'il y a un côté possédant et un côté inverse.

La notion de côté possédant et de côté inverse n'est, à la base, pas une notion technique mais plutôt une notion métier.

Par exemple, si vous avez une entité `Batiment` et une entité `Etage`, le côté possédant sera logiquement `Batiment` et le côté inverse `Etage`.
Pourquoi ? Car dans ce cas, un `Batiment` peut exister sans `Etage` mais un `Etage` ne peut exister sans `Batiment`.

Autre exemple : si vous avez une entité `Joueur` et une entité `Equipe`, est-ce le `Joueur` qui possède l'`Equipe` ou le contraire ? Dans ce cas, le choix n'est pas évident. En fait vous pourrez faire le choix que vous voulez car dans la pratique cela ne change rien !

Avec Doctrine, dans le cas d'une relation `OneToMany` ou `ManyToOne`, vous n'aurez pas le choix, le côté possédant sera forcément le côté `many` de la relation.
Dans le cas d'une relation `OneToOne` ou `ManyToMany`, vous pourrez librement choisir le côté possédant.

### Comment repérer le côté possédant / le côté inverse dans le code ?

Il y a un moyen simple de trouver **le côté possédant** d'une relation.
C'est l'entité qui possède la mention `inversedBy` dans l'annotation de la relation.

Pour trouver **le côté inverse** d'une relation, il faut repérer l'entité qui possède la mention `mappedBy` dans l'annotation de la relation.

### Comment repérer le côté possédant / le côté inverse dans la BDD ?

C'est la table qui contient la colonne de clé étrangère qui est **le côté possédant**.
Et logiquement la table qui ne contient **pas** la colonne de clé étrangère est **le côté inverse**.

## Références

- [https://symfony.com/doc/current/doctrine/associations.html](https://symfony.com/doc/current/doctrine/associations.html)
- [https://www.doctrine-project.org/projects/doctrine-orm/en/latest/reference/unitofwork-associations.html](https://www.doctrine-project.org/projects/doctrine-orm/en/latest/reference/unitofwork-associations.html)

