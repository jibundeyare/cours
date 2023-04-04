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
12. *(optionnel)* personnalisation de repository : ajouts de méthodes qui permettent de faire de nouveaux types de recherches
13. retour à l'étape `9` autant de fois que nécessaire
14. installation du package `doctrine/fixtures-bundle` avec la commande `composer require --dev orm-fixtures` (si ça n'a pas déjà été fait)
15. création de fixtures
16. chargement des fixtures avec la commande ` php bin/console doctrine:fixtures:load --no-interaction`  
    ou création d'un fichier de script `bin/dofilo.sh` avec quelques commandes supplémentaires
17. création d'un formulaire d'authentification avec la commande `php bin/console make:auth`
18. personnalisation du contrôleur d'authentification (activation de la redirection) `src/Controller/SecurityController.php`
19. personnalisation de l'authentificateur (redirection en fonction du rôle de l'utilisateur) `src/Security/LoginFormAuthenticator.php`
20. *(optionnel)* création d'un formulaire d'inscription avec la commande `php bin/console make:registration-form`
21. *(optionnel)* adaptation des champs dans le formulaire d'inscription `src/Form/RegistrationFormType.php`
22. configuration de permissions d'accès (les ACL) dans la section `access_control` du fichier `config/packages/security.yaml`  
    ou création de voters et ajout de filtres dans les contrôleurs avec la fonction `denyAccessUnlessGranted()`
23. *(optionnel)* ajout de filtres dans les contrôleurs et les templates avec la fonction `isGranted()` et `is_granted()` respectivement
24. création d'un CRUD pour chaque entité avec la commande `php bin/console make:crud`
25. correction des champs de type `EntityType` dans le formulaire de chaque entité (dossier `src/Form`)
26. *(optionnel)* adapation des routes, des contrôleurs, des formulaires et des vues par rapport aux rôles des utilisateurs (dossier `src/Controller`)
27. activation du mode thème bootstrap de twig dans le fichier de configuration `config/packages/twig.yaml`
28. installation du package webpack encore avec la commande `composer require symfony/webpack-encore-bundle`
29. installation des dépendances front-end avec la commande `npm install` ou `yarn install`
30. activation de SASS dans le fichier de configuration de webpack `webpack.config.js`
31. ajout des balises du package webpack encore dans le template de base `template/base.html.twig` : `{{ encore_entry_link_tags('app') }}` et `{{ encore_entry_script_tags('app') }}`
32. personnalisation des fichiers `assets/styles/app.scss` et `assets/app.js`
33. compilation en version dev des fichiers SASS et js avec les commandes `npm run dev`, `yarn dev`  
    ou compilation continue en version dev avec la commande `npm run watch` ou `yarn watch`  
    ou compilation en version prod avec la commande `npm run build` ou `yarn build`
34. personnalisation du template de base `template/base.html.twig` (ajout de balises `block`, ajout bootstrap via un CDN, création d'un container bootstrap, etc)
35. *(optionnel)* création de services dans le dossier `src/Service` pour mutualiser des foncionnalités dans toute l'application
36. *(optionnel)* installation d'un back office généré automatiquement avec la commande `composer require easycorp/easyadmin-bundle`
37. *(optionnel)* installation du serveur d'api avec la commande `composer req api`  
    ajout du `use ApiPlatform\Core\Annotation\ApiResource;` et de l'annotation `@ApiResource()` à toutes les entités qui doivent être accessible via l'api
38. création de tests unitaires ou fonctionnels
39. lancement des tests avec la commande `./bin/phpunit`
40. *(optionnel)* si votre serveur web est `apache2`, création du fichier `public/.htaccess` avec la commande `composer require symfony/apache-pack`
41. création du fichier de déploiement de l'outil `dep` (Deployer)
42. configuration du serveur (apache, mariadb et php fpm, entre autres)
43. déploiement avec avec l'outil `dep` (Deployer)

## Création d'utilisateurs de test

Vous pouvez créer des utilisateurs de test avec des fixtures mais quand on commence, on peut aussi créer le premier utilisateur « à la main ».

1. hachage du mot de passe avec la commande `php bin/console security:hash-password`
2. insertion dans la BDD avec PhpMyAdmin  
   collez la version « hachée » du mot de passe dans la colonne `password`

