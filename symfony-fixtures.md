# Symfony data fixtures

Les data fixtures sont des données que l'on charge dans la BDD pour faire des tests ou parce qu'elles sont nécessaires au bon fonctionnement d'une application.

Le package `doctrine/doctrine-fixtures-bundle` permet de créer facilement des data fixtures.

Le package `fzaninotto/faker` permet de générer des fauses données de façon aléatoire.
Par exemple : des noms de personne, des adresses, des numéros de téléphone, des paragraphes de texte (du type lorem ipsum), etc.

Le package `javiereguiluz/easyslugger` permet de sluggifier, c-à-d de transformer toute chaîne de caractères en chaîne de caractères sans majuscule, sans accent et sans espaces.
Par exemple : `Foo bar baz` devient `foo-bar-baz`.

## Install

    composer require orm-fixtures --dev
    composer require fzaninotto/faker --dev
    composer require javiereguiluz/easyslugger --dev

## Créer un gabarit de data fixtures

    php bin/console make:fixtures

Cette commande créé un nouveau fichier dans le dossier `src/DataFixtures`.

## Exemple de fixture

Le fichier suivant permet de générer :

- 1 compte super admin
- 1 compte admin
- 1 student nommé Toto
- 1 student nommé Titi
- 10 students avec un nom et un email générés aléatoirement

    <?php
    // src/DataFixtures/AppFixtures.php

    namespace App\DataFixtures;

    use App\Entity\Student;
    use App\Entity\User;
    use Doctrine\Bundle\FixturesBundle\Fixture;
    use Doctrine\Common\Persistence\ObjectManager;
    use EasySlugger\Slugger;
    use Symfony\Component\Security\Core\Encoder\UserPasswordEncoderInterface;

    class AppFixtures extends Fixture
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

## Charger les data fixtures

Attention : le chargement des data fixtures supprime d'abord les données de votre BDD.

Pour charger les données dans votre BDD, lancez la commande suivante :

    php bin/console doctrine:fixture:load --purge-with-truncate --no-interaction

Note : l'option `--purge-with-truncate` vide la BDD avant de charger les data fixtures.

Si vous voulez charger les données dans votre BDD de test, ajouter l'option `--env=test` :

    php bin/console doctrine:fixture:load --env=test --purge-with-truncate --no-interaction

## Le package `hautelook/AliceBundle`

Ce package permet de créer des data fixtures à partir de code YAML.

Son usage est un petit peu plus avancé mais permet de créer plus facilement des données complexes.

Exemples :

- créer 5 projets
- créer 10 tags
- créer 10 students, chacun lié aléatoirement à un des 5 projets et à 3 des 10 tags

Pour en savoir plus, voir [hautelook/AliceBundle: A Symfony bundle to manage fixtures with Alice and Faker.](https://github.com/hautelook/AliceBundle).

## Doc

- [DoctrineFixturesBundle (Symfony Bundles Docs)](https://symfony.com/doc/current/bundles/DoctrineFixturesBundle/index.html)
- [fzaninotto/Faker: Faker is a PHP library that generates fake data for you](https://github.com/fzaninotto/Faker)
- [javiereguiluz/EasySlugger: A fast and easy to use slugger with full UTF-8 support](https://github.com/javiereguiluz/EasySlugger)
- [hautelook/AliceBundle: A Symfony bundle to manage fixtures with Alice and Faker.](https://github.com/hautelook/AliceBundle)
