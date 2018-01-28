# Symfony

## Concepts de base

- Entity
- Template (Twig)
- Controller

- ORM (object relational mapping)
- EntityManager
- Repository
- Route
- Form
- Validation

## Bundles

1. télécharger le bundle avec composer
2. activer le bundle dans le fichier `app/AppKernel.php`
3. configurer le bundle

## Installation

Se rendre dans le dossier des projets (Windows) :

    cd C:\wamp64\www

Se rendre dans le dossier des projets (Macos) :

    cd /Applications/MAMP/htdocs

Installer Symfony avec l'installeur `symfony` :

    symfony new my_project 3.4

Aller dans le dossier du projet :

    cd my_project

Lancer le serveur web de développement :

    php bin/console server:run

Ouvrir l'url suivante dans un navigateur :

    http://localhost:8000/

Conseil : si vous rencontrer des problèmes à cette étape, consultez « Erreur lors de l'affichage de la home » dans [symfoy-3.4-trouble-shooting.md](symfoy-3.4-trouble-shooting.md).

## Configuration de l'accès à la base de données

### Création de la base de données

Avec PhpMyAdmin, créer une nouvelle base de données.

Les noms (BDD, tables, colonnes, etc) ne doivent comporter aucun espace ` `, ni tiret `-`, ni accent. Il est donc préférable de se limiter aux caractères de l'alphabet, aux chiffres et au « underscore » (tiret du bas) `_`.

Pour le nom de la BDD, choisissez le même nom que votre projet.

L'interclassement de la BDD doit être `utf8mb4_unicode_ci`.

### Configuration de Symfony

Modifier le fichier `app/config/parameters.yml` :

    database_name: symfony
    database_user: root
    database_password: null

afin d'obtenir :

    database_name: my_project
    database_user: root
    database_password: null

Adapter le `database_user` et le `database_password` avec ceux que vous utilisez quand vous vous connectez à PhpMyAdmin.

Conseils :

- avec MAMP, le `database_user` et le `database_password` par défaut sont tout les deux `root`
- si vous avez une erreur de connexion avec MAMP, consultez « Erreur de connexion à la base de données avec MAMP » dans [symfoy-3.4-trouble-shooting.md](symfoy-3.4-trouble-shooting.md)

## Bundles

Les Bundles sont des plugins.

### Bundle `remg/generator-bundle`

Installer le bundle :

    composer require --dev remg/generator-bundle dev-master

Pour activer le bundle, ouvrir le fichier `app/AppKernel.php` et modifier le bloc :

    if ('dev' === $this->getEnvironment()) {
        $bundles[] = new Sensio\Bundle\GeneratorBundle\SensioGeneratorBundle();
        $bundles[] = new Symfony\Bundle\WebServerBundle\WebServerBundle();
    }

afin d'obtenir :

    if ('dev' === $this->getEnvironment()) {
        $bundles[] = new Sensio\Bundle\GeneratorBundle\SensioGeneratorBundle();
        $bundles[] = new Symfony\Bundle\WebServerBundle\WebServerBundle();
        $bundles[] = new Remg\GeneratorBundle\RemgGeneratorBundle();
    }

Pour configurer le bundle, ajouter à la fin du fichier `app/config/config_dev.yml` :

    remg_generator:
        entity:
            # available configuration formats are: 'annotation', 'yaml', 'xml' and 'php'.
            configuration_format: annotation

### Bundle `doctrine/doctrine-migrations-bundle`

Installer le bundle :

    composer require doctrine/doctrine-migrations-bundle ~1.0

## Entités

### Création avec `remg/generator-bundle`

Créer une entité :

    php bin/console remg:generate:entity

### Configuration de valeurs par défaut

Les valeurs par défaut sont définies dans le constructeur.

Par convention, le constructeur est toujours situé entre le dernier attribut (variable) et la première méthode (fonction).

    private $category;              // <= dernier attribut

    /**
     * Constructor
     */
    public function __construct()
    {
        $this->isDone = false;
    }

    /**
     * Get id
     *
     * @return integer
     */
    public function getId() {       // <= première méthode

## Validation de formulaire

Pour désactiver la validation côté client (dans le navigateur web), modifier la fonction `configureOptions()` du form type afin d'obtenir :

    class PostType extends AbstractType
    {
        // ...

        public function configureOptions(OptionsResolver $resolver)
        {
            $resolver->setDefaults(array(
                'attr'=> array('novalidate' => 'novalidate'),
                'data_class' => 'AppBundle\Entity\Post'
            ));
        }

        // ...
    }

Attention : veiller à adapter le nom de l'entité (`AppBundle\Entity\Post` dans l'exemple).

## Commandes

Afficher la liste des routes disponibles :

    php bin/console debug:router

Afficher la liste des services disponibles dans le container :

    php bin/console debug:container

Afficher la correspondance entre les services et le nom des classes PHP qui peuvent être utilisés en « type hinting » :

    php bin/console debug:autowiring

Afficher la liste des events et leur degré de priorité :

    php bin/console debug:event-dispatcher

## Doc

- [Symfony 3.4 Documentation](https://symfony.com/doc/3.4/index.html)

### Install

- [Installing & Setting up the Symfony Framework (Symfony 3.4 Docs)](http://symfony.com/doc/3.4/setup.html)

### Entity

- [Databases and the Doctrine ORM (Symfony 3.4 Docs)](https://symfony.com/doc/3.4/doctrine.html)
- [Remg/GeneratorBundle: Code generation tools for Symfony3.](https://github.com/Remg/GeneratorBundle)

### Form

- [Form Types Reference (Symfony 3.4 Docs)](http://symfony.com/doc/3.4/reference/forms/types.html)
- [EntityType Field (Symfony 3.4 Docs)](http://symfony.com/doc/3.4/reference/forms/types/entity.html)
- [Validation Constraints Reference (Symfony 3.4 Docs)](http://symfony.com/doc/3.4/reference/constraints.html)

### Route

- [Routing (Symfony 3.4 Docs)](https://symfony.com/doc/3.4/routing.html)
- [SensioFrameworkExtraBundle (Symfony Bundles Docs)](https://symfony.com/doc/master/bundles/SensioFrameworkExtraBundle/index.html)

## Bootstrap et jQuery

- [twig - What is the correct way to add bootstrap to a symfony app? - Stack Overflow](https://stackoverflow.com/questions/36453039/what-is-the-correct-way-to-add-bootstrap-to-a-symfony-app)
- [Bootstrap · The world's most popular mobile-first and responsive front-end framework.](https://getbootstrap.com/docs/3.3/)
- [jQuery](http://jquery.com/)
- [Getting started · Bootstrap](https://getbootstrap.com/docs/3.3/getting-started/)
- [jQuery CDN](https://code.jquery.com/)
