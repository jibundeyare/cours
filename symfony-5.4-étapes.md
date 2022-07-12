# Symfony 5.4 étapes

Ce guide liste les principales étapes à suivre lors du développement d'un projet Symfony, de l'installation au déploiement.
Bien sûr vous pouvez changer l'ordre de certaines étapes, en supprimer ou en rajouter.

Attention : je pars du principe que vous avez déjà fait l'analyse du schéma de BDD.

## Prérequis

- la commande `symfony` pour la création d'un projet Symfony
- la commande `composer` pour l'installation de packages
- la commande `dep` pour le déploiement
- un accès à la documentation de Symfony : [Symfony current Documentation](https://symfony.com/doc/current/index.html)
- une bonne dose de patience !

## Étapes

1. création du projet avec la commande `symfony new`  
   attention aux options `--webapp` ou `--version`
2. création du fichier local des variables d'environnement `.env.local`  
   ce fichier contient les codes d'accès de la BDD ou du serveur de mail
3. choix de la langue dans le fichier de configuration `config/packages/translation.yaml`
4. création de la BDD avec la commande `php bin/console doctrine:database:create` (si ça n'a pas déjà été fait)
5. *(optionnel)* installation de packages supplémentaires avec la commande `composer require`
6. création d'une classe `User` avec la commande `php bin/console make:user`
7. création d'un fichier de migration avec la commande `php bin/console doctrine:migrations:diff` ou `php bin/console make:migration`
8. application du fichier de migration sur la BDD avec la commande `php bin/console doctrine:migrations:migrate`
9. création d'entités avec la commande `php bin/console make:entity`  
   personnellement, je préfère créer toutes les entités puis ajouter les relations dans un deuxième temps  
   mais ce n'est pas une obligation, on peut commencer à créer les relations avant que toutes les entités soient disponibles
10. création d'un fichier de migration avec la commande `php bin/console doctrine:migrations:diff` ou `php bin/console make:migration`
11. application du fichier de migration sur la BDD avec la commande `php bin/console doctrine:migrations:migrate`
12. installation du package `doctrine/fixtures-bundle` avec la commande `composer require --dev orm-fixtures` (si ça n'a pas déjà été fait)
13. création de fixtures
14. chargement des fixtures avec la commande ` php bin/console doctrine:fixtures:load --no-interaction`  
    ou création d'un fichier de script `bin/dofilo.sh` avec quelques commandes supplémentaires

15. création d'un CRUD pour chaque entité avec la commande `php bin/console make:crud`
16. correction des champs de type `EntityType` dans le formulaire de chaque entité (dossier `src/Form`)
17. *(optionnel)* correction des routes dans les contrôleurs de chaque entité (dossier `src/Controller`)
18. *(optionnel)* ajout ou modification d'entités (retour à l'étape `8`)
19. activation du mode thème bootstrap de twig dans le fichier de configuration `config/packages/twig.yaml`
20. installation du package webpack encore avec la commande `composer require symfony/webpack-encore-bundle`
21. installation des dépendances front-end avec la commande `npm install` ou `yarn install`
22. activation de SASS dans le fichier de configuration de webpack `webpack.config.js`
23. personnalisation des fichiers `assets/styles/app.scss` et `assets/app.js`  
    compilation des fichiers SASS et js avec la commande `npm run dev` ou `yarn encore dev`  
    ou compilation continue avec la commande `npm run watch` ou `yarn encore dev --watch`  
    ajout des balises du package webpack encore dans le template de base `template/base.html.twig` : `{{ encore_entry_link_tags('app') }}` et `{{ encore_entry_script_tags('app') }}`
24. personnalisation du template de base `template/base.html.twig` (ajout de balises `block`, ajout bootstrap via un CDN, création d'un container bootstrap, etc)
25. création d'un formulaire d'inscription avec la commande `php bin/console make:registration-form`
26. adaptation des champs dans le formulaire d'inscription `src/Form/RegistrationFormType.php`  
    on peut réutiliser les champs du form dedié à l'entité `src/Form/UserType.php`
27. configuration de permissions d'accès (les ACL) dans la section `access_control` du fichier `config/packages/security.yaml`  
    ou création de voters et ajout de filtres dans les contrôleurs avec la fonction `denyAccessUnlessGranted()`
28. *(optionnel)* ajout de filtres dans les contrôleurs et les templates avec la fonction `isGranted()` et `is_granted()` respectivement
29. création d'un formulaire d'authentification avec la commande `php bin/console make:auth`
30. personnalisation (activation de la redirection) du contrôleur d'authentification `src/Controller/SecurityController.php`
31. personnalisation (redirection en fonction de l'utilisateur) de l'authentificateur `src/Security/LoginFormAuthenticator.php`
32. *(optionnel)* personnalisation de contrôleurs : ajout de nouvelles routes
33. *(optionnel)* personnalisation de repository : ajouts de méthodes qui permettent de faire de nouveaux types de recherches
34. *(optionnel)* création de services dans le dossier `src/Service` pour mutualiser des foncionnalités dans toute l'application
35. *(optionnel)* installation d'un back office généré automatiquement avec la commande `composer require easycorp/easyadmin-bundle`
36. *(optionnel)* installation du serveur d'api avec la commande `composer req api`  
    ajout du `use ApiPlatform\Core\Annotation\ApiResource;` et de l'annotation `@ApiResource()` à toutes les entités qui doivent être accessible via l'api
37. création de tests unitaires ou fonctionnels
38. lancement des teste avec la commande `./bin/phpunit`
39. *(optionnel)* si votre serveur web est `apache2`, création du fichier `public/.htaccess` avec la commande `composer require symfony/apache-pack`
40. création du fichier de déploiement de l'outil `dep` (Deployer)
41. configuration du serveur (apache, mariadb et php fpm, entre autres)
42. déploiement avec avec l'outil `dep` (Deployer)

## Création d'utilisateurs de test

Vous pouvez créer des utilisateurs de test avec des fixtures mais quand on commence, on peut aussi créer le premier utilisateur « à la main ».

1. hashage du mot de passe avec la commande `php bin/console security:encode-password`
2. insertion dans la BDD avec PhpMyAdmin  
   collez la version « hashée » du mot de passe dans la colonne `password`

