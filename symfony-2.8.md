# Symfony 2.8

## Bundles

    Remg/GeneratorBundle

## Commandes utiles

Générer un CRUD :

    php app/console doctrine:generate:crud

## Installation

Installer Symfony 2.8 :

	composer create-project symfony/framework-standard-edition my_project_2.8 ^2.8

Pendant le téléchargement, créer une nouvelle base de données avec phpmyadmin.

Lors de la configuration du fichier `app/config/parameters.yml`, accepter les valeurs par défaut sauf les champs suivants qui doivent être adaptés :

	database_name: symfony
	database_user: root
	database_password: null
	[...]
	secret: ThisTokenIsNotSoSecretChangeIt

afin d'obtenir :

	database_name: my_project
	database_user: root
	database_password: null
	[...]
	secret: 652bf42c0bd10547cd29bc21542826aa8d0d0038

Obtenir une valeur aléatoire pour le champs `secret` : [Symfony 2 Secret Generator - nux.net](http://nux.net/secret)

## Langue

Pour configurer la langue, ouvrir le fichier `app/config/config.yml` et adapter :

	parameters:
	    locale: en

	framework:
	    #esi: ~
	    #translator: { fallbacks: ['%locale%'] }

afin d'obtenir :

	parameters:
	    locale: fr

	framework:
	    #esi: ~
	    translator: { fallbacks: ['%locale%'] }

## Démarrer l'application web

Démarrer l'application web :

	cd my_project_2.8/
	php app/console server:run

## Création d'entités

Installer remg generator bundle :

	composer require --dev remg/generator-bundle dev-master

Si le message d'erreur suivant apparaît :

	./composer.json has been updated
	Loading composer repositories with package information
	Updating dependencies (including require-dev)
	Your requirements could not be resolved to an installable set of packages.

	  Problem 1
	    - Installation request for remg/generator-bundle dev-master -> satisfiable by remg/generator-bundle[dev-master].
	    - remg/generator-bundle dev-master requires php >=5.5.9 -> your PHP version (7.1.12) overridden by "config.platform.php" version (5.3.9) does not satisfy that requirement.


	Installation failed, reverting ./composer.json to its original content.

Affichez la version de php installée :

	php -v

Vous obtenez :

	PHP 7.1.12 (cli) (built: Nov 22 2017 17:43:08) ( NTS )
	Copyright (c) 1997-2017 The PHP Group
	Zend Engine v3.1.0, Copyright (c) 1998-2017 Zend Technologies

Ouvrir le fichier `composer.json` et adapter :

	"config": {
		"bin-dir": "bin",
		"platform": {
			"php": "5.3.9"
		},
		"sort-packages": true
	},

afin d'obtenir :

	"config": {
		"bin-dir": "bin",
		"platform": {
			"php": "7.1.12"
		},
		"sort-packages": true
	},

Réessayez :

	composer require --dev remg/generator-bundle dev-master

Si vous obetenez le message d'erreur suivant :

	./composer.json has been updated
	Loading composer repositories with package information
	Updating dependencies (including require-dev)
	Your requirements could not be resolved to an installable set of packages.

	  Problem 1
	    - Installation request for remg/generator-bundle dev-master -> satisfiable by remg/generator-bundle[dev-master].
	    - Conclusion: don't install doctrine/orm v2.5.13
	    - Conclusion: don't install doctrine/orm v2.5.12
	    - Conclusion: don't install doctrine/orm v2.5.11
	    - Conclusion: don't install doctrine/orm v2.5.10
	    - Conclusion: don't install doctrine/orm v2.5.9
	    - Conclusion: don't install doctrine/orm v2.5.8
	    - Conclusion: don't install doctrine/orm v2.5.7
	    - Conclusion: don't install doctrine/orm v2.5.6
	    - Conclusion: don't install doctrine/orm v2.5.5
	    - Conclusion: don't install doctrine/orm v2.5.4
	    - Conclusion: don't install doctrine/orm v2.5.3
	    - Conclusion: don't install doctrine/orm v2.5.2
	    - Can only install one of: doctrine/orm[v2.5.0, v2.4.8].
	    - Can only install one of: doctrine/orm[v2.5.0, v2.4.8].
	    - Can only install one of: doctrine/orm[v2.5.0, v2.4.8].
	    - remg/generator-bundle dev-master requires doctrine/orm ^2.5 -> satisfiable by doctrine/orm[v2.5.0, v2.5.1, v2.5.10, v2.5.11, v2.5.12, v2.5.13, v2.5.2, v2.5.3, v2.5.4, v2.5.5, v2.5.6, v2.5.7, v2.5.8, v2.5.9].
	    - Conclusion: don't install doctrine/orm v2.5.1
	    - Installation request for doctrine/orm (locked at v2.4.8, required as ^2.4.8) -> satisfiable by doctrine/orm[v2.4.8].


	Installation failed, reverting ./composer.json to its original content.

Mettre à jour doctrine/orm vers la version ^2.5 :

	composer require doctrine/orm ^2.5

Réessayez :

	composer require --dev remg/generator-bundle dev-master

Pour activer le bundle, ouvrir le fichier `app/AppKernel.php` et adapter :

	if (in_array($this->getEnvironment(), array('dev', 'test'), true)) {
		$bundles[] = new Symfony\Bundle\DebugBundle\DebugBundle();
		// [...]
		$bundles[] = new Sensio\Bundle\GeneratorBundle\SensioGeneratorBundle();
	}

afin d'obtenir :

	if (in_array($this->getEnvironment(), array('dev', 'test'), true)) {
		$bundles[] = new Symfony\Bundle\DebugBundle\DebugBundle();
		// [...]
		$bundles[] = new Sensio\Bundle\GeneratorBundle\SensioGeneratorBundle();
		$bundles[] = new Remg\GeneratorBundle\RemgGeneratorBundle();
	}

Pour configurer le bundle, ouvrir le fichier `app/config/config_dev.yml` et ajouter :

	# remg/generator-bundle
	remg_generator:
	    entity:
	        # available configuration formats are: 'annotation', 'yaml', 'xml' and 'php'.
	        configuration_format: annotation

Créer une entité :

	php app/console remg:generate:entity

## Création de la base de données

Installer doctrine migrations bundle

	composer require doctrine/doctrine-migrations-bundle "^1.0"

Pour activer le bundle, ouvrir le fichier `app/AppKernel.php` et adapter :

	$bundles = array(
		new Symfony\Bundle\FrameworkBundle\FrameworkBundle(),
		[...]
		new AppBundle\AppBundle(),
	);

afin d'obtenir :

	$bundles = array(
		new Symfony\Bundle\FrameworkBundle\FrameworkBundle(),
		[...]
		new AppBundle\AppBundle(),
		new Doctrine\Bundle\MigrationsBundle\DoctrineMigrationsBundle(),
	);

Pour configurer le bundle, ouvrir le fichier `app/config.yml` et ajouter :

	# doctrine/doctrine-migrations-bundle
	doctrine_migrations:
	    dir_name: "%kernel.root_dir%/DoctrineMigrations"
	    namespace: Application\Migrations
	    table_name: migration_versions
	    name: Application Migrations
	    organize_migrations: false # Version >=1.2 Possible values are: "BY_YEAR", "BY_YEAR_AND_MONTH", false

Obtenir le status actuel de la base de données (BDD) :

	php app/console doctrine:migrations:status

Créer un diff de la BDD :

	php app/console doctrine:migrations:diff

Appliquer le diff de la BDD :

	php app/console doctrine:migrations:migrate --no-interaction

## Création de CRUDs

Générer un crud :

	php app/console doctrine:generate:crud

Adapter le formulaire des posts `src/AppBundle/Form/PostType.php` :

	$builder->add('title')->add('body')->add('published')->add('category')->add('tags');

afin d'obtenir :

	$builder
		->add('title')
		->add('body')
		->add('published')
		->add('category', EntityType::class, array(
			'class' => 'AppBundle:Category',
			'choice_label' => 'name',
		))
		->add('tags', EntityType::class, array(
			'class' => 'AppBundle:Tag',
			'choice_label' => 'name',
			'multiple' => true,
			'required' => false,
		))
	;

Adapter le formulaire des tags `src/AppBundle/Form/TagType.php` :

	$builder->add('name')->add('posts');

afin d'obtenir :

	$builder->add('name');

## Admin

Installer javiereguiluz easyadmin bundle :

	composer require javiereguiluz/easyadmin-bundle

Pour activer le bundle, ouvrir le fichier `app/AppKernel.php` et adapter :

	$bundles = array(
		new Symfony\Bundle\FrameworkBundle\FrameworkBundle(),
		[...]
		new AppBundle\AppBundle(),
		new Doctrine\Bundle\MigrationsBundle\DoctrineMigrationsBundle(),
	);

afin d'obtenir :

	$bundles = array(
		new Symfony\Bundle\FrameworkBundle\FrameworkBundle(),
		[...]
        new AppBundle\AppBundle(),
		new Doctrine\Bundle\MigrationsBundle\DoctrineMigrationsBundle(),
		new EasyCorp\Bundle\EasyAdminBundle\EasyAdminBundle(),
	);

Pour configurer le bundle, ouvrir le fichier `app/config/routing.yml` et ajouter :

	# javiereguiluz/easyadmin-bundle
	easy_admin_bundle:
	    resource: "@EasyAdminBundle/Controller/AdminController.php"
	    type:     annotation
	    prefix:   /admin

déployer les dépendances statiques :

	php app/console assets:install --symlink

puis ouvrir le fichier `app/config/config.yml` et ajouter :

	# javiereguiluz/easyadmin-bundle
	easy_admin:
	    entities:
	        Post:
	            class: AppBundle\Entity\Post
	            form:
	                fields:
	                    - 'title'
	                    - 'body'
	                    - 'category'
	                    - { property: 'category', type: 'entity', type_options: { choice_label: 'name', required: false } }
	                    - { property: 'tags', type: 'entity', type_options: { choice_label: 'name', required: false } }
	        Category:
	            class: AppBundle\Entity\Category
	            form:
	                fields:
	                    - 'name'
	        Tag:
	            class: AppBundle\Entity\Tag
	            form:
	                fields:
	                    - 'name'

## Doc

- [Installing & Setting up the Symfony Framework (Symfony 2.8 Docs)](https://symfony.com/doc/2.8/setup.html)
- [Symfony 2.8 Documentation](https://symfony.com/doc/2.8/index.html)
- [The Bundle System (Symfony Bundles Docs)](http://symfony.com/doc/2.8/bundles.html)
- [Remg/GeneratorBundle: Code generation tools for Symfony3.](https://github.com/Remg/GeneratorBundle)
- [SensioGeneratorBundle (Symfony Bundles Docs)](http://symfony.com/doc/current/bundles/SensioGeneratorBundle/index.html)
- [Databases and the Doctrine ORM (Symfony 2.8 Docs)](https://symfony.com/doc/2.8/doctrine.html)
- [Form Types Reference (Symfony 2.8 Docs)](http://symfony.com/doc/2.8/reference/forms/types.html)
- [Validation Constraints Reference (Symfony 2.8 Docs)](http://symfony.com/doc/2.8/reference/constraints.html)
- [EasyAdminBundle (Symfony Bundles Docs)](http://symfony.com/doc/current/bundles/EasyAdminBundle/index.html)
- [Security (Symfony 2.8 Docs)](https://symfony.com/doc/2.8/security.html)
