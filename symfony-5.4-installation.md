# Symfony 5.4 - Installation

## Mise en place

### Installation de `symfony-cli` (à faire une seule fois par poste)

L'outil en ligne de commande `symfony-cli` permet de créer un nouveau projet ou de lancer un serveur web de développement.

Installation de `symfony-cli` avec Debian ou Ubuntu :

```bash
$ curl -1sLf 'https://dl.cloudsmith.io/public/symfony/stable/setup.deb.sh' | sudo -E bash
$ sudo apt install symfony-cli
```

Si vous avez une autre ditrib linux, vérifiez les instructions sur la page officielle : [Download Symfony Framework and Components](https://symfony.com/download).

### Activation du protocole HTTPS (à faire une seule fois par poste)

Pour pouvoir utiliser le protocole HTTPS dans le navigateur avec notre projet il faut d'abord installer des certificats SSL auto-signés.

Installation du package `libnss3-tools` (seulement avec Debian ou Ubuntu) :

```bash
$ sudo apt install libnss3-tools
```

Installation de certificats auto-signés :

```bash
$ symfony server:ca:install
```

## Création de la BDD (optionnel)

Création d'une BDD nommée `src_symfony_5_4` avec les install scripts :

```bash
$ cd ~/install-scripts
$ ./mkdb.sh src_symfony_5_4
```

Si vous ne voulez pas utilisez les install scripts, vous pouvez créer la BDD avec PhpMyAdmin ou même avec mariadb directement dans le terminal.

Dernière possibilité, si votre compte maraidb du projet a suffisamment de privilèges, vous pourrez créer la BDD plus tard avec la commande `php bin/console do:da:cr` (voir plus bas).

## Création du dossier du projet

Création du projet :

```bash
$ symfony new --webapp --version=lts src-symfony-5.4
* Creating a new Symfony 5.4 project with Composer
  (running /usr/local/bin/composer create-project symfony/website-skeleton /home/foo/projects/src-symfony-5.4 5.4.* --no-interaction)

* Setting up the project under Git version control
  (running git init /home/foo/projects/src-symfony-5.4)


 [OK] Your project is now ready in /home/foo/projects/src-symfony-5.4

```

## Configuration du projet

Vérification de la version de mariadb :

```bash
$ mariadb --version
mariadb  Ver 15.1 Distrib 10.8.3-MariaDB, for Linux (x86_64) using readline 5.1
```

Creation du fichier config local :

```bash
$ cd src-symfony-5.4/
$ touch .env.local
$ code .env.local
```

Exemple de code d'accès mariadb :

- utilisateur : `src_symfony_5_4`
- mot de passe : `123`
- base de données : `src_symfony_5_4`
- serveur : `127.0.0.1`
- port : `3306`

Configuration de l'accès à la BDD dans le fichier `.env.local` :

```
APP_ENV=dev
DATABASE_URL="mysql://src_symfony_5_4:123@127.0.0.1:3306/src_symfony_5_4?serverVersion=mariadb-10.8.3&charset=utf8mb4"
```

Création de la BDD (seulement si ça n'a pas déjà été fait) :

```bash
$ php bin/console do:da:cr
Created database `src_symfony_5_4` for connection named default
```

Vérification de l'accès à la BDD :

```bash
$ php bin/console do:sc:va

Mapping
-------


 [OK] The mapping files are correct.


Database
--------


 [OK] The database schema is in sync with the mapping files.


```

Configuration de la langue dans le fichier `config/packages/translation.yaml` :

```diff-yaml
  framework:
-     default_locale: en
+     default_locale: fr
      translator:
          default_path: '%kernel.project_dir%/translations'
          fallbacks:
              - en
```

## Installation de packages supplémentaires

Installation de `doctrine/fixtures-bundle` :

```bash
$ composer require orm-fixtures --dev
```

Installation de `fakerphp/faker` :

```bash
$ composer require fakerphp/faker --dev
```

Installation de `javiereguiluz/easyslugger` :

```bash
$ composer require javiereguiluz/easyslugger --dev
```

Installation de `knplabs/knp-paginator-bundle` :

```bash
$ composer require knplabs/knp-paginator-bundle
```

## Préparation de fixtures de test

Création de fixtures de test :

```bash
$ php bin/console make:fixtures

 The class name of the fixtures to create (e.g. AppFixtures):
 > TestFixtures

 created: src/DataFixtures/TestFixtures.php


  Success!


 Next: Open your new fixtures class and start customizing it.
 Load your fixtures by running: php bin/console doctrine:fixtures:load
 Docs: https://symfony.com/doc/current/bundles/DoctrineFixturesBundle/index.html
```

Instanciation de doctrine et de faker dans le fichier de fixtures de test :

```diff-php
  namespace App\DataFixtures;

  use Doctrine\Bundle\FixturesBundle\Fixture;
+ use Doctrine\Persistence\ManagerRegistry;
  use Doctrine\Persistence\ObjectManager;
+ use Faker\Factory as FakerFactory;
+ use Faker\Generator as FakerGenerator;
+ use Symfony\Component\PasswordHasher\Hasher\UserPasswordHasherInterface;

  class AppFixtures extends Fixture
  {
+     private $doctrine;
+     private $faker;
+     private $hasher;
+
+     public function __construct(ManagerRegistry $doctrine, UserPasswordHasherInterface $hasher)
+     {
+         $this->doctrine = $doctrine;
+         $this->faker = FakerFactory::create('fr_FR');
+         $this->hasher = $hasher;
+
+     }
+
      public function load(ObjectManager $manager): void
      {
-         // $product = new Product();
-         // $manager->persist($product);
-
          $manager->flush();
      }
  }
```

Si vous voulez lire des données de la BDD, vous pouvez récupérer un repository avec l'instruction `$this->doctrine->getRepository(Foo::class)`.
N'oubliez d'ajouter pas un `use App\Entity\Foo;` au début de fichier et adapatez le nom de classe à la place de `Foo`.

Création du script `bin/dofilo.sh` :

```bash
#!/bin/bash

php bin/console doctrine:database:drop --force --if-exists
php bin/console doctrine:database:create --no-interaction
php bin/console doctrine:migrations:migrate --no-interaction
php bin/console doctrine:fixtures:load --no-interaction
```

Rendre le script exécutable :

```bash
$ chmod +x bin/dofilo.sh
```

Ce script fait les choses suivantes :

- suppression de la BDD (attention, à ne pas faire sur la prod !)
- création de la BDD
- création du schéma de la BDD (tables, colonnes, clé primaires, etc)
- injection des données de test dans la BDD

Note : si ce script fonctionne sans aucune erreur, votre schéma de BDD est prêt à être déployé sur une nouvelle prod.

## Démarrage du serveur web

Lancement du serveur web de développement :

```bash
$ symfony serve
```

Ouvrez le lien suivant avec votre navigateur : [https://127.0.0.1:8000](https://127.0.0.1:8000)

Si vous voyez une page avec le texte « Welcome to Symfony 5.4.10 » (ou une version plus récente), c'est que ça fonctionne, bravo !

