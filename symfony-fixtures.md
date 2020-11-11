# Symfony data fixtures

Les data fixtures (ou fixtures tout court) sont des données que l'on charge dans la BDD pour faire des tests ou parce qu'elles sont nécessaires au bon fonctionnement d'une application.

## Fixtures en SQL ou aves des fichiers CSV

Voir la section « Générateurs de données de test » de [mariadb.md](mariadb.md).

## Symfony et les fixtures

Le package `doctrine/doctrine-fixtures-bundle` permet de créer facilement des fixtures.

Le package `fzaninotto/faker` permet de générer des fauses données de façon aléatoire.
Par exemple : des noms de personne, des adresses, des numéros de téléphone, des paragraphes de texte (du type lorem ipsum), etc.

Le package `javiereguiluz/easyslugger` permet de sluggifier, c-à-d de transformer toute chaîne de caractères en chaîne de caractères sans majuscule, sans accent et sans espaces.
Par exemple : `Foo bar baz` devient `foo-bar-baz`.

## Install

    composer require orm-fixtures
    composer require fzaninotto/faker
    composer require javiereguiluz/easyslugger

## Utiliser les fixtures en environnement de prod (optionnel)

**Attention : si vous ne faites pas cette manip, vous ne pourrez charger des fixtures qu'en environnement de dev et de test, pas de prod.**

Dans le fichier `config/bundles.php`, trouvez la ligne suivante :

    Doctrine\Bundle\FixturesBundle\DoctrineFixturesBundle::class => ['dev' => true, 'test' => true],

Pour la remplacer par :

    Doctrine\Bundle\FixturesBundle\DoctrineFixturesBundle::class => ['all' => true],

## Générer automatiquement un gabarit de fixtures

    php bin/console make:fixtures

Cette commande créé un nouveau fichier dans le dossier `src/DataFixtures`.

## Exemple de fixtures

Le fichier suivant permet de générer des comptes d'utilisateurs, dont :

- 1 compte super admin
- 1 compte admin
- 1 student nommé Foo
- 1 student nommé Bar
- 10 students avec un nom et un email générés aléatoirement

Notez que la méthode `getGroups()` étant absente, ces fixtures n'appartiennent à aucune groupe.

Ouvrir le fichier `src/DataFixtures/AppFixtures.php` :

    <?php
    // src/DataFixtures/AppFixtures.php

    namespace App\DataFixtures;

    use App\Entity\Student;
    use App\Entity\User;
    use Doctrine\Bundle\FixturesBundle\Fixture;
    use Doctrine\Bundle\FixturesBundle\FixtureGroupInterface;
    use Doctrine\Common\Persistence\ObjectManager;
    use EasySlugger\Slugger;
    use Symfony\Component\Security\Core\Encoder\UserPasswordEncoderInterface;

    class AppFixtures extends Fixture implements FixtureGroupInterface
    {
        private $encoder;

        public function __construct(UserPasswordEncoderInterface $encoder)
        {
            $this->encoder = $encoder;
        }

        public function load(ObjectManager $manager)
        {
            // créer un utilisateur ayant un rôle super admin
            $user = new User();
            $user->setEmail('supadmin@example.com');
            $user->setRoles(['ROLE_SUPER_ADMIN']);
            // encoder le mot de passe
            $password = $this->encoder->encodePassword($user, '123');
            $user->setPassword($password);
            $manager->persist($user);

            // créer un utilisateur ayant un rôle admin
            $user = new User();
            $user->setEmail('admin@example.com');
            $user->setRoles(['ROLE_ADMIN']);
            // encoder le mot de passe
            $password = $this->encoder->encodePassword($user, '123');
            $user->setPassword($password);
            $manager->persist($user);

            // créer un student
            $student = new Student();
            $student->setFirstname('Foo');
            $student->setLastname('Pop');
            $student->setEmail('foo.pop@example.com');
            $manager->persist($student);

            // créer un autre student
            $student = new Student();
            $student->setFirstname('Bar');
            $student->setLastname('Pop');
            $student->setEmail('bar.pop@example.com');
            $manager->persist($student);

            // créer un générateur de fausses données, localisé pour le français
            $faker = \Faker\Factory::create('fr_FR');

            // créer 10 students
            for ($i = 0; $i < 10; $i++) {
                // générer un prénom et un nom de famille
                $firstname = $faker->firstName;
                $lastname = $faker->lastName;

                // sluggifier le prénom et le nom de famille (enlever les majuscules et les accents)
                // et concaténer avec un nom de domaine de mail généré
                $email = Slugger::slugify($firstname).'.'.Slugger::slugify($lastname).'@'.$faker->safeEmailDomain;

                // créer un student avec les données générées
                $student = new Student();
                $student->setFirstname($firstname);
                $student->setLastname($lastname);
                $student->setEmail($email);
                $manager->persist($student);
            }

            // sauvegarder le tout en BDD
            $manager->flush();
        }
    }

## Gabarits de fixtures

Il est possible, mais pas obligatoire, d'affecter un groupe aux fixtures.
Ceci permet de charger des fixtures en fonctione du contexte : environnement de dev, de test, de prod, etc.

De façon générale, il y a deux types de groupes :

- le groupe `required` qui contient les fixtures nécessaires au bon fonctionnement de l'application
- le groupe `test` qui contient les fixtures nécessaires pour tester l'application

Les fixtures du groupe `required` sont nécessaires dans tous les environnements (dev, test, prod, etc).
Les fixtures du groupe `test` ne sont nécessaires que dans les environnements de dev ou de test.

Si vous voulez créer des fixtures appartennant à ces groupes, vous devez faire attention à la façon dont vous organisez les données.

- les fixtures du groupe `required` doivent contenir toutes les données absoluments nécessaires au bon fonctionnement de l'application (par exemple le compte admin, des options par défaut, etc)
- les fixtures du groupe `test` doivent contenir toutes les données nécessaires aux tests de l'application (par exemple des comptes utilisateurs, des commandes, des factures, etc)

### Fixtures sans groupe spécifique

Ces fixtures seront chargées si vous ne spécifiez pas d'environnement particulier avec l'option `--group`.

    <?php
    // src/DataFixtures/AppFixtures.php

    namespace App\DataFixtures;

    // use App\Entity\Foo;
    use Doctrine\Bundle\FixturesBundle\Fixture;
    use Doctrine\Common\Persistence\ObjectManager;

    class AppFixtures extends Fixture
    {
        public function load(ObjectManager $manager)
        {
            // créer un foo
            // $foo = new Foo();
            // $manager->persist($foo);

            // sauvegarder le tout en BDD
            $manager->flush();
        }
    }

### Fixtures du groupe `required`

Ces fixtures ne seront chargées que si l'option `--group=required` est utilisée.

    <?php
    // src/DataFixtures/RequiredFixtures.php

    namespace App\DataFixtures;

    // use App\Entity\Foo;
    use Doctrine\Bundle\FixturesBundle\Fixture;
    use Doctrine\Bundle\FixturesBundle\FixtureGroupInterface;
    use Doctrine\Common\Persistence\ObjectManager;

    class RequiredFixtures extends Fixture implements FixtureGroupInterface
    {
        public function load(ObjectManager $manager)
        {
            // créer un foo
            // $foo = new Foo();
            // $manager->persist($foo);

            // sauvegarder le tout en BDD
            $manager->flush();
        }

        public static function getGroups(): array
        {
            return ['required'];
        }
    }

### Fixtures du groupe `test`

Ces fixtures ne seront chargées que si l'option `--group=test` est utilisée.

Normalement, les fixtures du groupe `test` nécessitent que les fixtures du `required` soient chargés d'abord.
Ceci est précisé dans la méthode `getDependencies()`.

    <?php
    // src/DataFixtures/TestFixtures.php

    namespace App\DataFixtures;

    use App\DataFixtures\ProdFixtures;
    // use App\Entity\Foo;
    use Doctrine\Bundle\FixturesBundle\Fixture;
    use Doctrine\Bundle\FixturesBundle\FixtureGroupInterface;
    use Doctrine\Common\Persistence\ObjectManager;

    class TestFixtures extends Fixture implements FixtureGroupInterface
    {
        public function load(ObjectManager $manager)
        {
            // créer un foo
            // $foo = new Foo();
            // $manager->persist($foo);

            // sauvegarder le tout en BDD
            $manager->flush();
        }

        public function getDependencies()
        {
            return array(
                RequiredFixtures::class,
            );
        }

        public static function getGroups(): array
        {
            return ['test'];
        }
    }

## Charger les fixtures

**Attention : l'idée est de supprimer d'abord toutes les données avant de charger les fixtures.**
Autrement dit, cette procédure supprime toutes les données de votre BDD.

### Charger les fixtures dans la BDD de prod

Cette manip se fait sur votre serveur de prod.

Pour charger les données dans votre BDD de prod, lancez les commandes suivante :

    php bin/console doctrine:fixtures:load --no-interaction --group=required --purge-with-truncate

### Charger les fixtures dans la BDD de dev

Cette manip se fait sur votre poste de dev.

Pour charger les données dans votre BDD de dev, lancez les commandes suivante :

    php bin/console doctrine:fixtures:load --no-interaction --group=test --purge-with-truncate

### Charger les fixtures dans la BDD de test

Cette manip se fait sur votre serveur de test.

Pour charger les données dans votre BDD de test, lancez les commandes suivante :

    php bin/console doctrine:fixtures:load --no-interaction --group=test --purge-with-truncate

## Le package `hautelook/AliceBundle`

Ce package permet de créer des fixtures à partir de code YAML.

Son usage est un peu plus avancé mais permet de créer plus facilement des données complexes.

Exemples :

- créer 5 projets
- créer 10 tags
- créer 10 students, chacun lié aléatoirement à un des 5 projets et à 3 des 10 tags

Pour en savoir plus, voir [hautelook/AliceBundle: A Symfony bundle to manage fixtures with Alice and Faker.](https://github.com/hautelook/AliceBundle).

## Doc

- [javiereguiluz/EasySlugger: A fast and easy to use slugger with full UTF-8 support](https://github.com/javiereguiluz/EasySlugger)

### Fixtures

- [DoctrineFixturesBundle (Symfony Bundles Docs)](https://symfony.com/doc/master/bundles/DoctrineFixturesBundle/index.html)
- [hautelook/AliceBundle: A Symfony bundle to manage fixtures with Alice and Faker.](https://github.com/hautelook/AliceBundle)

### Faker

**Attention : fzaninotto/Faker est officiellement non maintenu depuis le 21/10/2020.**

- [fzaninotto/Faker: Faker is a PHP library that generates fake data for you](https://github.com/fzaninotto/Faker)
- [Sunsetting PHP Faker](https://marmelab.com/blog/2020/10/21/sunsetting-faker.html?fbclid=IwAR1L086nsjvB2ObHU78ylWPEti9hW7w0HIVWKiJnmO8gaJhn4FO5MlvrGgQ)

Le repo FakerPHP/Faker est le fork principal qui va prendre la relève.

- [GitHub - FakerPHP/Faker: Faker is a PHP library that generates fake data for you](https://github.com/FakerPHP/Faker)

