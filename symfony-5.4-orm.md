# Symfony 5.4 - L'ORM

L'ORM par défaut de Symfony s'appelle Doctrine ORM.

## Les rôle des composants

Il y a trois composants qui permettent à un projet Symfony de d'échanger des données avec une BDD :

- l'entity manager
- les repository
- les entités

### Le rôle de l'entity manager

L'entity manager est comme un « setter » pour la BDD.
Il ne permet pas de lire dans la BDD mais il peut écrire dedans (insérer de nouvelles données, mettre à jour des données existantes, supprimer des données existantes).

Note : on dit d'un objet qui est stocké en BDD qu'il est « persisté ».

### Le rôle des repository

Les repository sont comme des « getter » pour la BDD.
Il ne permettent pas d'écrire dans la BDD mais ils permettent de lire dedans.

De plus, nous pouvons y rajouter des méthodes pour créer de nouvelles façon de récupérer des données.

Par exemple :

- récupérer tous les articles dont le titre contient un mot clé
- récupérer tous les articles parus avant ou après une date
- récupérer les articles qui ne sont pas publiés
- récupérer l'auteur d'un article à partir de l'id de l'article

Il est important de noter que certaines méthodes renvoient un tableau d'objets (qui peut être vide si aucun élément n'a été trouvé) alors que d'autres méthodes renvoient un seul objet (ou la valeur `null` si aucun élément n'a été trouvé).

### Le rôle des entités

Les entités sont des représentations des objets que l'application va manipuler.
Dans Symfony il s'agit ni plus moins que de classes PHP aggrémentées d'annoations pour Doctrine.

Quand nous récupérons des données de la BDD, ces données sont fournies sous forme d'objets (au sens de la POO).
Les entités permettent de définir quels sont les propriétés et les méthodes ces objets.

## Utiliser l'entity manager

### Récupération des instances via la méthode 1

Voici une première méthode pour récupérer une instance de l'entity manager ou du repository de l'entité Foo via une instance de doctrine :

```php
  namespace App\Controller;

+ use App\Entity\Foo;
+ use Doctrine\Persistence\ManagerRegistry;
  use Symfony\Bundle\FrameworkBundle\Controller\AbstractController;
  use Symfony\Component\HttpFoundation\Response;
  use Symfony\Component\Routing\Annotation\Route;

  class FooController extends AbstractController
  {
      #[Route('/foo', name: 'app_foo')]
-     public function index(): Response
+     public function index(ManagerRegistry $doctrine): Response
      {
+         // Récupération de l'entity manager via une instance de doctrine
+         $manager = $doctrine->getManager();
+
+         // Récupération du repository de l'entité Foo via une instance de doctrine
+         $repository = $doctrine->getRepository(Foo::class);
+
+         // les variables $manager et $repository contiennent l'instance de l'entity manager et du repository de l'entité Foo
+
          return $this->render('foo/index.html.twig', [
              'controller_name' => 'FooController',
          ]);
      }
  }
```

### Récupération des instances via la méthode 2

Au lieu de récupérer l'instance de l'entity manager et du repository de l'entité Foo via une instance de doctrine, voici une deuxième méthode qui les récupère via le système de « auto-wiring » de Symfony :

```php
  namespace App\Controller;

+ use App\Repository\FooRepository;
+ use Doctrine\ORM\EntityManagerInterface;
  use Symfony\Bundle\FrameworkBundle\Controller\AbstractController;
  use Symfony\Component\HttpFoundation\Response;
  use Symfony\Component\Routing\Annotation\Route;

  class FooController extends AbstractController
  {
      #[Route('/foo', name: 'app_foo')]
-     public function index(): Response
+     public function foo(EntityManagerInterface $manager, FooRepository $repository): Response
      {
+         // les variables $manager et $repository contiennent l'instance de l'entity manager et du repository de l'entité Foo
+
          return $this->render('foo/index.html.twig', [
              'controller_name' => 'FooController',
          ]);
      }
  }
```

### Utilisation des instances

Création et enregistrement d'un nouvel objet :

```php
// Création d'un nouvel objet
$foo = new Foo();
// Affectation des valeurs du nouvel objet
$foo->setName('foo bar baz');
$foo->setSize(42);

// Ordre d'enregistrement de l'objet
$manager->persist($foo);

// Exécution de l'ordre d'enregistrement
$manager->flush();
```

Modification et enregistrement d'un objet existant :

```php
// Récupération d'un objet à partir de son id
$foo = $repository->find(123);

// Affectation des valeurs de l'objet
$foo->setName('foo bar baz');
$foo->setSize(42);

// L'appel de la fonction $manager->persist() n'est pas nécessaire car doctrine sait déjà que l'objet doit être enregistré en BDD

// Enregistrement de la mise à jour de l'objet
$manager->flush();
```

Suppression d'un objet existant :


```php
// Récupération d'un objet à partir de son id
$foo = $repository->find(123);

// Ordre de suppression de l'objet
$manager->remove($foo);

// Exécution de l'ordre de suppression
$manager->flush();
```

## Utiliser les repository

Voici comment récupérer une instance du repository de l'entité Foo :

```php
  namespace App\Controller;

+ // import de l'entité Foo
+ use App\Entity\Foo;
+ // import de doctrine
+ use Doctrine\Persistence\ManagerRegistry;
  use Symfony\Bundle\FrameworkBundle\Controller\AbstractController;
  use Symfony\Component\HttpFoundation\Response;
  use Symfony\Component\Routing\Annotation\Route;

  class FooController extends AbstractController
  {
      #[Route('/foo', name: 'app_foo')]
-     public function index(): Response
+     public function index(ManagerRegistry $doctrine): Response
      {
+         // Récupération du repository de l'entité Foo via une instance de doctrine
+         $repository = $doctrine->getRepository(Foo::class);
+
          return $this->render('foo/index.html.twig', [
              'controller_name' => 'FooController',
          ]);
      }
  }
```

Récupération de tous les objets :

```php
$foos = $repository->findAll();
```

Récupération d'un objet à partir de son id :

```php
$foo = $repository->find(123);
```

Récupération d'un objet à partir de la valeur d'une propriété :

```php
$foo = $repository->findOneBy(['name' => 'foo bar baz']);
```

Récupération d'un objet à partir de la valeur de plusieurs propriétés :

```php
$foo = $repository->findOneBy([
    'name' => 'foo bar baz',
    'size' => 42,
]);
```

Récupération de plusieurs objets à partir de la valeur d'une propriété, avec tri ascendant par valeur d'une propriété :

```php
$foos = $repository->findBy(
    ['name' => 'foo bar baz'],
    ['size => 'ASC']
);
```

