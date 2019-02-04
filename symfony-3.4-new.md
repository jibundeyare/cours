# Symfony 3.4 New

Attention : ce cours installe une version de Symfony 3.4 qui a une structure de dossier identique avec celle de Symfony 4.x.

Dans Symfony 4.x, certains dossiers changent de nom et d'emplacement :

- `/app/config` devient `/config`
- `/app/Ressources/views` devient `/templates`
- `/src/AppBundle` devient `/src`
- `/web` devient `/public`

Le repo suivant contient le résultat de ce tutoriel : [jibundeyare/src-symfony-3.4](https://github.com/jibundeyare/src-symfony-3.4).

## Install

Attention : l'installation nécessite la nouvelle version (`4.x`) de l'installeur Symfony. Voir [symfony-installer.md](symfony-installer.md).

Installer Symfony 3.4 :

    symfony new --full --version=3.4 my_project

Se rendre dans le dossier du projet :

    cd my_project

Installer le bundle `symfony/web-server-bundle` :

    composer require server --dev

Lancer le serveur web de développement :

    php bin/console server:run

Note : pour stopper le serveur, appuyer sur `CTRL + C`.

Pour tester l'installation, ouvrir l'URL suivante dans un navigateur :

    http://localhost:8000/

## Configuration de l'accès à la base de données

Créez un fichier `.env.local` à la racine du projet.

Ouvrez le fichier `.env` à la racine du projet et copiez le bloc :

    APP_ENV=dev
    APP_SECRET=24209e2e73cff25fd43b74b21cf4e173

et colle-le dans le fichier `.env.local`.

Copiez aussi le bloc dans le fichier `.env` :

    DATABASE_URL=mysql://db_user:db_password@127.0.0.1:3306/db_name

et colle-le dans le fichier `.env.local`.

Ensuite, modifiez la ligne `DATABASE_URL` du fichier `.env.local` pour que Symfony puisse accéder à votre BDD.

Vous devez modifier les éléments `db_user`, `db_password`, `127.0.0.1`, `3306` et `db_name` avec les informations qui correspondent à votre configuration.

### Exemples de configuration de `DATABASE_URL`

Voici la ligne originale :

    DATABASE_URL=mysql://db_user:db_password@127.0.0.1:3306/db_name

#### Wamp

Voici la ligne pour le user `root`, sans mot de passe, une connection sur le port `3306` (mysql) sur le même serveur, la BDD `my_project` :

    DATABASE_URL=mysql://root:@127.0.0.1:3306/my_project

#### MAMP

Voici la ligne pour le user `root`, le mot de passe `root`, une connection sur le port `8889` (mysql) sur le même serveur, la BDD `my_project` :

    DATABASE_URL=mysql://root:root@127.0.0.1:8889/my_project

#### Autre

Voici la ligne pour le user `root`, le mot de passe `123`, une connection sur le port `3306` (mysql) sur le même serveur, la BDD `my_project` :

    DATABASE_URL=mysql://root:123@127.0.0.1:3306/my_project

## Le `APP_SECRET` de `.env`

Si vous voulez regénérer le `APP_SECRET` de votre fichier `.env` (ou de votre `.env.local`), vous pouvez utiliser le script suivant qui génèrera une chaîne de 32 caractères en hexadécimal tirée au hasard :

    <?php
    $bytes = random_bytes(16);
    echo bin2hex($bytes).PHP_EOL;

Sinon vous pouvez utiliser un outil en ligne (qui génère 40 caractères et non 32) : [Symfony 2 Secret Generator - nux.net](http://nux.net/secret).

## Mode debug

Il est possible de désactiver le mode debug même en mode dev.

Pour cela, il faut ajouter dans le fichier `.env.local` la ligne :

    APP_DEBUG=0

Si vous voulez réactiver le mode debug, vous pouvez commenter la ligne ou la changer pour obtenir :

    APP_DEBUG=1

## Création de la base de données

### Avec la console

Interrompre le serveur web avec `CTRL + C` si nécessaire.

Dans le terminal :

    php bin/console doctrine:database:create

Relancer le serveur web avec la commande `php bin/console server:run` si nécessaire.

### Avec PhpMyAdmin

Avec PhpMyAdmin, créer une nouvelle base de données.

Les noms (BDD, tables, colonnes, etc) ne doivent comporter aucun espace ` `, ni tiret `-`, ni point `.`, ni accent.
Il faut donc se limiter aux caractères minuscules de l'alphabet, aux chiffres et au « underscore » (tiret du bas) `_`.

Pour le nom de la BDD, choisissez le même nom que votre projet, en remplaçant les caractères interdits par des caractères autorisés.

L'interclassement de la BDD doit être `utf8mb4_unicode_ci`.

## Langue

Pour configurer la langue, ouvrir le fichier `config/services.yaml`, et modifier le bloc :

    # Put parameters here that don't need to change on each machine where the app is deployed
    # https://symfony.com/doc/current/best_practices/configuration.html#application-related-configuration
    parameters:
        locale: 'en'

afin d'obtenir :

    # Put parameters here that don't need to change on each machine where the app is deployed
    # https://symfony.com/doc/current/best_practices/configuration.html#application-related-configuration
    parameters:
        locale: 'fr'

## Doctrine ORM

Voir [doctrine-orm.md](doctrine-orm.md).

## Configuration de Doctrine ORM

Attention : si vous voulez utiliser le type de données `json` dans vos entités, il est primordiale de bien lire cette partie.

Si vous utilisez MySQL 5.7 ou plus, votre serveur est capable de traiter nativement des données de type `json`.
Mais si vous avez une version plus ancienne, votre serveur n'est pas capable traiter nativement des données de type `json` et il faut le signaler à Doctrine ORM.

Dans le fichier `config/packages/doctrine.yaml`, vous devez modifier la ligne :

            server_version: '5.7'

pour obtenir :

            server_version: '5.6'

Pour savoir quelle version de MySQL vous avez, tapez la commande suivante :

    mysql -V

Pour en savoir plus, voir :

- [Doctrine Configuration Reference (DoctrineBundle) (Symfony 3.4 Docs)](https://symfony.com/doc/3.4/reference/configuration/doctrine.html)
- [Customizing the User Entity > Symfony Security: Beautiful Authentication, Powerful Authorization | SymfonyCasts](https://symfonycasts.com/screencast/symfony-security/user-entity#setting-doctrines-server-version).

## Entités

La commande suivante permet de générer le code PHP d'une entité :

    php bin/console make:entity

## Contrôleur

Le contrôleur est un élément très important dans une application Symfony.
C'est l'élément qui permet de répondre à une demande de l'utilisateur et déclencher des actions comme une recherche en BDD, le traitement de données d'un formulaire ou l'affichage d'une page HTML.

La commande suivante permet de générer le code PHP d'un contrôleur de base :

    php bin/console make:controller

### Exemple de contrôleur principal

Dans le dossier `src/Controller`, créer le fichier `MainController.php` :

    <?php
    // src/Controller/MainController.php

    namespace App\Controller;

    use Symfony\Bundle\FrameworkBundle\Controller\Controller;
    use Symfony\Component\HttpFoundation\Request;
    use Symfony\Component\Routing\Annotation\Route;
    use Symfony\Component\Routing\Annotation\Method;

    /**
     * @Route("/")
     */
    class MainController extends Controller
    {
        /**
         * @Route("/", name="main_index")
         */
        public function index()
        {
            return $this->render('main/index.html.twig', [
                // ...
            ]);
        }

        /**
         * @Route("/hello/{name}", name="main_hello")
         */
        public function hello(Request $request, $name)
        {
            $greeting = "Hello {$name}!";

            return $this->render('main/hello.html.twig', [
                'greeting' => $greeting,
            ]);
        }
    }

Les routes portent des noms.
Dans l'annotation `@Route("/", name="main_index")`, l'URL de la route est `/` et le nom de la route est `main_index`.
Dans l'annotation `@Route("/hello/{name}", name="main_hello")`, l'URL de la route est `/hello/{name}` et le nom de la route est `main_hello`.
De plus, la partie `{name}` est une variable qui peut changer.
Dans le navigateur, l'utilisateur peut donc utiliser `/hello/Toto`, `/hello/Foo`, `/hello/Bar`, etc.

Dans le dossier `templates`, créer un dossier `main` puis créer le fichier `index.html.twig` dedans :

    {# templates/main/index.html.twig #}
    <!DOCTYPE html>
    <html>
    <head>
        <meta charset="utf-8" />
        <title>Hello MainController index!</title>
    </head>
    <body>
        <h1>Hello MainController index!</h1>
    </body>
    </html>

Toujours dans le dossier `main`, créer le fichier `hello.html.twig` :

    {# templates/main/hello.html.twig #}
    <!DOCTYPE html>
    <html>
    <head>
        <meta charset="utf-8" />
        <title>{{ greeting }}</title>
    </head>
    <body>
        <h1>{{ greeting }}</h1>
    </body>
    </html>

Pour tester de contôleur, vous devez lancer votre serveur web :

    php bin/console server:run

Puis ouvrir les URL associées :

- [http://localhost:8000/](http://localhost:8000/)
- [http://localhost:8000/hello/Toto](http://localhost:8000/hello/Toto)

### Changer le préfixe d'un contrôleur

Il est possible de changer les URL de toutes les actions d'un contrôleur.
Pour cela, il faut modifier m'annotation `@Route` qui est associée au contrôleur.

Par exemple, pour que toutes les URL commencent par `/main`, il faut modifier le contrôleur pour que la partie :

    // ...

    /**
     * @Route("/")
     */
    class MainController extends Controller
    {
        // ...

devienne :

    // ...

    /**
     * @Route("/main")
     */
    class MainController extends Controller
    {
        // ...

Les URL `/` et `hello/{name}` peuvent alors être ouvertes avec :

- [http://localhost:8000/main](http://localhost:8000/main)
- [http://localhost:8000/main/hello/Toto](http://localhost:8000/main/hello/Toto)

## Accéder au service `database_connection`

Modifier tous les contrôleurs qui doivent utiliser le service `database_connection`, en ajoutant la ligne de code suivante :

    use Doctrine\DBAL\Connection;

    // ...

        private $conn;

        public function __construct(Connection $conn)
        {
            $this->conn = $conn;
        }

### Exemple avec le contrôleur principal

Dans le dossier `src/Controller`, modifier le fichier `MainController.php` :

    <?php
    // src/Controller/MainController.php

    namespace App\Controller;

    use Doctrine\DBAL\Connection;
    use Symfony\Bundle\FrameworkBundle\Controller\Controller;
    use Symfony\Component\HttpFoundation\Request;
    use Symfony\Component\Routing\Annotation\Route;
    use Symfony\Component\Routing\Annotation\Method;

    /**
     * @Route("/")
     */
    class MainController extends Controller
    {
        private $conn;

        public function __construct(Connection $conn)
        {
            $this->conn = $conn;
        }

        /**
         * @Route("/", name="main_index")
         */
        public function index(Request $request)
        {
            return $this->render('main/index.html.twig', [
                // ...
            ]);
        }

        /**
         * @Route("/hello/{name}", name="main_hello")
         */
        public function hello(Request $request, $name)
        {
            $greeting = "Hello {$name}!";

            return $this->render('main/hello-name.html.twig', [
                'greeting' => $greeting,
            ]);
        }

        /**
         * @Route("/items", name="main_items_index")
         */
        public function itemsIndex(Request $request, $name)
        {
            $sql = 'SELECT * FROM item';
            $items = $this->conn->fetchAll($sql);

            return $this->render('main/items-index.html.twig', [
                'items' => $items,
            ]);
        }
    }

Il faut aussi créer un template qui peut afficher les données de la requête SQL.
Dans le dossier `templates/main`, créer le fichier `items-index.html.twig` :

    {# templates/main/items-index.html.twig #}
    {% block title %}Liste des items{% endblock %}
    {% block body %}
    <h1>Liste des items</h1>

    <ul>
    {% for item in items %}
        <li><a href="/item/{{item.id}}">{{ item.name }}</a></li>
    {% endfor %}
    </ul>
    {% endblock %}

## Authentification par mot de passe

L'authentification par mot de passe nécessite la mise en place de plusieurs choses :

- une ou plusieurs routes à sécuriser (sans ça, l'authentification n'a aucun sens)
- une entité `User` (ou autre nom de classe) qui représentera un utilisateur connecté
- un authentificateur (une classe) qui permettra de valider un mot de passe fourni par un utilisateur
- un contrôleur qui gèrera l'affichage du formulaire d'inscription et son enregistrement
- un template qui affichera le formulaire d'inscription
- un contrôleur qui gèrera l'affichage du formulaire de login et son authentification
- un template qui affichera le formulaire d'authentification
- une route qui gèrera la déconnexion

Afin de montrer un exemple, nous allons sécuriser la route `main_secured` (c-à-d `/secured`).
Les utilisateurs pourront s'inscrire via la route `/register`, s'authentifier via la route `/login` et seront redirigés vers la route `main_secured` si l'authentification a fonctionné.

Mais nous pourrions aussi bien créer un nouveau contrôleur dont le prefixe est `/admin` et sécuriser toutes les routes du type `/admin`.

Pour en savoir plus, voir :

- [Security (Symfony 3.4 Docs)](https://symfony.com/doc/3.4/security.html)
- [Symfony Security: Beautiful Authentication, Powerful Authorization Video Tutorial Screencast | SymfonyCasts](https://symfonycasts.com/screencast/symfony-security)


### Création d'une route sécurisée

Ouvrir le fichier `src/Controller/MainController.php` et ajouter la route suivante :

        /**
         * @Route("/secured", name="main_secured")
         */
        public function secured(Request $request)
        {
            return $this->render('main/secured.html.twig', [
                // ...
            ]);
        }

Créez le fichier `secured.html.twig` dans le dossier `templates/main` :

    {% extends 'base.html.twig' %}

    {% block title %}Hello {{ app.user.username }}!{% endblock %}

    {% block body %}
    <h1>Hello {{ app.user.username }}!</h1>
    {% endblock %}

Ouvrir le fichier `config/packages/security.yaml` pour protéger la nouvelle route.
Modifier la partie `access_control` :

        access_control:
            # - { path: ^/admin, roles: ROLE_ADMIN }
            # - { path: ^/profile, roles: ROLE_USER }

pour obtenir :

        access_control:
            # - { path: ^/admin, roles: ROLE_ADMIN }
            # - { path: ^/profile, roles: ROLE_USER }
            - { path: ^/secured, roles: IS_AUTHENTICATED_FULLY }

La ligne `{ path: ^/secured, roles: IS_AUTHENTICATED_FULLY }` veut dire que seuls les utilisateurs authentifiés ont accès aux route qui commencent par `/secured`.
Mais il est possible de limiter l'accès aux routes à des utilisateurs ayant certains niveaux de privilèges.
C'est ce que veulent dire `ROLE_USER` ou `ROLE_ADMIN`.

Il est aussi possible d'autoriser l'accès à plusieurs types d'utilisateurs avec la règle suivante :

            - { path: ^/secured, roles: [ROLE_USER, ROLE_ADMIN] }

De même, il est possible d'autoriser explicitement l'accès à des utilisateurs non authentifiés :

            - { path: ^/secured/foo, roles: IS_AUTHENTICATED_ANONYMOUSLY }

Dans ce dernier cas, les utilisateurs non authentifiés n'auront pas accès aux routes commençant par `/secured` mais auront quand même accès aux routes commençant par `/secured/foo`.
Attention cependant à ajouter cet `access_control` de la route `/secured/foo` avant celui de la route `/secured` :

            - { path: ^/secured/foo, roles: IS_AUTHENTICATED_ANONYMOUSLY }
            - { path: ^/secured, roles: IS_AUTHENTICATED_FULLY }

Sachez qu'il existe aussi d'autre paramètres pour filtrer des utilisateurs comme l'adresse IP ou le nom de domaine.

Pour en savoir plus, voir : [How Does the Security access_control Work? (Symfony 3.4 Docs)](https://symfony.com/doc/3.4/security/access_control.html).

### Création de l'entité `User`

Attention : avant de continuer, assurez-vous d'avoir bien lu la section « Configuration de Doctrine ORM ».
Les roles des utilisateurs sont stockés avec une donnée de type `json`, si Doctrine ORM n'est pas correctement configuré, vous aurez des erreurs.

Lancez la commande suivante :

    php bin/console make:user

Voici les réponses :

    The name of the security user class (e.g. User) [User]:
    > 

    Do you want to store user data in the database (via Doctrine)? (yes/no) [yes]:
    > 

    Enter a property name that will be the unique "display" name for the user (e.g. email, username, uuid) [email]:
    > 

    Will this app need to hash/check user passwords? Choose No if passwords are not needed or will be checked/hashed by some other system (e.g. a single sign-on server).

    Does this app need to hash/check user passwords? (yes/no) [yes]:
    > 

    The newer Argon2i password hasher requires PHP 7.2, libsodium or paragonie/sodium_compat. Your system DOES support this algorithm.
    You should use Argon2i unless your production system will not support it.

    Use Argon2i as your password hasher (bcrypt will be used otherwise)? (yes/no) [yes]:
    > no

    created: src/Entity/User.php
    created: src/Repository/UserRepository.php
    updated: src/Entity/User.php
    updated: config/packages/security.yaml

            
    Success! 
            

    Next Steps:
    - Review your new App\Entity\User class.
    - Use make:entity to add more fields to your Foo entity and then run make:migration.
    - Create a way to authenticate! See https://symfony.com/doc/current/security.html

Les fichiers suivants sont créés ou mis à jour :

    - `src/Entity/User.php`
    - `src/Repository/UserRepository.php`
    - `config/packages/security.yaml`

Notez que je réponds non à la question `Use Argon2i as your password hasher (bcrypt will be used otherwise)?` car je ne suis pas sûr que la prod possède également l'algorithme Argon2i.

Si vous voulez activer l'algorithme Argon2i par après, il suffit d'ouvrir le fichier `config/packages/security.yaml` et de modifier les lignes :

    security:
        encoders:
            App\Entity\User:
                algorithm: bcrypt

pour obtenir :

    security:
        encoders:
            App\Entity\User:
                algorithm: argon2i

Sauvegardons les changements dans la BDD :

    php bin/console doctrine:migrations:diff
    php bin/console doctrine:migrations:migrate

### Création de l'authentificateur et du formulaire d'authentification

Lancez la commande suivante :

    php bin/console make:auth

Voici les réponses :

    What style of authentication do you want? [Empty authenticator]:
    [0] Empty authenticator
    [1] Login form authenticator
    > 1

    The class name of the authenticator to create (e.g. AppCustomAuthenticator):
    > AppCustomAuthenticator

    Choose a name for the controller class (e.g. SecurityController) [SecurityController]:
    > 

    created: src/Security/AppCustomAuthenticator.php
    updated: config/packages/security.yaml
    created: src/Controller/SecurityController.php
    created: templates/security/login.html.twig

            
    Success! 
            

    Next:
    - Customize your new authenticator.
    - Finish the redirect "TODO" in the App\Security\AppCustomAuthenticator::onAuthenticationSuccess() method.
    - Review App\Security\AppCustomAuthenticator::getUser() to make sure it matches your needs.
    - Review & adapt the login template: templates/security/login.html.twig.

Les fichiers suivants sont créés ou mis à jour :

    - `src/Security/AppCustomAuthenticator.php`
    - `config/packages/security.yaml`
    - `src/Controller/SecurityController.php`
    - `templates/security/login.html.twig`

Vous devez indiquer à l'authentificateur, où les utilisateurs doivent être redirigés quand l'authentification a réussi.

Ouvrez le fichier `src/Security/AppCustomAuthenticator.php` et modifier les lignes suivantes :

        public function onAuthenticationSuccess(Request $request, TokenInterface $token, $providerKey)
        {
            if ($targetPath = $this->getTargetPath($request->getSession(), $providerKey)) {
                return new RedirectResponse($targetPath);
            }

            // For example : return new RedirectResponse($this->urlGenerator->generate('some_route'));
            throw new \Exception('TODO: provide a valid redirect inside '.__FILE__);
        }

pour obtenir :

    public function onAuthenticationSuccess(Request $request, TokenInterface $token, $providerKey)
    {
        if ($targetPath = $this->getTargetPath($request->getSession(), $providerKey)) {
            return new RedirectResponse($targetPath);
        }

        return new RedirectResponse($this->urlGenerator->generate('main_secured'));
    }

Vous pouvez rediriger les utilisateurs vers n'importe quelle route, qu'elle soit sécurisée ou non.
Changez juste `main_secured` par le nom de la route que vous souhaitez.

Si vous voulez personnaliser l'intégration HTML CSS du formulaire d'authentification, le template se trouve dans le fichier `templates/security/login.html.twig`.

### Création du formulaire d'inscription

Lancez la commande suivante :

    php bin/console make:registration-form

Voici les réponses :

    Creating a registration form for App\Entity\User

    Do you want to add a @UniqueEntity validation annotation on your User class to make sure duplicate accounts aren't created? (yes/no) [yes]:
    > 

    Do you want to automatically authenticate the user after registration? (yes/no) [yes]:
    > 

    updated: src/Entity/User.php
    created: src/Form/RegistrationFormType.php
    created: src/Controller/RegistrationController.php
    created: templates/registration/register.html.twig

            
    Success! 
            

    Next: Go to /register to check out your new form!
    Make any changes you need to the form, controller & template.

Les fichiers suivants sont créés ou mis à jour :

    - `src/Entity/User.php`
    - `src/Form/RegistrationFormType.php`
    - `src/Controller/RegistrationController.php`
    - `templates/registration/register.html.twig`

Si vous voulez modifier le formulaire d'inscription :

- pour ajouter des champs, le Form Type se trouve dans le fichier `src/Form/RegistrationFormType.php`. 
- pour personnaliser l'intégration HTML CSS, le template se trouve dans le fichier `templates/registration/register.html.twig`.

### Création automatique d'utilisateur

Pour créer plusieurs utilisateurs automatiquement, il faut utiliser des Fixtures.

Pour en savoir plus, voir [DoctrineFixturesBundle (Symfony Bundles Docs)](https://symfony.com/doc/current/bundles/DoctrineFixturesBundle/index.html).

### Rôle d'un utilisateur

Par défaut, tous les utilisateurs inscrits ont seulement le rôle `ROLE_USER`.

Si vous voulez ponctuellement attribuer le rôle `ROLE_ADMIN` à un utilisateur, vous pouvez utiliser une requête SQL.

Pour voir les rôles d'un utilisateur, tapez la commande suivante en adaptant son email :

    php bin/console doctrine:query:sql "SELECT * FROM user WHERE email = 'foo.bar@example.com'"

Pour attribuer le rôle `ROLE_ADMIN` à un utilisateur, tapez la commande suivante en adaptant son email :

    php bin/console doctrine:query:sql "UPDATE user SET roles = '[\"ROLE_ADMIN\"]' WHERE email = 'foo.bar@example.com'"

Pour supprimer le rôle `ROLE_ADMIN` d'un utilisateur, tapez la commande suivante en adaptant son email :

    php bin/console doctrine:query:sql "UPDATE user SET roles = '[]' WHERE email = 'foo.bar@example.com'"

### Rôle de plusieurs utilisateurs

Vous pouvez modifier la colonne `roles` de plusieurs utilisateurs avec une requête SQL, mais si vous voulez que tous les utilisateurs qui s'inscrivent aient le rôle `ROLE_ADMIN` vous devez modifier l'entité `User`.

Ouvrir le fichier `src/Entity/User.php` et ajouter ou modifier le constructeur :

        public function __construct()
        {
            $this->roles[] = 'ROLE_ADMIN';
        }

La ligne `$this->roles[] = 'ROLE_ADMIN';` ajoutera le rôle `ROLE_ADMIN` à tous les utilisateurs.

### Création d'une route de déconnexion

Ouvrir le fichier `src/Controller/SecurityController.php` et ajouter la route suivante :

        /**
         * @Route("/logout", name="app_logout")
         */
        public function logout(): void
        {
            throw new \Exception('Will be intercepted before getting here');
        }

Cette route est spéciale, en réalité le code de la fonction ne sera jamais appelé.
Symfony va intercepter la requête, déconecter l'utilisateur et le rediriger vers la page d'accueil (la route `/`).

Ouvrir le fichier `config/packages/security.yaml` et modifier le firewall `main` en y ajoutant :

                logout:
                    path: app_logout

Le firewall devrait ressembler à ceci :

        firewalls:
            dev:
                pattern: ^/(_(profiler|wdt)|css|images|js)/
                security: false
            main:
                anonymous: true
                guard:
                    authenticators:
                        - App\Security\AppCustomAuthenticator

                # activate different ways to authenticate

                # http_basic: true
                # https://symfony.com/doc/current/security.html#a-configuring-how-your-users-will-authenticate

                # form_login: true
                # https://symfony.com/doc/current/security/form_login_setup.html

                logout:
                    path: app_logout

### Authentification par token (API Key)

Ceci est l'étape d'après et permet de mettre en place l'authentification par token pour l'accès aux API.

Pour en savoir plus, voir : [How to Authenticate Users with API Keys (Symfony 3.4 Docs)](https://symfony.com/doc/3.4/security/api_key_authentication.html).

## Personnalisation des messages d'erreurs

La personnalisation des pages d'erreur est très simple.

Il suffit de créer le dossier `templates/bundles/TwigBundle/Exception` et d'y stocker ses propres templates en respectant des règles de nommage de fichier.

Dans un terminal, lancer la commande suivante :

    mkdir -p templates/bundles/TwigBundle/Exception

Puis créer un template Twig dans ce dossier.
Le template aura accès notamment aux variables suivantes :

- `status_code`
- `status_text`

Pour en savoir plus, voir : [How to Customize Error Pages (Symfony Docs)](https://symfony.com/doc/current/controller/error_pages.html).

### Exemple de page d'erreur de type `403 Access denied`.

Créer un fichier `error403.html.twig` dans le dossier `templates/bundles/TwigBundle/Exception` avec le code suivant :

    {% extends 'base.html.twig' %}

    {% block title %}Erreur {{ status_code }}{% endblock %}

    {% block body %}
    <h1>Oups! Il y a une erreur</h1>
    <h2>Le serveur a renvoyé une erreur "{{ status_code }} {{ status_text }}".</h2>

    <div>
        Vous essayez d'accéder à une ressource protégée.
        Si vous estimez que votre demande est légitime, veuillez nous contacter afin de trouver une solution.
    </div>

    <div>
        <ul>
            <li><a href="/">accueil</a></li>
            <li><a href="/login">authentification</a></li>
            <li><a href="/register">inscription</a></li>
        </ul>
    </div>
    {% endblock %}

Je me suis inspiré du fichier `vendor/symfony/twig-bundle/Resources/views/Exception/error.html.twig` pour créer le mien.
Vous trouverez dans le dossier `vendor/symfony/twig-bundle/Resources/views/Exception` d'autres fichiers qui pourront vous servir de base.

N'oubliez pas de consulter la section sur le « Mode debug » pour jongler entre le mode debug et sans debug (et pouvoir tester vos templates Twig).

## Migrer des entités, des contrôleurs et des formulaires de Symfony 2.8 (ou Symfony 3.4 classique) vers Symfony 3.4

Si vous avez du code Symfony 2.8, ou du code Symfony 3.4 utilisant le namespace `AppBundle`, il est assez facile de le transformer en code compatible avec Symfony 3.4 sans bundle ou avec Symfony 4.x.

Attention : quand vous faites ce type de remplacement de chaîne de caractère, cocher la case « sensible à la casse ».

Dans les entités, remplacez les occurences de :

- `AppBundle` par `App`

Dans les contrôleurs, remplacez les occurences de :

- `AppBundle` par `App`
- `App:` par `App\Entity\`

Dans les formulaires, remplacez les occurences de :

- `AppBundle` par `App`
- `appbundle` par `app`

## Doc

### Install

- [Download Symfony Framework and Components](https://symfony.com/download)
- [Framework Configuration Reference (FrameworkBundle) (Symfony 3.4 Docs)](https://symfony.com/doc/3.4/reference/configuration/framework.html#secret)

### Entity

- [Databases and the Doctrine ORM (Symfony 3.4 Docs)](https://symfony.com/doc/3.4/doctrine.html#persisting-objects-to-the-database)
- [How to Work with Doctrine Associations / Relations (Symfony 3.4 Docs)](https://symfony.com/doc/3.4/doctrine/associations.html)
- [Association Mapping - Object Relational Mapper (ORM) - Doctrine](https://www.doctrine-project.org/projects/doctrine-orm/en/latest/reference/association-mapping.html)
- [Doctrine\Common\Collections\ArrayCollection | API](https://www.doctrine-project.org/api/collections/latest/Doctrine/Common/Collections/ArrayCollection.html)

### GET, POST, FILES, SESSION, COOKIES, SERVER

- [Symfony and HTTP Fundamentals (Symfony 3.4 Docs)](https://symfony.com/doc/3.4/introduction/http_fundamentals.html)
- [The HttpFoundation Component (Symfony 3.4 Docs)](https://symfony.com/doc/3.4/components/http_foundation.html)

### Form

- [Forms (Symfony 3.4 Docs)](https://symfony.com/doc/3.4/forms.html)
- [Form Types Reference (Symfony 3.4 Docs)](http://symfony.com/doc/3.4/reference/forms/types.html)
- [EntityType Field (Symfony 3.4 Docs)](http://symfony.com/doc/3.4/reference/forms/types/entity.html)
- [Validation Constraints Reference (Symfony 3.4 Docs)](http://symfony.com/doc/3.4/reference/constraints.html)

## CSS et Javascript

@todo ajouter [Managing CSS and JavaScript (Symfony 3.4 Docs)](https://symfony.com/doc/3.4/frontend.html)

## Fixtures

- [DoctrineFixturesBundle (Symfony Bundles Docs)](https://symfony.com/doc/current/bundles/DoctrineFixturesBundle/index.html)

## Authentification

- [Security (Symfony 3.4 Docs)](https://symfony.com/doc/3.4/security.html)
- [Symfony Security: Beautiful Authentication, Powerful Authorization Video Tutorial Screencast | SymfonyCasts](https://symfonycasts.com/screencast/symfony-security)
- [Customizing the User Entity > Symfony Security: Beautiful Authentication, Powerful Authorization | SymfonyCasts](https://symfonycasts.com/screencast/symfony-security/user-entity#setting-doctrines-server-version)
- [How Does the Security access_control Work? (Symfony 3.4 Docs)](https://symfony.com/doc/3.4/security/access_control.html)
- [How to Create a Custom Authentication System with Guard (Symfony 3.4 Docs)](https://symfony.com/doc/3.4/security/guard_authentication.html)
- [How to Build a Traditional Login Form (Symfony 3.4 Docs)](https://symfony.com/doc/3.4/security/form_login_setup.html)
- [How to Customize Error Pages (Symfony Docs)](https://symfony.com/doc/current/controller/error_pages.html)
- [How to Access the User, Request, Session & more in Twig via the app Variable (Symfony Docs)](https://symfony.com/doc/current/templating/app_variable.html)
- [How to Access the User, Request, Session & more in Twig via the app Variable (Symfony Docs)](https://symfony.com/doc/current/templating/app_variable.html)
- [How to Secure any Service or Method in your Application (Symfony 3.4 Docs)](https://symfony.com/doc/3.4/security/securing_services.html)

## Deploy

- [How to Deploy a Symfony Application (Symfony 3.4 Docs)](https://symfony.com/doc/3.4/deployment.html)
- [Symfony 4 how to enable prod mode? - Stack Overflow](https://stackoverflow.com/questions/48869011/symfony-4-how-to-enable-prod-mode)
⁻ [How to Deploy a Symfony 4 Application to Production with LEMP on Ubuntu 18.04 | DigitalOcean](https://www.digitalocean.com/community/tutorials/how-to-deploy-a-symfony-4-application-to-production-with-lemp-on-ubuntu-18-04)
