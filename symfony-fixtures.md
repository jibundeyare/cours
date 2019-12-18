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

## Exemple de fixture

Le fichier suivant permet de générer :

- 1 compte super admin
- 1 compte admin
- 1 student nommé Toto
- 1 student nommé Titi
- 10 students avec un nom et un email générés aléatoirement

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
            $student->setFirstname('Toto');
            $student->setLastname('Pop');
            $student->setEmail('toto.pop@example.com');
            $manager->persist($student);

            // créer un autre student
            $student = new Student();
            $student->setFirstname('Titi');
            $student->setLastname('Pop');
            $student->setEmail('titi.pop@example.com');
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

Il est possible de tagger des fixtures pour l'environnement de dev, de test, de prod, etc.

Si vous voulez créer des fixtures taggées dev et prod par exemple, vous devez faire attention à la façon dont vous organisez les données.

- les fixtures de prod doivent contenir toutes les données absoluments nécessaires au bon fonctionnement de l'application (par exemple le compte admin, des options par défaut, etc)
- les fixtures de dev doivent contenir les données nécessaires aux tests de l'application

Donc quand vous recréez une nouvelle instance de l'application en phase de dev ou de test, vous devez charger :

1. les fixtures de prod
2. les fixtures de dev

### Fixtures non taggées

Ces fixtures seront chargées si vous ne spécifiez pas d'environnement particulier avec l'option `--group`.

    <?php
    // src/DataFixtures/AppFixtures.php

    namespace App\DataFixtures;

    // use App\Entity\Foo;
    use Doctrine\Bundle\FixturesBundle\Fixture;
    use Doctrine\Common\Persistence\ObjectManager;

    class AppFixtures extends Fixture
    {
        public function __construct()
        {
        }

        public function load(ObjectManager $manager)
        {
            // créer un foo
            // $foo = new Foo();
            // $manager->persist($foo);

            // sauvegarder le tout en BDD
            $manager->flush();
        }
    }

### Fixtures taggées prod

Ces fixtures ne seront chargées que si l'option `--group=prod` est utilisée.

    <?php
    // src/DataFixtures/ProdFixtures.php

    namespace App\DataFixtures;

    // use App\Entity\Foo;
    use Doctrine\Bundle\FixturesBundle\Fixture;
    use Doctrine\Bundle\FixturesBundle\FixtureGroupInterface;
    use Doctrine\Common\Persistence\ObjectManager;

    class ProdFixtures extends Fixture implements FixtureGroupInterface
    {
        public function __construct()
        {
        }

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
            return ['prod'];
        }
    }

### Fixtures taggées dev

Ces fixtures ne seront chargées que si l'option `--group=dev` ou `--group=test` est utilisée.

Normalement, les fixtures de dev nécessite que les fixtures de prod soient chargés d'abord.
Ceci est précisé dans la méthode `getDependencies()`.

    <?php
    // src/DataFixtures/DevFixtures.php

    namespace App\DataFixtures;

    use App\DataFixtures\ProdFixtures;
    // use App\Entity\Foo;
    use Doctrine\Bundle\FixturesBundle\Fixture;
    use Doctrine\Bundle\FixturesBundle\FixtureGroupInterface;
    use Doctrine\Common\Persistence\ObjectManager;

    class DevFixtures extends Fixture implements FixtureGroupInterface
    {
        public function __construct()
        {
        }

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
                ProdFixtures::class,
            );
        }

        public static function getGroups(): array
        {
            return ['test', 'dev'];
        }
    }

## Charger les fixtures

**Attention : l'idée est de supprimer d'abord toutes les données avant de charger les fixtures.**
Autrement dit, cette procédure supprime toutes les données de votre BDD.

### Charger les fixtures dans la BDD de prod

Cette manip se fait sur votre serveur de prod.

Pour charger les données dans votre BDD de prod, lancez les commandes suivante :

    php bin/console doctrine:fixtures:load --no-interaction --group=prod --purge-with-truncate

### Charger les fixtures dans la BDD de dev

Cette manip se fait sur votre poste de dev.

Pour charger les données dans votre BDD de dev, lancez les commandes suivante :

    php bin/console doctrine:fixtures:load --no-interaction --group=prod --purge-with-truncate
    php bin/console doctrine:fixtures:load --no-interaction --group=dev --append

### Charger les fixtures dans la BDD de test

Cette manip se fait sur votre serveur de test.

Pour charger les données dans votre BDD de test, lancez les commandes suivante :

    php bin/console doctrine:fixtures:load --no-interaction --group=prod --purge-with-truncate
    php bin/console doctrine:fixtures:load --no-interaction --group=dev --append

## Le package `hautelook/AliceBundle`

Ce package permet de créer des fixtures à partir de code YAML.

Son usage est un petit peu plus avancé mais permet de créer plus facilement des données complexes.

Exemples :

- créer 5 projets
- créer 10 tags
- créer 10 students, chacun lié aléatoirement à un des 5 projets et à 3 des 10 tags

Pour en savoir plus, voir [hautelook/AliceBundle: A Symfony bundle to manage fixtures with Alice and Faker.](https://github.com/hautelook/AliceBundle).

## Doc

- [DoctrineFixturesBundle (Symfony Bundles Docs)](https://symfony.com/doc/master/bundles/DoctrineFixturesBundle/index.html)
- [fzaninotto/Faker: Faker is a PHP library that generates fake data for you](https://github.com/fzaninotto/Faker)
- [javiereguiluz/EasySlugger: A fast and easy to use slugger with full UTF-8 support](https://github.com/javiereguiluz/EasySlugger)
- [hautelook/AliceBundle: A Symfony bundle to manage fixtures with Alice and Faker.](https://github.com/hautelook/AliceBundle)
