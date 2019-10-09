# Deployer

*Pour en savoir plus sur les modalités de livraison d'un projet, voir [livraison.md](livraison.md).*

Deployer est une application PHP qui fonctionne dans le terminal.
Cette application permet d'automatiser le déploiement de projets Symfony (ou autre framework) sur d'autres serveurs.

## Prérequis

- la commande deployer
- une connexion SSH avec authentification par clé publique
- PHP (7.1+ fortement recommandé)
- un `composer` installé globalement sur le serveur
- un serveur web (Apache, nginx ou autre)
- une base de données (BDD) avec une connexion root pour créer un utilisateur et BDD (MariaDB, PostgreSQL, ou autre)
- un compte root pour créer un pool php-fpm et un vhost
- un compte utilisateur qui a le droit de redémarrage du pool php-fpm et du serveur web

## Les étapes du déploiement

### Le premier déploiement

1. En local : Vérification de l'accès SSH
2. Sur le serveur : création de la BDD
3. Sur le serveur : création du user dans la BDD
4. Sur le serveur : création du pool php-fpm
5. Sur le serveur : création du vhost
6. Sur le serveur : activation du vhost
7. Sur le serveur : attribution du droit de rédamerrer le pool php-fpm et le serveur web
7. En local : préparation des fichiers de config pour la production (`.htaccess`, `.env`)
8. En local : déploiement avec deployer

### Les autres déploiements

1. En local : déploiement avec deployer

### Oups, j'ai fait de la m...e, je reviens en arrière (rollback)

1. En local : faire un rollback des fichiers
2. En local : s'il y a lieu, faire un rollback de la BDD

## La création d'un fichier `.htaccess`

Si le serveur web est Apache2, il faut créer un fichier `.htaccess` pour que les routes de symfony puissent fonctionner correctement.

Depuis la racine de votre projet, lancez la commande suivante :

    composer require symfony/apache-pack

## Créer le fichier d'environnement de production `.env.prod`

Créez le fichier à la racine du projet et insérez le code suivant :

    APP_ENV=prod
    APP_DEBUG=0
    APP_SECRET=3b66278912b18bc0e3e6d96e6e33b472
    DATABASE_URL=mysql://src_symfony_3_4:123@127.0.0.1:3306/src_symfony_3_4

Adaptez la valeur de la chaîne de caractère `DATABASE_URL` avec celles de votre BDD :

    DATABASE_URL=mysql://user:password@server:port/database

## Le `APP_SECRET` de `.env`

Voir [Symfony `APP_SECRET`](symfony-app-secret.md).

## Installation de deployer

Dans un terminal, tapez les commandes suivantes :

    curl -LO https://deployer.org/deployer.phar
    sudo mv deployer.phar /usr/local/bin/dep
    sudo chmod +x /usr/local/bin/dep

Pour terminer l'installation, il faut ajouter l'autocomplétion.
La commande suivante indique quelle commande entrer pour ajouter l'autocomplétion :

    dep autocomplete

Exemple sous Linux avec Bash :

    dep autocomplete --install | sudo tee /etc/bash_completion.d/deployer

## Vérification de l'accès SSH

Avant toute chose, vous devez avoir un accès SSH au serveur.

Pour tester la connexion SSH :

    ssh server_name cat /etc/hostname

Cet utilisateur doit pouvoir redémarrer le pool php-fpm et le serveur web.
Si ça n'est pas le cas, voir la section « Attribution du droit de redémarrage du pool php-fpm et du serveur web ».

## Installation de composer

Voir [composer](composer.md).

## Créer un accès à la base de données (BDD)

Si votre application web utiliser une BDD, il lui faut un accès permettant de créer, détruire et modifier une BDD.

Connectez-vous à votre serveur en SSH puis tapez la commande suivante :

    mysql -u root

Si le compte root nécessite un mot de passe, tapez plutôt la commande suivante :

    mysql -u root -p

Dès que vous êtes connecté à MariaDB, créez la BDD :

    CREATE DATABASE src_symfony_3_4 DEFAULT CHARACTER SET utf8mb4 COLLATE utf8mb4_unicode_ci;

**Attention : reprenez le nom de votre application web pour le nom de la BDD. Par exemple, si le projet s'appelle `src-symfony-3.4`, la BDD devrait s'appeler `src_symfony_3_4`.**

Créez l'utilisateur associé à la BDD :

    CREATE USER 'src_symfony_3_4'@'localhost' IDENTIFIED BY '123';

**Attention : si possible, reprenez le nom de votre BDD pour votre le nom de votre utilisateur. Utilisez un mot de passe robuste et pas `123` comme dans l'exemple.**

Accordez tous les droits à l'utilisateur pour manipuler la BDD :

    GRANT ALL ON src_symfony_3_4.* TO 'src_symfony_3_4'@'localhost';

Mettez à jour et fermez la connexion de la BDD :

    FLUSH PRIVILEGES;
    EXIT;

## Création du pool php-fpm, création du vhost et activation du vhost

Il faut créer un virtual host.
Voir les fichiers gist : [jibundeyare’s gists](https://gist.github.com/jibundeyare).

## Attribution du droit de redémarrage du pool php-fpm et du serveur web

Dans un terminal, tapez la commande suivante :

    sudo visudo -f /etc/sudoers.d/user_name

Insérez le code suvant :

    user_name ALL=(ALL) NOPASSWD: /bin/systemctl reload apache2,/bin/systemctl reload php7.3-fpm

Puis enregistrer le résultat avec `ctrl o` (la lettre Ô, pas le zéro) en sortez de l'éditeur nano avec `ctrl x`.

**Attention : pensez à remplacer `user_name` par le vrai nom d'utilisateur.**

## Création automatique du fichier de config `deploy.php`

La commande suivante permet de générer automatiquement le fichier de config `deploy.php`.

Depuis la racine de votre projet, lancez la commande suivante :

    dep init

**Attention : malheureusement cette configuration automatique ne fonctionne pas avec Symfony 3.4 et plus.**

## Configuration manuelle du fichier `deploy.php`

### Création du fichier


### Version alpha

Créez le fichier `deploy.php` (à la racine du projet) et ajoutez-y le code suivant :

    <?php

    namespace Deployer;

    require 'recipe/symfony4.php';

    // tasks

    task('test', function () {
        writeln('Hello world');
    });

Pour tester cette config, tapez la commande suivante :

    dep test

Voici les résultat :

    [src-symfony-3.4]$ dep test
    ➤ Executing task test
    Hello world
    ✔ Ok

### Version beta

Créez le fichier `deploy.php` (à la racine du projet) et ajoutez-y le code suivant :

    <?php

    namespace Deployer;

    require 'recipe/symfony4.php';

    // example.com
    host('example.com')
        ->user('user_name');

    // tasks

    task('test', function () {
        writeln('Hello world');
    });

    task('hostname', function () {
        $result = run('cat /etc/hostname');
        writeln("$result");
    });

Pour tester cette config, tapez la commande suivante :

    dep hostname

Voici les résultat :

    [src-symfony-3.4]$ dep hostname
    ➤ Executing task hostname
    vps608861
    ✔ Ok

### Version de base

Créez le fichier `deploy.php` (à la racine du projet) et ajoutez-y le code suivant :

    <?php

    namespace Deployer;

    require 'recipe/symfony4.php';

    // set default stage
    set('default_stage', 'prod');

    // projects directory
    set('projects_dir', 'projects');

    // project name
    set('application', 'src-symfony-3.4');

    // project repository
    set('repository', 'https://github.com/jibundeyare/src-symfony-3.4.git');

    // [Optional] Allocate tty for git clone. Default value is false.
    set('git_tty', true);

    // shared files / dirs between deploys
    add('shared_files', []);
    add('shared_dirs', []);

    // writable dirs by web server
    add('writable_dirs', []);
    set('allow_anonymous_stats', false);

    // hosts

    // example.com
    host('example.com')
        ->stage('prod')
        ->user('user_name')
        ->set('deploy_path', '~/{{projects_dir}}/{{application}}');

    // user the web server runs as. If this parameter is not configured, deployer try to detect it from the process list.
    set('http_user', 'user_name');

    // writable mode
    //
    // - acl (default) use setfacl for changing ACL of dirs.
    // - chmod use unix chmod command,
    // - chown use unix chown command,
    // - chgrp use unix chgrp command,
    set('writable_mode', 'chmod');

    // whether to use sudo with writable command. Default to false.
    set('writable_use_sudo', false);

    // tasks

    // [optional] if deploy fails automatically unlock.
    after('deploy:failed', 'deploy:unlock');

    // migrate database before symlink new release.
    before('deploy:symlink', 'database:migrate');

    desc('Test deployer');
    task('test', function () {
        writeln('Hello world');
    });

    desc('Get server hostname');
    task('hostname', function () {
        $result = run('cat /etc/hostname');
        writeln("$result");
    });

    desc('Copy .env.prod as .env.local');
    task('env:prod', function () {
        upload('.env.prod', '~/{{projects_dir}}/{{application}}/shared/.env.local');
    });

    desc('Reload services');
    task('services:reload', function () {
        run('sudo /bin/systemctl reload php7.3-fpm');
        run('sudo /bin/systemctl reload apache2');
    });

    desc('Rollback database');
    task('database:rollback', function () {
        $options = '--allow-no-migration';
        if (get('migrations_config') !== '') {
            $options = sprintf('%s --configuration={{release_path}}/{{migrations_config}}', $options);
        }
        run(sprintf('{{bin/console}} doctrine:migrations:migrate prev %s', $options));
    });

    after('deploy', 'services:reload');

**Attention : pensez à adapter les valeurs suivantes à votre cas de figure :**

- `application` : c'est le dossier dans lequel votre application doit être installée
- `repository` : c'est l'adresse du repository git contenant le code de votre projet
- `host` : c'est le nom de domaine ou l'adresse IP de votre serveur
- `user_name` : c'est le nom d'utilisateur avec lequel vous vous connectez en SSH

**Attention : lisez la section suivante pour tester cette config.**

## Déploiement

### Le premier déploiement

Dans un terminal à la racine du projet, tapez :

    dep deploy:prepare
    dep env:prod
    dep deploy

Voici le résultat :

    [src-symfony-3.4]$ dep deploy:prepare
    ✔ Executing task deploy:prepare
    [src-symfony-3.4]$ dep env:prod
    ✔ Executing task env:prod
    [src-symfony-3.4]$ dep deploy
    ✈︎ Deploying master on popschool-lens.fr
    ✔ Executing task deploy:prepare
    ✔ Executing task deploy:lock
    ✔ Executing task deploy:release
    ➤ Executing task deploy:update_code
    Clonage dans '/home/daishi/projects/src-symfony-3.4/releases/1'...
    remote: Enumerating objects: 7, done.
    remote: Counting objects: 100% (7/7), done.
    remote: Compressing objects: 100% (7/7), done.
    remote: Total 182 (delta 0), reused 3 (delta 0), pack-reused 175
    Réception d'objets: 100% (182/182), 92.76 KiB | 664.00 KiB/s, fait.
    Résolution des deltas: 100% (60/60), fait.
    Connection to 54.38.189.100 closed.
    ✔ Ok
    ✔ Executing task deploy:shared
    ➤ Executing task deploy:vendors
    ✔ Executing task deploy:writable
    ✔ Executing task deploy:cache:clear
    ✔ Executing task deploy:cache:warmup
    ✔ Executing task database:migrate
    ✔ Executing task deploy:symlink
    ✔ Executing task deploy:unlock
    ✔ Executing task cleanup
    Successfully deployed!
    ✔ Executing task services:reload

### Les autres déploiements

Dans un terminal à la racine du projet, tapez :

    dep deploy

### Oups, j'ai fait de la m...e, je reviens en arrière (rollback)

Dans un terminal à la racine du projet, tapez :

    dep rollback

Voici le résultat :

    [src-symfony-3.4]$ dep rollback
    ✔ Executing task rollback

Si la BDD doit aussi être remise dans son état antérieur :

    dep database:rollback

Voici le résultat :

    [src-symfony-3.4]$ dep database:rollback
    ✔ Executing task database:rollback

## Doc

- [How to Deploy a Symfony Application (Symfony 3.4 Docs)](https://symfony.com/doc/3.4/deployment.html)
- [Deployer – A deployment tool for php](https://deployer.org/)
- [GitHub - deployphp/deployer: A deployment tool written in PHP with support for popular frameworks out of the box](https://github.com/deployphp/deployer)
- [deployphp/recipes: Deployer Recipes](https://github.com/deployphp/recipes)
- [How to Automatically Deploy Laravel Applications with Deployer | DigitalOcean](https://www.digitalocean.com/community/tutorials/automatically-deploy-laravel-applications-deployer-ubuntu)
- [jibundeyare’s gists](https://gist.github.com/jibundeyare)
