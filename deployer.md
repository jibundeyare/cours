# Deployer

*Pour en savoir plus sur les modalités de livraison d'un projet, voir [livraison.md](livraison.md).*

Deployer est une application PHP qui fonctionne dans le terminal.
Cette application permet d'automatiser le déploiement de projets Symfony (ou autre framework) sur d'autres serveurs.

**Attention : la recette de déploiement de ce cours est adaptée au déploiement de Symfony 3.4.**
Si vous voulez déployer une version antérieure ou déployer un autre framework ou application, vous devrez modifier la configuration.

## Prérequis

- un projet dans un repository git accessible via internet (Github, Framagit, Bitbucket, etc)
- une config pour l'environnement de prod (accès à la BDD, accès à un SMTP, etc)
- la commande `dep` (c-à-d deployer)
- une connexion SSH avec authentification par clé publique
- PHP (7.1+ fortement recommandé) sur votre serveur
- un `composer` installé globalement sur le serveur
- un serveur web (Apache, nginx ou autre) sur le serveur
- une base de données (BDD) avec une connexion root pour créer un utilisateur et BDD (MariaDB, PostgreSQL, ou autre) sur le serveur
- un compte root pour créer un pool php-fpm et un vhost
- un compte utilisateur qui est sudoer pour redémarrer des services (optionnel)

## Installation de la commande `dep` (c-à-d deployer)

Suivez les instructions de la documentation officielle :

- [Getting Started | Deployer – A deployment tool for php](https://deployer.org/docs/getting-started.html)
- [CLI Usage | Deployer – A deployment tool for php](https://deployer.org/docs/cli.html)

Ou dans un terminal, tapez les commandes suivantes :

    curl -LO https://deployer.org/deployer.phar
    sudo mkdir -p /usr/local/bin
    sudo mv deployer.phar /usr/local/bin/dep
    sudo chmod +x /usr/local/bin/dep

Pour terminer l'installation, il faut ajouter l'autocomplétion.
La commande suivante indique quelle commande entrer pour ajouter l'autocomplétion :

    dep autocomplete

Exemple sous Linux avec Bash :

    dep autocomplete --install | sudo tee /etc/bash_completion.d/deployer

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

## Préparation du déploiement automatique

### Création d'un fichier `.htaccess`

Si le serveur web est Apache2, il faut créer un fichier `.htaccess` pour que les routes de symfony puissent fonctionner correctement.

Depuis la racine de votre projet, lancez la commande suivante :

    composer require symfony/apache-pack

Et validez quand composer vous demande si vous voulez exécuter la recette.

### Vérification de l'accès SSH par clé publique

Avant toute chose, vous devez avoir un accès SSH au serveur.

Pour tester la connexion SSH :

    ssh server_name cat /etc/hostname

Cet utilisateur doit pouvoir redémarrer le pool php-fpm et le serveur web.
Si ça n'est pas le cas, voir la section « Attribution du droit de redémarrage du pool php-fpm et du serveur web ».

### Installation de composer sur le serveur

Voir [composer](composer.md).

### Créer un accès à la base de données (BDD) sur le serveur

Si votre application web utiliser une BDD, il lui faut un accès permettant de créer, détruire et modifier une BDD.

Connectez-vous à votre serveur en SSH puis tapez la commande suivante :

    sudo mysql -u root

Si le compte root nécessite un mot de passe, tapez plutôt la commande suivante :

    sudo mysql -u root -p

Dès que vous êtes connecté à MariaDB, créez la BDD :

    CREATE DATABASE src_symfony_3_4 DEFAULT CHARACTER SET utf8mb4 COLLATE utf8mb4_unicode_ci;

Syntaxe :

    CREATE DATABASE database_name DEFAULT CHARACTER SET utf8mb4 COLLATE utf8mb4_unicode_ci;

**Attention : reprenez le nom de votre application web pour le nom de la BDD. Par exemple, si le projet s'appelle `src-symfony-3.4`, la BDD devrait s'appeler `src_symfony_3_4`.**

Créez l'utilisateur associé à la BDD :

    CREATE USER 'src_symfony_3_4'@'localhost' IDENTIFIED BY '123';

Syntaxe :

    CREATE USER 'user_name'@'localhost' IDENTIFIED BY 'password';

*Le `user_name` est le nom d'utilisateur avec lequel vous vous connectez à la BDD (pas au serveur).*

**Attention : si possible, reprenez le nom de votre BDD pour votre le nom de votre utilisateur. Utilisez un mot de passe robuste et pas `123` comme dans l'exemple.**

Accordez tous les droits à l'utilisateur pour manipuler la BDD :

    GRANT ALL ON src_symfony_3_4.* TO 'src_symfony_3_4'@'localhost';

Syntaxe :

    GRANT ALL ON database_name.* TO 'user_name'@'localhost';

*Le `user_name` est le nom d'utilisateur avec lequel vous vous connectez à la BDD (pas au serveur).*

Mettez à jour et fermez la connexion de la BDD :

    FLUSH PRIVILEGES;
    EXIT;

## Création du pool php-fpm, création du vhost et activation du vhost sur le serveur

Il faut créer un virtual host.

Voir les fichiers gist : [jibundeyare’s gists](https://gist.github.com/jibundeyare).

Vous aurez besoin de :

- `rmwebsite.sh`
- `mkwebsite.sh`
- `template-pool.conf`
- `template-vhost.conf`
- `template-vhost-symfony.conf`

## Création automatique du fichier de config `deploy.php` sur le poste de dev

**Attention : la création automatique ne fonctionne pas avec Symfony 3.4 et plus.**

Cette manipulation est à faire sur votre poste de dev.

La commande suivante permet de générer automatiquement le fichier de config `deploy.php`.

Depuis la racine de votre projet, lancez la commande suivante :

    dep init

## Création manuelle du fichier `deploy.php` sur le poste de dev

Cette manipulation est à faire sur votre poste de dev.

### Création du fichier

#### Version alpha

Cette version du fichier `deploy.php` va vous permettre de valider que deployer fonctionne correctement en local.

Créez le fichier `deploy.php` (à la racine du projet) et ajoutez-y le code suivant :

    <?php

    namespace Deployer;

    require 'recipe/symfony4.php';

    // tasks

    desc('Test deployer');
    task('test:hello', function () {
        writeln('Hello world');
    });

Pour tester cette config, tapez la commande suivante :

    dep test:hello

Voici les résultat :

    [src-symfony-3.4]$ dep test:hello
    ➤ Executing task test:hello
    Hello world
    ✔ Ok

Bravo, vous venez d'exécuter en local la tâche `test` du fichier `deploy.php` !

#### Version beta

Cette version du fichier `deploy.php` va vous permettre de valider que deployer fonctionne correctement sur le serveur.

Créez le fichier `deploy.php` (à la racine du projet) et ajoutez-y le code suivant :

    <?php

    namespace Deployer;

    require 'recipe/symfony4.php';

    // example.com
    host('example.com')
        ->user(getenv('ssh_user'));

    // tasks

    desc('Test deployer');
    task('test:hello', function () {
        writeln('Hello world');
    });

    desc('Get server hostname');
    task('test:hostname', function () {
        $result = run('cat /etc/hostname');
        writeln("$result");
    });

*Le `user_name` est le nom d'utilisateur avec lequel vous vous connectez au serveur en SSH (pas à la BDD).*

Pour tester cette config, tapez les commandes suivantes :

    ssh_user=foo
    dep test:hostname

Voici les résultat :

    $ ssh_user=foo
    $ dep test:hostname
    ➤ Executing task test:hostname
    vps608861
    ✔ Ok

Bravo, vous venez d'exécuter sur le serveur la tâche `hostname` du fichier `deploy.php` !

#### Version de base

Cette version du fichier `deploy.php` va vous permettre de déployer votre projet.

**Attention : lisez la section suivante pour tester correctement cette config.**

Créez le fichier `deploy.php` (à la racine du projet) et ajoutez-y le code suivant :

    <?php

    namespace Deployer;

    use Symfony\Component\Console\Input\InputOption;

    require 'recipe/symfony4.php';

    option('all-fixtures', 'a', InputOption::VALUE_NONE, 'Load all fixtures');

    // @todo configure this
    project repository
    set('repository', 'https://github.com/foo/bar.git');

    // hosts

    @todo configure this
    host('example.com')
        ->stage('prod')
        ->user(getenv('ssh_user'))
        // user the web server runs as. If this parameter is not configured, deployer try to detect it from the process list.
        ->set('http_user', getenv('ssh_user'))
        // projects directory
        ->set('projects_dir', 'projects')
        // projects name
        ->set('application', 'foo')
        ->set('deploy_path', '~/{{projects_dir}}/{{application}}');

    // @todo configure this
    host('test.example.com')
        ->stage('test')
        ->user(getenv('ssh_user'))
        // user the web server runs as. If this parameter is not configured, deployer try to detect it from the process list.
        ->set('http_user', getenv('ssh_user'))
        // projects directory
        ->set('projects_dir', 'projects')
        // projects name
        ->set('application', 'foo')
        ->set('deploy_path', '~/{{projects_dir}}/{{application}}');

    // set default stage
    set('default_stage', 'prod');

    // [Optional] Allocate tty for git clone. Default value is false.
    set('git_tty', true);

    // shared files / dirs between deploys
    add('shared_files', []);
    add('shared_dirs', []);

    // writable dirs by web server
    add('writable_dirs', []);
    set('allow_anonymous_stats', false);

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
    task('test:hello', function () {
        writeln('Hello world');
    });

    desc('Get server hostname');
    task('test:hostname', function () {
        $result = run('cat /etc/hostname');
        writeln("$result");
    });

    desc('Copy .env.{{stage}}.local as .env.local');
    task('deploy:env', function () {
        upload('.env.{{stage}}.local', '~/{{projects_dir}}/{{application}}/shared/.env.local');
    });

    desc('Clean git files');
    task('clean:git-files', function () {
        run('rm -fr ~/{{projects_dir}}/{{application}}/current/.git');
    });

    desc('Load fixtures');
    task('fixtures:load', function () {
        $allFixtures = null;

        if (input()->hasOption('all-fixtures')) {
            $allFixtures = input()->getOption('all-fixtures');
        }

        if ($allFixtures) {
            writeln("Loading all fixtures");
            $result = run('{{bin/console}} doctrine:fixtures:load --no-interaction --purge-with-truncate');
            writeln("$result");
        } else {
            if (get('stage') == 'prod') {
                writeln("Loading prod fixtures");
                $result = run('{{bin/console}} doctrine:fixtures:load --no-interaction --group=prod --append');
                writeln("$result");
            } else { // get('stage') == 'test'
                writeln("Loading prod fixtures");
                $result = run('{{bin/console}} doctrine:fixtures:load --no-interaction --group=prod --purge-with-truncate');
                writeln("$result");
                writeln("Loading dev fixtures");
                $result = run('{{bin/console}} doctrine:fixtures:load --no-interaction --group=dev --append');
                writeln("$result");
            }
        }
    });

    desc('Rollback database');
    task('database:rollback', function () {
        $options = '--allow-no-migration';
        if (get('migrations_config') !== '') {
            $options = sprintf('%s --configuration={{release_path}}/{{migrations_config}}', $options);
        }
        run(sprintf('{{bin/console}} doctrine:migrations:migrate prev %s', $options));
    });

    after('deploy', 'clean:git-files');

**Attention : il y a un peu configuration à faire avant de pouvoir tester.**

Quel que soit l'environnement vers lequel vous déployez, vous devez spécifier la source du projet (c-à-d où se trouve le repo git) :

- `repository` : c'est l'adresse du repository git contenant le code de votre projet

Pour chaque host, vous devez configurer :

- `host()` : vous devez renseigner le nom de domaine ou l'adresse IP du serveur
- `stage()` : vous devez préciser sde quel environnement il s'agit (prod, test, etc)
- `projects_dir` : c'est le dossier dans lequel se trouvent les projets web sur votre serveur
- `application` : c'est le dossier de votre application sur le serveur

## Déploiement

### Nom d'utilisateur pour la connexion SSH

Avant de lancer la commande de déploiement, vous devez spécifier le nom d'utilisateur avec lequel vous vous connecter au server.
Pour des raison de sécurité, nous faisons ça en utilisant une variable d'environnement.
En effet, si vous ne stockez pas l'information du nom d'utilisateur dans le fichier `deploy.php`, vous pouvez le commiter avec git sans craindre de publier des informations confidentielles.

Si vous voulez vous connecter avec l'utilisateur foo, vous devdez taper :

    ssh_user=foo

Note : avec Windows, vous devez rajoutez `set` devant la commande :

    set ssh_user=foo

**Note : Une fois que vous avez défini cette variable d'environnement, à moins de fermer votre terminal, vous n'avez plus à le refaire avant chaque commande de déploiement.**
Dans les commandes des sections ci-dessous, je répète la commande mais en réalité ce n'est donc pas nécessaire.

### Nom de domaine ou adresse IP du serveur pour la connexion SSH (optionnel)

Si vous souhaitez aussi rendre le nom de l'hôte confidentiel et le spécifier en utilisant une variable d'environnement faites les modifications suivantes.

- dans `deploy.php`, remplacez `host('example.com')` par `host(getenv('ssh_host'))`
- dans `deploy.php`, pour éviter se déployer par erreur sur le serveur de prod, définissez une valeur vide pour l'environnement poar défaut :

        set('default_stage', '');

   vous devrez alors toujours spécifier la plateforme de déploiement.
   par exemple pour la prod :

        dep deploy prod

- avant de lancer le déploiment, définissez le nom de domaine ou l'adresse IP du serveur avec la commande :

        ssh_host=example.com

  ou avec Windows :

    set ssh_host=example.com

### Le premier déploiement

**Attention : la commande `dep deploy:env` ne peut fonctionner que si vous avez bien créé le fichier `.env.prod.local` (la config pour l'environnement de prod). Si ce n'est pas le cas, voir la section « Création de la config pour l'environnement de prod » dans [symfony-3.4.md](symfony-3.4.md).**

Pour lancer le déploiement, dans un terminal à la racine du projet, tapez :

    ssh_user=foo
    dep deploy:prepare
    dep deploy:env
    dep deploy

Voici le résultat :

    $ ssh_user=foo
    $ dep deploy:prepare
    ✔ Executing task deploy:prepare
    $ dep deploy:env
    ✔ Executing task deploy:env
    $ dep deploy
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
    ✔ Executing task clean:git-files

### Chargement de toutes les fixtures, quelque soit l'environnement

Cette commande charge toutes les fixtures, quelque soit leur groupe.

**Note : cette commande est utile si vos fixtures n'appartiennent à aucun groupe particulier.**

Si vous voulez charger toutes les fixtures, en environnement de prod, tapez la commande suivante :

    ssh_user=foo
    dep fixtures:load --all-fixtures

Si vous voulez charger toutes les fixtures, en environnement de test, tapez la commande suivante :

    ssh_user=foo
    dep fixtures:load --all-fixtures test

### Chargement des fixtures pour l'environnement de prod

Cette commande ne charge que les fixtures du groupe `required`.

**Note : en prod, par précaution, aucune données n'est supprimée avant le chargement des fixtures.**

Si vous déployez vers un environnement de prod pour la première fois, tapez la commande suivante :

    ssh_user=foo
    dep fixtures:load

**Note : si vous utilisez cette commande, vous devez avoir des fixtures appartenant au groupe `required`.**
Sinon vous verrez l'erreur :

    [ERROR] Could not find any fixture services to load in the groups (prod).

### Chargement des fixtures pour l'environnement de test

Cette commande charge d'abord les fixtures du groupe `required` puis celles du groupe `test`.

Si vous déployez vers un environnement de test (la première fois ou les autres fois), tapez la commande suivante :

    ssh_user=foo
    dep fixtures:load test

**Note : si vous utilisez cette commande, vous devez avoir des fixtures appartenant au groupe `required` et au groupe `test`.**
Sinon vous verrez l'erreur :

    [ERROR] Could not find any fixture services to load in the groups (prod).

### Les déploiements suivants

Dans un terminal à la racine du projet, tapez :

    ssh_user=foo
    dep deploy

### Oups, j'ai fait de la m...e, je reviens en arrière (rollback)

Dans un terminal à la racine du projet, tapez :

    ssh_user=foo
    dep rollback

Voici le résultat :

    $ ssh_user=foo
    $ dep rollback
    ✔ Executing task rollback

Si la BDD doit aussi être remise dans son état antérieur :

    ssh_user=foo
    dep database:rollback

Voici le résultat :

    $ ssh_user=foo
    $ dep database:rollback
    ✔ Executing task database:rollback

### Déploiement vers un environnement autre que prod (test, staging, etc)

Assurez-vous d'abord que vous avez le fichier config correspondant à l'environnement :

- `.env.test.local` pour l'environnement de test
- `.env.staging.local` pour l'environnement de staging
- etc

Ensuite retirez les commentaires du bloc de code concernés dans `deploy.php` et adaptez-le.

Par exemple pour l'environnement de test :

    host('test.example.com')
        ->stage('test')
        ->user('foo')
        ->set('deploy_path', '~/{{projects_dir}}/{{application}}');

Ou par exemple pour l'environnement de staging :

    host('staging.example.com')
        ->stage('staging')
        ->user('foo')
        ->set('deploy_path', '~/{{projects_dir}}/{{application}}');

Maintenant, pour le déploiement, ajoutez le nom de l'environnement à la fin des commandes.

Par exemple, pour déployer vers l'environnement de test, tapez :

    ssh_user=foo
    dep deploy:prepare test
    dep deploy:env test
    dep deploy test

Ou pour déployer vers l'environnement de staging, tapez :

    ssh_user=foo
    dep deploy:prepare staging
    dep deploy:env staging
    dep deploy staging

## Doc

- [How to Deploy a Symfony Application (Symfony 3.4 Docs)](https://symfony.com/doc/3.4/deployment.html)
- [Deployer – A deployment tool for php](https://deployer.org/)
- [Getting Started | Deployer – A deployment tool for php](https://deployer.org/docs/getting-started.html)
- [GitHub - deployphp/deployer: A deployment tool written in PHP with support for popular frameworks out of the box](https://github.com/deployphp/deployer)
- [deployphp/recipes: Deployer Recipes](https://github.com/deployphp/recipes)
- [How to Automatically Deploy Laravel Applications with Deployer | DigitalOcean](https://www.digitalocean.com/community/tutorials/automatically-deploy-laravel-applications-deployer-ubuntu)
- [jibundeyare’s gists](https://gist.github.com/jibundeyare)

