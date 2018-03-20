# Doctrine Migrations Bundle

Doctrine Migrations Bundle est un plugin Symfony qui permet de gérer les mises à jour (`upgrade`) et retour en arrière (`downgrade`) d'une base de données.

## Installation

Dans un terminal, si vous n'êtes pas déjà dans la dossier racine de votre projet, taper :

    cd [dossier du projet web]

Pour installer le bundle :

    composer require doctrine/doctrine-migrations-bundle ~1.0

Pour activer le bundle, ouvrir le fichier `app/AppKernel.php` et modifier le bloc :

    public function registerBundles()
    {
        $bundles = [
            new Symfony\Bundle\FrameworkBundle\FrameworkBundle(),
            new Symfony\Bundle\SecurityBundle\SecurityBundle(),
            new Symfony\Bundle\TwigBundle\TwigBundle(),
            new Symfony\Bundle\MonologBundle\MonologBundle(),
            new Symfony\Bundle\SwiftmailerBundle\SwiftmailerBundle(),
            new Doctrine\Bundle\DoctrineBundle\DoctrineBundle(),
            new Sensio\Bundle\FrameworkExtraBundle\SensioFrameworkExtraBundle(),
            new AppBundle\AppBundle(),
        ];

afin d'obtenir :

    public function registerBundles()
    {
        $bundles = [
            new Symfony\Bundle\FrameworkBundle\FrameworkBundle(),
            new Symfony\Bundle\SecurityBundle\SecurityBundle(),
            new Symfony\Bundle\TwigBundle\TwigBundle(),
            new Symfony\Bundle\MonologBundle\MonologBundle(),
            new Symfony\Bundle\SwiftmailerBundle\SwiftmailerBundle(),
            new Doctrine\Bundle\DoctrineBundle\DoctrineBundle(),
            new Sensio\Bundle\FrameworkExtraBundle\SensioFrameworkExtraBundle(),
            new AppBundle\AppBundle(),
            new Doctrine\Bundle\MigrationsBundle\DoctrineMigrationsBundle(),
        ];

Pour configurer le bundle, ajouter à la fin du fichier `app/config/config.yml` :

    # doctrine/doctrine-migrations-bundle
    doctrine_migrations:
        dir_name: "%kernel.root_dir%/DoctrineMigrations"
        namespace: Application\Migrations
        table_name: migration_versions
        name: Application Migrations
        organize_migrations: false # Version >=1.2 Possible values are: "BY_YEAR", "BY_YEAR_AND_MONTH", false

## Utilisation

Script de migration == diff de la BDD.

Jouer les scripts de migration == appliquer un diff de la BDD

### Validation du schéma

Obtenir le status actuel de la base de données (BDD) :

	php bin/console doctrine:schema:validate

Quatre cas de figure sont possibles :

1. erreur de mapping et erreur de BDD
2. erreur de mapping mais BDD synchronisée
3. mapping OK mais BDD pas synchronisée
4. tout est OK

#### Erreur de mapping

Voici un message d'erreur typique :

    [Mapping] FAIL - The entity-class 'AppBundle\Entity\Foo' mapping is invalid

Il faut plonger dans le code et corriger jusqu'à ce que les erreurs de mapping disparaissent.

#### Erreur de synchronisation de la BDD

Voici un message d'erreur typique :

    [Database] FAIL - The database schema is not in sync with the current mapping file.

Il faut générer un diff de la BDD et jouer les migrations.

### Génération d'un diff de la BDD

Attention : toujours jouer les scripts de migration avant de générer un nouveau diff.
Sinon on se retrouve avec des copies du même diff qui provoqueront des erreurs quand on jouera les migrations.

Générer un diff de la BDD :

	php bin/console doctrine:migrations:diff

### Affichage du status des scripts de migration

Obtenir le status actuel des scripts de migration :

	php bin/console doctrine:migrations:status

### Application des diff de la BDD

Jouer le script de migration :

	php bin/console doctrine:migrations:migrate --no-interaction

NB : vous pouvez revalider le schéma afin de vous assurer que les entités et la BDD sont bien synchronisées.

## Doc

- [DoctrineMigrationsBundle (Symfony Bundles Docs)](http://symfony.com/doc/current/bundles/DoctrineMigrationsBundle/index.html)
- [Databases and the Doctrine ORM (Symfony 3.4 Docs)](https://symfony.com/doc/3.4/doctrine.html)
- [5. Association Mapping — Doctrine 2 ORM 2 documentation](http://docs.doctrine-project.org/projects/doctrine-orm/en/latest/reference/association-mapping.html)
