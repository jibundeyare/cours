# WIP Symfony 4.4 - Déploiement manuel

@WIP

## Prérequis

En local :

- git
- un repo sur github

Sur le serveur :

- la stack Apache (ou Nginx) MariaDB PHP
- git
- composer
- nodejs et npm

## Le déploiement manuel dans les grandes lignes

### Le premier déploiement

Les premières étapes doivent être faite en local, sur le poste de dev :

- si vous utilisez Apache, installez le package `apache-pack` avec composer
- commitez et poussez le code sur github avec git

Toutes les étapes suivantes se passent sur le serveur :

- créez une BDD et un utilisateur dédié
- récupérez le code du projet depuis github avec git
- installez les dépendances avec composer et npm
- compilez les dépendances front end avec webpack
- configurez les codes d'accès à la BDD et aux autres services dans le fichier `.env.local`
- créez le schéma de la BDD avec doctrine migration
- injectez les données indispensables dans la BDD avec doctrine fixtures
- créez un virtual host et un pool php fpm
- redémarrez Apache

Il n'y a plus qu'à tester.

### Les déploiements suivants

La première étape doit être faite en local, sur le poste de dev :

- commitez et poussez le code sur github avec git

Toutes les étapes suivantes se passent sur le serveur :

- récupérez le code du projet depuis github avec git
- installez les dépendances avec composer et npm
- compilez les dépendances front end avec webpack
- si vous utilisez de nouveaux services avec code d'accès, ajoutez-les dans le fichier `.env.local`
- si vous avez changé vos entités, mettez à jour le schéma de la BDD avec doctrine migration
- s'il y a de nouvelles données indispensables, injectez les dans la BDD avec doctrine fixtures
- videz le cache

Il n'y a plus qu'à tester.

## Le déploiement manuel dans le détail

### Le premier déploiement

Dans cet exemple, on admet qu'on travaille sur le projet Symfony `bar` du développeur `foo`.
L'URL du projet sur github est donc [https://github.com/foo/bar](https://github.com/foo/bar).

Les premières étapes doivent être faite en local, sur le poste de dev :

```bash
# Ceci n'est nécessaire que si vous utilisez Apache.
composer require apache-pack

# Ajoutez les fichiers, commitez puis poussez le code sur github.
git add .
git commit
git push
```

Toutes les étapes suivantes se passent sur le serveur :

```bash
# Dans cet exemple on va utiliser les install scripts pour créer la BDD mais
# vous pouvez utiliser une autre méthode.
cd ~/install-scripts
./mkdb.sh bar

# Clonez le repo github.
# Utilisez de préférence le protocole HTTPS pour éviter de laisser une clé SSH
# sur votre serveur de prod.
git clone https://github.com/foo/bar

# Installez les dépendances.
cd bar
composer install
npm install

# Compilez les dépendances front end.
npm run build

# Créez votre fichier .env.local.
# Ou utilisez la commande scp depuis un terminal en local pour copier votre
# fichier de config sur le serveur de prod.
nano .env.local

# Créez le schéma de BDD.
php bin/console doctrine:migrations:migrate

# Injectez les fixtures.
# Vous aurez certainement à adapter le nom du groupe.
# L'option --env=dev est nécessaire car normalement les fixtures ne sont
# disponibles que en environement de dev.
php bin/console doctrine:fixtures:load --env=dev --group=prod

# Créez un virtual host et un pool php fpm
cd ~/install-scripts
# Si vous avez un nom de domaine, vous pouvez créer un vhost avec un
# sous-domaine.
./mkwebsite.sh foo projects bar bar.example.com template-vhost-symfony.conf
# Si vous n'avez pas de nom de domaine, vous pouvez créer un vhost avec un
# sous-répertoire.
# Notez que dans ce cas, le nom de domaine bar.local est ignoré par le script.
./mkwebsite.sh foo projects bar bar.local template-subdir-symfony.conf

# Redémarrez Apache.
# Pas la peine, le script l'a déjà fait !
```

Il n'y a plus qu'à tester.

### Les déploiements suivants

La première étape doit être faite en local, sur le poste de dev :

```bash
# Ajoutez les fichiers, commitez puis poussez le code sur github.
git add .
git commit
git push
```

Toutes les étapes suivantes se passent sur le serveur :

- récupérez le code du projet depuis github avec git
- installez les dépendances avec composer et npm
- compilez les dépendances front end avec webpack
- si vous utilisez de nouveaux services avec code d'accès, ajoutez-les dans le fichier `.env.local`
- si vous avez changé vos entités, mettez à jour le schéma de la BDD avec doctrine migration
- s'il y a de nouvelles données indispensables, injectez-les en mode « ajout » dans la BDD avec doctrine fixtures
- videz le cache

```bash
# Mettez à jour votre repo.
cd ~/projects/bar
git pull

# Installez les dépendances.
composer install
npm install

# Compilez les dépendances front end.
npm run build

# Mettez à jour votre fichier .env.local si nécessaire.
# Ou utilisez la commande scp depuis un terminal en local pour copier votre
# fichier de config sur le serveur de prod.
nano .env.local

# Mettez à jour le schéma de BDD.
# S'il n'y a aucune mise à jour c'est pas grave, on peut quand même exécuter
# la commande.
php bin/console doctrine:migrations:migrate

!!! APPEND
# Injectez les fixtures.
# Vous aurez certainement à adapter le nom du groupe.
# L'option --env=dev est nécessaire car normalement les fixtures ne sont
# disponibles que en environement de dev.
php bin/console doctrine:fixtures:load --env=dev --group=prod

# Créez un virtual host et un pool php fpm
cd ~/install-scripts
# Si vous avez un nom de domaine, vous pouvez créer un vhost avec un
# sous-domaine.
./mkwebsite.sh foo projects bar bar.example.com template-vhost-symfony.conf
# Si vous n'avez pas de nom de domaine, vous pouvez créer un vhost avec un
# sous-répertoire.
# Notez que dans ce cas, le nom de domaine bar.local est ignoré par le script.
./mkwebsite.sh foo projects bar bar.local template-subdir-symfony.conf

# Redémarrez Apache.
# Pas la peine, le script l'a déjà fait !
```

Il n'y a plus qu'à tester.

## La commande `scp` pour copier des fichiers sur un serveur


