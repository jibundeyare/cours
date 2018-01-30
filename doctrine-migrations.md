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

Pour configurer le bundle, ajouter à la fin du fichier `app/config/config_dev.yml` :

    doctrine_migrations:
        dir_name: "%kernel.root_dir%/DoctrineMigrations"
        namespace: Application\Migrations
        table_name: migration_versions
        name: Application Migrations
        organize_migrations: false # Version >=1.2 Possible values are: "BY_YEAR", "BY_YEAR_AND_MONTH", false

## Utilisation

Attention : toujours jouer les scripts de migration avant de générer un nouveau diff.

Obtenir le status actuel de la base de données (bdd) :

	php app/console doctrine:migrations:status

Créer un diff de la bdd :

	php app/console doctrine:migrations:diff

Appliquer le diff de la bdd :

	php app/console doctrine:migrations:migrate --no-interaction

## Doc

- [DoctrineMigrationsBundle (Symfony Bundles Docs)](http://symfony.com/doc/current/bundles/DoctrineMigrationsBundle/index.html)
