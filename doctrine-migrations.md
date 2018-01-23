# Doctrine migrations

## Installation

Dans un terminal, si vous n'êtes pas déjà dans la dossier racine de votre projet, taper :

    cd [dossier du projet web]

Pour installer le paquet :

    composer require doctrine/doctrine-migrations-bundle ~1.0

@todo
// app/AppKernel.php
public function registerBundles()
{
    $bundles = array(
        //...
        new Doctrine\Bundle\MigrationsBundle\DoctrineMigrationsBundle(),
    );
}

@todo
doctrine_migrations:
    dir_name: "%kernel.root_dir%/DoctrineMigrations"
    namespace: Application\Migrations
    table_name: migration_versions
    name: Application Migrations
    organize_migrations: false # Version >=1.2 Possible values are: "BY_YEAR", "BY_YEAR_AND_MONTH", false

## Utilisation

Attention : toujours jouer les scripts de migration avant de générer un nouveau diff.

    php app/console doctrine:migrations:status

    php app/console doctrine:migrations:diff

    php app/console doctrine:migrations:migrate

## Doc

- [DoctrineMigrationsBundle (Symfony Bundles Docs)](http://symfony.com/doc/current/bundles/DoctrineMigrationsBundle/index.html)
