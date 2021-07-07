# Wordpress - WP CLI

WP CLI est un outil qui permet de manipuler un site Wordpress depuis le terminal.

## Installation

On télécharge le binaire et on l'installe :

```bash
curl -O https://raw.githubusercontent.com/wp-cli/builds/gh-pages/phar/wp-cli.phar
chmod +x wp-cli.phar
sudo mv wp-cli.phar /usr/local/bin/wp
```

Testez pour voir si la commande est bien installée :

```bash
wp --info
```

Si vous optenez quelque chose comme ça, c'est que ça fonctionne :

```bash
$ wp --info
OS:	Linux 4.19.0-14-amd64 #1 SMP Debian 4.19.171-2 (2021-01-30) x86_64
Shell:	/bin/bash
PHP binary:	/usr/bin/php7.4
PHP version:	7.4.16
php.ini used:	/etc/php/7.4/cli/php.ini
MySQL binary:	/usr/bin/mysql
MySQL version:	mysql  Ver 15.1 Distrib 10.3.27-MariaDB, for debian-linux-gnu (x86_64) using readline 5.2
SQL modes:	
WP-CLI root dir:	phar://wp-cli.phar/vendor/wp-cli/wp-cli
WP-CLI vendor dir:	phar://wp-cli.phar/vendor
WP_CLI phar path:	/home/popschool
WP-CLI packages dir:	
WP-CLI global config:	
WP-CLI project config:	
WP-CLI version:	2.5.0
```

## Mise à jour de WP CLI

```bash
sudo wp cli update
```

## Installer un nouveau Wordpress

Nous allons créer un projet nommé "My WP Project".

Avant totue chose, il vous faut une BDD.

On va la créer avec les install scripts :

```bash
cd ~/install-scripts
./mkdb.sh my_wp_project
```

Je pars du principe que vos projets sont installés dans le dossier `projects` de votre home :

```bash
cd ~/projects
mkdir my-wp-project
cd my-wp-project
wp core download --locale=fr_FR
wp config create --dbname=my_wp_project --dbuser=my_wp_project --dbpass=123 --locale=fr_FR
wp core install --url=localhost:8000 --title="My WP Project" --admin_user=admin --admin_password=123 --admin_email=info@example.com
```

Le nom d'utilisateur `admin_user` et le mot de passe `admin_password` vont vous permettre de vous connecter au backoffice.

L'adresse email `admin_email` doit être une adresse joignable car Wordpress peut vous envoyer des emails dans certains cas.

Lancez le serveur web de dev :

```bash
php -S localhost:8000
```

Puis testez le front [http://localhost:8000](http://localhost:8000) et le back [http://localhost:8000/wp-admin/](http://localhost:8000/wp-admin/).

