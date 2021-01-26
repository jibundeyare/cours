# Symfony 4.4 étapes

Ce guide liste les principales étapes à suivre lors du développement d'un projet Symfony, de l'installation au déploiement.
Bien sûr vous pouvez changer l'ordre de certaines étapes, en supprimer ou en rajouter.

Attention : je pars du principe que vous avez déjà fait l'analyse du schéma de BDD.

## Prérequis

- la commande `symfony` pour la création d'un projet symfony
- la commande `composer` pour l'installation de packages
- la commande `dep` pour le déploiement
- une bonne dose de patience !

## Étapes

1. création du projet avec la commande `symfony new`
   attention aux options `--version` ou `--full`
2. création du fichier local des variables d'environnement  
   ce fichier contient les codes d'accès de la BDD ou du serveur de mail
3. création de la BDD avec la commande `php bin/console doctrine:database:create`  
   ou avec l'install script `mkdb.sh`
4. *(optionel)* installation de packages supplémentaires avec la commande `composer require`
5. création d'une classe `User` avec la commande `php bin/console make:user`
6. création d'un fichier de migration avec la commande `php bin/console doctrine:migrations:diff` ou `php bin/console make:migration`
7. application du fichier de migration sur la BDD avec la commande `php bin/console doctrine:migrations:migrate`
8. création d'entités avec la commande `php bin/console make:entity`  
   personnellement, je préfère créer toutes les entités puis ajouter les relations dans un deuxième temps  
   mais ce n'est pas une obligation, on peut commencer à créer les relations avant que toutes les entités soient disponibles
9. création d'un fichier de migration avec la commande `php bin/console doctrine:migrations:diff` ou `php bin/console make:migration`
10. application du fichier de migration sur la BDD avec la commande `php bin/console doctrine:migrations:migrate`
11. création d'un CRUD pour chaque entité
12. correction des champs de type `EntityType` dans le formulaire de chaque entité (dossier `src/Form`)
13. *(optionel)* correction des routes dans les contrôleurs de chaque entité (dossier `src/Controller`)
14. *(optionel)* ajout ou modification d'entités (retour à l'étape `8`)
15. activation du mode thème bootstrap de twig dans le fichier de configuration `config/packages/twig.yaml`
16. installation du package webpack encore avec la commande `composer require symfony/webpack-encore-bundle`
17. installation des dépendances front-end avec la commande `npm install` ou `yarn install`
18. activation de SASS dans le fichier de configuration de webpack `webpack.config.js`
19. personnalisation des fichiers `assets/styles/app.scss` et `assets/app.js`  
    compilation des fichiers SASS et js avec la commande `npm run dev` ou `yarn encore dev`  
    ou compilation continue avec la commande `npm run watch` ou `yarn encore dev --watch`  
    ajout des balises du package webpack encore dans le template de base `template/base.html.twig` : `{{ encore_entry_link_tags('app') }}` et `{{ encore_entry_script_tags('app') }}`
20. personnalisation du template de base `template/base.html.twig` (ajout de balises `block`, ajout bootstrap via un CDN, création d'un container bootstrap, etc)
21. création d'un formulaire d'inscription avec la commande `php bin/console make:registration-form`
22. adaptation des champs dans le formulaire d'inscription `src/Form/RegistrationFormType.php`
    on peut réutiliser les champs du form dedié à l'entité `src/Form/UserType.php`
23. configuration de permissions d'accès (les ACL) dans la section `access_control` du fichier `config/packages/security.yaml`
24. création d'un formulaire d'authentification avec la commande `php bin/console make:auth`
25. personnalisation (activation de la redirection) du contrôleur d'authentification `src/Controller/SecurityController.php`
26. personnalisation (redirection en fonction de l'utilisateur) de l'authentificateur `src/Security/LoginFormAuthenticator.php`
27. *(optionel)* installation d'un back office généré automatiquement avec la commande `composer require easycorp/easyadmin-bundle`
28. *(optionel)* installation du serveur d'api avec la commande `composer req api`
    ajout du `use ApiPlatform\Core\Annotation\ApiResource;` et de l'annotation `@ApiResource()` à toutes les entités qui doivent être accessible via l'api
29. installation du package doctrine fixtures bundle avec la commande `composer require --dev orm-fixtures`
30. création de fixtures
31. chargement des fixtures avec la commande ` php bin/console doctrine:fixtures:load`
32. création de tests unitaires ou fonctionnels
33. lancement des teste avec la commande `./bin/phpunit`
34. *(optionel)* si votre serveur web est `apache2`, création du fichier `public/.htaccess` avec la commande `composer require symfony/apache-pack`
35. création du fichier de déploiement de l'outil `dep` (Deployer)
36. configuration du serveur
37. déploiement avec avec l'outil `dep` (Deployer)

## Création d'utilisateurs de test

Vous pouvez créer des utilisateurs de test avec des fixtures mais au début, il est souvent plus simple de créer le premier utilisateur avec PhpMyAdmin.

1. insertion dans la BDD  
   à ce stade on met le mot de passe en clair
2. hashage du mot de passe avec la commande `php bin/console security:encode-password`
3. remplacement de la version claire du mot de passe par la version hashée dans la BDD

