# Symfony 4.4 - Doctrine extensions

Le package `doctrine-extensions` permet de rajouter des fonctionnalités courantes mais embêtantes à implémnter soi-même.

Pourquoi réinventer la roue quand un simple `composer require` peut faire l'affaire ?

## Les fonctionnalités

Le package `doctrine-extensions` propose les fonctionnélités suivantes :

- l'horodatage
- la suppression non destructive
- la triabilité
- etc

Pour une liste comploète, voyez la doc officielle : [doctrine-extensions/DoctrineExtensions: Doctrine2 behavioral extensions, Translatable, Sluggable, Tree-NestedSet, Timestampable, Loggable, Sortable](https://github.com/doctrine-extensions/DoctrineExtensions).

## Installation

Il y a un package pour Symfony :

```bash
composer require stof/doctrine-extensions-bundle
```

## L'horodatage

Nous allons utiliser la fonctionnalité des Traits (aussi appelés mixins dans les autres languages) pour gagner du temps.

Dans l'exemple ci-dessous, je vais modifier l'entité `Foo` pour la rendre horodatable :

```diff-php
  use App\Repository\FooRepository;
  use Doctrine\ORM\Mapping as ORM;
+ use Gedmo\Timestampable\Traits\TimestampableEntity;

  /**
   * @ORM\Entity(repositoryClass=FooRepository::class)
   */
  class Foo
  {
+     use TimestampableEntity;
      // ...
```

Maintenant il faut activer l'horodatage dans la configuration.

Ouvrez le fichier `config/packages/stof_doctrine_extensions.yaml` et ajoutez l'activation de l'horodatage :

```diff-yaml
  stof_doctrine_extensions:
      default_locale: fr_FR
+     orm:
+         default:
+             timestampable: true

```

Nous avons modifié une entité.
Donc réflexe : n'oubliez pas de génerer un fichier de migration et de mettre à jour la BDD.

```bash
php bin/console doctrine:migrations:diff
php bin/console doctrine:migrations:migrate
```

Et le tour est joué.
Vous devriez voir apparaître dans la BDD deux colonnes supplémentaires qui enregistrent la date de création et la date de modification.
Elles seront remplies sans rien faire.
Pratique !

## La suppression non destructive

La suppression non destructive consiste à marquer dans la BDD un élément pour signifier qu'il a été supprimé... tout en le conservant dans la BDD.
L'idée est de faire disparaître de la vue de l'utilisateur les éléments effacés tout en s'assurant que rien ne disparaît vraiment de la BDD.

Comme pour l'horodatage, nous allons utiliser la fonctionnalité des Traits pour gagner du temps.

Modifiez l'entité `Foo` pour la rendre supprimable de façon non destructive :

```diff-php
  use App\Repository\FooRepository;
  use Doctrine\ORM\Mapping as ORM;
+ use Gedmo\Mapping\Annotation as Gedmo;
+ use Gedmo\SoftDeleteable\Traits\SoftDeleteableEntity;

  /**
+  * @Gedmo\SoftDeleteable(fieldName="deletedAt", timeAware=false, hardDelete=false)
   * @ORM\Entity(repositoryClass=FooRepository::class)
   */
  class Foo
  {
+     use SoftDeleteableEntity;
      // ...
```

Notez que quand on a l'option `hardDelete=true`, une deuxième demande de suppression va réellement supprimer l'objet de la BDD.
Dans notre exemple, j'ai désactivé cette fonctionnalité avec un `hardDelete=false`.
Mais selon les situations, ça peut ne pas être une bonne idée (manque de flexibilité, saturation de l'espace disque).

Maintenant il faut activer la suppression non destructive dans la configuration.

Ouvrez le fichier `config/packages/stof_doctrine_extensions.yaml` et ajoutez l'activation de l'horodatage :

```diff-yaml
  stof_doctrine_extensions:
      default_locale: fr_FR
+     orm:
+         default:
+             softdeleteable: true

```

Comme avec l'horodatage nous avons modifié une entité.
Donc réflexe : n'oubliez pas de génerer un fichier de migration et de mettre à jour la BDD.

```bash
php bin/console doctrine:migrations:diff
php bin/console doctrine:migrations:migrate
```

Vous devriez voir apparaître dans la BDD une colonne supplémentaire qui enregistrent la date de supression.

Une dernière opération.
Il faut préciser à Doctrine ORM que les objets supprimés ne doivent plus être apparants.

Ouvrez le fichier `config/packages/doctrine.yaml` et ajoutez le filtrage :

```diff-yaml
  doctrine:
      # ...
      orm:
          # ...
+         filters:
+             softdeleteable:
+                 class: Gedmo\SoftDeleteable\Filter\SoftDeleteableFilter
+                 enabled: true
```

Maintenant une demande de suppression d'un objet de type `Foo` devrait remplir la colonne contenant un horodatage de la suppression, ce qui aura pour effet de rendre l'objet invisible à Doctrine ORM.

Testez la fonctionnalité avec le code suivant dans un contrôleur :

```php
    /**
     * @Route("/{id}/softdelete", name="foo_softdelete", methods={"GET"})
     */
    public function softdelete(Foo $foo): Response
    {
        $entityManager = $this->getDoctrine()->getManager();
        $entityManager->remove($foo);
        $entityManager->flush();

        return $this->redirectToRoute('foo_index');
    }
```

Si besoin, vous pouvez temporairement désactiver le filtrage des éléments supprimés.
Du coup il réapparaîtront dans des résultats de requête.
Ça peut être pratique pour l'admin peut-être.

```php
// désactivation temporaire du filtre
$entityManager->getFilters()->disable('softdeleteable');

// la variable $foos contient aussi les objets supprimés (de façon non destructive) 
$foos = $fooRepository->findAll();

// réactivation du filtre
$entityManager->getFilters()->enable('softdeleteable');

// la variable $foos ne contient plus que les objets non supprimés 
$foos = $fooRepository->findAll();
```

## Références

- [Installation (Stof DoctrineExtensions Bundle Docs)](https://symfony.com/doc/current/bundles/StofDoctrineExtensionsBundle/installation.html)
- [doctrine-extensions/DoctrineExtensions: Doctrine2 behavioral extensions, Translatable, Sluggable, Tree-NestedSet, Timestampable, Loggable, Sortable](https://github.com/doctrine-extensions/DoctrineExtensions)

L'horodatage :

- [DoctrineExtensions/timestampable.md at main · doctrine-extensions/DoctrineExtensions](https://github.com/doctrine-extensions/DoctrineExtensions/blob/main/doc/timestampable.md)

La suppression non destructive :

- [DoctrineExtensions/softdeleteable.md at main · doctrine-extensions/DoctrineExtensions](https://github.com/doctrine-extensions/DoctrineExtensions/blob/main/doc/softdeleteable.md)
- [StofDoctrineExtensionsBundle/softdeleteable-filter.rst at master · stof/StofDoctrineExtensionsBundle](https://github.com/stof/StofDoctrineExtensionsBundle/blob/master/docs/softdeleteable-filter.rst)


