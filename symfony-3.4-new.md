# Symfony 3.4 New

Attention : ce cours installe une version de Symfony 3.4 qui a une structure de dossier identique avec celle de Symfony 4.x.

Dans Symfony 4.x, certains dossiers changent de nom et d'emplacement :

- `/app/config` devient `/config`
- `/app/Ressources/views` devient `/templates`
- `/src/AppBundle` devient `/src`
- `/web` devient `/public`

## Install

Attention : l'installation nécessite la nouvelle version (`4.x`) de l'installeur Symfony. Voir [symfony-installer.md](symfony-installer.md).

Installer Symfony 3.4 :

    symfony new --full --version=3.4 my_project

Se rendre dans le dossier du projet :

    cd my_project

Installer le bundle `symfony/web-server-bundle` :

    composer require server --dev

Lancer le serveur web de développement :

    php bin/console server:run

Note : pour stopper le serveur, appuyer sur `CTRL + C`.

Pour tester l'installation, ouvrir l'url suivante dans un navigateur :

    http://localhost:8000/

## Configuration de l'accès à la base de données

Ouvrir le fichier `.env` et modifier le bloc :

    DATABASE_URL=mysql://db_user:db_password@127.0.0.1:3306/db_name

afin d'obtenir :

    DATABASE_URL=mysql://root:123@127.0.0.1:3306/my_project

## Le `APP_SECRET` de `.env`

Si vous voulez regénérer le `APP_SECRET` de votre fichier `.env`, vous pouvez utiliser le script suivant qui génèrera une chaîne de 32 caractères en hexadécimal tirée au hasard :

    <?php
    $bytes = random_bytes(16);
    echo bin2hex($bytes).PHP_EOL;

Sinon vous pouvez utiliser un outil en ligne (qui génère 40 caractères et non 32) : [Symfony 2 Secret Generator - nux.net](http://nux.net/secret).

### Création de la base de données

#### Avec la console

Interrompre le serveur web avec `CTRL + C` si nécessaire.

Dans le terminal :

    php bin/console doctrine:database:create

Relancer le serveur web avec la commande `php bin/console server:run` si nécessaire.

#### Avec PhpMyAdmin

Avec PhpMyAdmin, créer une nouvelle base de données.

Les noms (BDD, tables, colonnes, etc) ne doivent comporter aucun espace ` `, ni tiret `-`, ni point `.`, ni accent.
Il faut donc se limiter aux caractères minuscules de l'alphabet, aux chiffres et au « underscore » (tiret du bas) `_`.

Pour le nom de la BDD, choisissez le même nom que votre projet, en remplaçant les caractères interdits par des caractères autorisés.

L'interclassement de la BDD doit être `utf8mb4_unicode_ci`.

## Langue

Pour configurer la langue, ouvrir le fichier `config/services.yaml`, et modifier le bloc :

    # Put parameters here that don't need to change on each machine where the app is deployed
    # https://symfony.com/doc/current/best_practices/configuration.html#application-related-configuration
    parameters:
        locale: 'en'

afin d'obtenir :

    # Put parameters here that don't need to change on each machine where the app is deployed
    # https://symfony.com/doc/current/best_practices/configuration.html#application-related-configuration
    parameters:
        locale: 'fr'

## Exemple de contrôleur minimaliste

Dans le dossier `src/Controller`, créer le fichier `MinimalController.php` :

    <?php

    namespace App\Controller;

    use Symfony\Bundle\FrameworkBundle\Controller\AbstractController;
    use Symfony\Component\HttpFoundation\Request;
    use Symfony\Component\Routing\Annotation\Route;
    use Symfony\Component\Routing\Annotation\Method;

    /**
     * @Route("/")
     */
    class MinimalController extends AbstractController
    {
        /**
         * @Route("/hello/{name}", name="minimal_hello_name")
         */
        public function helloName(Request $request, $name)
        {
            $greeting = "Hello {$name}!";

            return $this->render('minimal/hello-name.html.twig', [
                'greeting' => $greeting,
            ]);
        }
    }



## Accéder au service `database_connection`

Modifier tous les contrôleurs qui doivent utiliser le service `database_connection`, en ajoutant la ligne de code suivante :

    use Doctrine\DBAL\Connection;

    // ...

        private $conn;

        public function __construct($conn)
        {
            $this->conn = $conn;
        }

Exemple avec notre contrôleur minimaliste :

    <?php
    namespace App\Controller;

    use Doctrine\DBAL\Connection;
    use Symfony\Bundle\FrameworkBundle\Controller\AbstractController;
    use Symfony\Component\HttpFoundation\Request;
    use Symfony\Component\Routing\Annotation\Route;
    use Symfony\Component\Routing\Annotation\Method;

    /**
     * @Route("/")
     */
    class MinimalController extends AbstractController
    {
        private $conn;

        public function __construct($conn)
        {
            $this->conn = $conn;
        }

        /**
         * @Route("/hello/{name}", name="minimal_hello_name")
         */
        public function helloName(Request $request, $name)
        {
            $greeting = "Hello {$name}!";

            return $this->render('minimal/hello-name.html.twig', [
                'greeting' => $greeting,
            ]);
        }

        /**
         * @Route("/items", name="minimal_items")
         */
        public function items(Request $request, $name)
        {
            $sql = '
            SELECT *
            FROM item
            ';

            $items = $this->conn->fetchAll($sql);

            return $this->render('minimal/hello-name.html.twig', [
                'items' => $items,
            ]);
        }
    }

## Migrer des entités, des contrôleurs et des formulaires 2.8 vers 3.4

Dans les entités, remplacer les occurences de :

- `AppBundle` par `App`

Dans les contrôleurs, remplacer les occurences de :

- `AppBundle` par `App`
- `App:` par `App\Entity\`

Dans les formulaires, remplacer les occurences de :

- `AppBundle` par `App`
- `appbundle` par `app`

## Doc

### Install

- [Download Symfony Framework and Components](https://symfony.com/download)
- [Framework Configuration Reference (FrameworkBundle) (Symfony 3.4 Docs)](https://symfony.com/doc/3.4/reference/configuration/framework.html#secret)

### Entity

- [Databases and the Doctrine ORM (Symfony 3.4 Docs)](https://symfony.com/doc/3.4/doctrine.html#persisting-objects-to-the-database)
- [How to Work with Doctrine Associations / Relations (Symfony 3.4 Docs)](https://symfony.com/doc/3.4/doctrine/associations.html)
- [Association Mapping - Object Relational Mapper (ORM) - Doctrine](https://www.doctrine-project.org/projects/doctrine-orm/en/latest/reference/association-mapping.html)
- [Doctrine\Common\Collections\ArrayCollection | API](https://www.doctrine-project.org/api/collections/latest/Doctrine/Common/Collections/ArrayCollection.html)

### GET, POST, FILES, SESSION, COOKIES, SERVER

- [Symfony and HTTP Fundamentals (Symfony 3.4 Docs)](https://symfony.com/doc/3.4/introduction/http_fundamentals.html)
- [The HttpFoundation Component (Symfony 3.4 Docs)](https://symfony.com/doc/3.4/components/http_foundation.html)

### Form

- [Forms (Symfony 3.4 Docs)](https://symfony.com/doc/3.4/forms.html)
- [Form Types Reference (Symfony 3.4 Docs)](http://symfony.com/doc/3.4/reference/forms/types.html)
- [EntityType Field (Symfony 3.4 Docs)](http://symfony.com/doc/3.4/reference/forms/types/entity.html)
- [Validation Constraints Reference (Symfony 3.4 Docs)](http://symfony.com/doc/3.4/reference/constraints.html)

