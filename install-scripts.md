# Install scripts

Les install scripts sont un ensemble de petits scripts censé vous aider à administrer un serveur web.

Le code source est accessible ici [https://github.com/jibundeyare/install-scripts](https://github.com/jibundeyare/install-scripts).

## Prérequis

Certains scripts nécessitent la présence du package `pwgen`.
Si vous ne l'avez pas, installez-le avec la commande :

    apt install pwgen

## Installation

    git clone https://github.com/jibundeyare/install-scripts.git

Et pour la mise à jour un simple `git pull` suffit.

## Configuration de la sécurité

Note : vous pouvez utiliser ce script sur votre machine de dev mais il est surtout utile sur votre vps.

Ce script :

- installe fail2ban (anti brute force) et ufw (parefeu)
- renforce la configuration du serveur ssh, notamment la désactivation de connexion avec le compte root
- personnalise le port ssh
- configure fail2ban pour qu'il utilise le port ssh personnalisé
- configure ufw pour ne laisser passer que le serveur web et le serveur ssh (par le port personnalisé)

Attention : avant d'utiliser ce script, vérifiez que vous avez bien un accès à votre vps avec une clé ssh (sans mot de passe).

La configuration :

    ./configure-security.sh [nom-utilisateur] [port-ssh]

Pour le numéro de port, choisissez un nombre entre `49152` et `65535`.

Exemple :

    ./configure-security.sh johndoe 54321

Pour plus d'informations sur ssh, veuillez consulter [ssh.md](ssh.md).
Pour plus d'informations sur fail2ban, veuillez consulter la section « Sécurisation avec fail2ban » de [admin-sys.md](admin-sys.md).

## Installation de la stack AMP

Ce script installe :

- apache 2
- mariadb
- php-fpm

Il créé un dossier dans lequel seront stockés tous les projets web et un dossier contenant le site web par défaut.
Les processus apache et php-fpm seroint démarrés avec le compte utilisateur.

L'installation :

    ./install-amp.sh [nom-utilisateur] [dossier-projets] [site-web-par-défaut]

Exemple :

    ./install-amp.sh johndoe projects www

## Installation de phpMyAdmin (pma)

Ce script installe :

- phpMyAdmin (pma)

Attention : l'installation se fait à partir des sources de pma et non à partir d'un package debian.
Entre 2019 et 2020, le package debian a été retiré pour cause d'obsolescence.

Il ajoute une authentification http avant de donner accès à la page d'authentification de pma.
Il crée aussi un compte d'administrateur de BDD pour éviter d'utiliser le compte root.

L'installation :

    ./install-phpmyadmin-from-src.sh [nom-utilisateur] [administrateur-bdd] [sous-dossier-pma] [version-pma]

Exemple :

    ./install-phpmyadmin-from-src.sh johndoe dba pma_subdir 5.0.2

Après cette installation, pma devient accessible depuis l'url `http://[nom-domaine]/pma_subdir`.
Sur votre machine de dev cela donne [http://localhost/pma_subdir](http://localhost/pma_subdir) ou [http://127.0.0.1/pma_subdir](http://127.0.0.1/pma_subdir).

## Oups, j'ai oublié mon mot de passe http pour pma !

Si vous avez oublié le mot de passe de l'authentification http pour pma, pas de panique.
Ce script permet de resetter ce mot de passe.

Le reset :

    ./reset-pma-http-auth-password.sh [administrateur-bdd]

Exemple :

    ./reset-pma-http-auth-password.sh dba

## Création d'un site web

Sur votre machine de dev, plusieurs étapes sont nécessaire :

1. création d'une BDD
2. création d'un vhost et d'un pool php-fpm
3. création d'un nom de domaine local

Sur votre VPS, seules deux étapes sont nécessaire :

1. création d'une BDD
2. création d'un vhost et d'un pool php-fpm

### Création d'une BDD

Ce script crée une BDD et un nouvel utilisateur qui portent le même nom.

La création :

    ./mkdb [nom-appplication]

Exemple :

    ./mkdb foo

Après la commande, je pourrai utiliser la BDD `foo` et y accéder avec l'utilisateur `foo`.

### Création d'un vhost et d'un pool php-fpm associé

Ce script est le plus complexe des trois scripts.
Il permet de créer le vhost et le pool php-fpm qui va communiquer avec apache.

La création :

    ./mkwebsite.sh [nom-utilisateur] [dossier-projets] [dossier-projet] [nom-domaine] [template-vhost]

Le dernier paramètre est optionnel.
Si aucun paramètre n'est spécifié, c'est le template par défaut qui est choisi.
Voir la section « Les templates de vhost » ci-dessous pour plus d'infos.

Note : le paramètre `[nom-domaine]` est ignoré si un template de vhost du type `subdir` (sous-dossier) est utilisé.

Exemple sur un vps :

    ./mkwebsite.sh johndoe projects foo foo.com

Après la création, le site web est accessible depuis l'url [http://foo.com](http://foo.com).

Exemple en local :

    ./mkwebsite.sh johndoe projects foo foo.local

Après la création, le site web est accessible depuis l'url [http://foo.local](http://foo.local).

Attention : sur votre machine de dev vous devez créer un nom de domaine local pour que l'url soit reconnue.

#### Les templates de vhost

Il est possible de choisir un template de vhost parmis plusieurs choix ou de créer ses propres templates de vhost.
Le template par défaut est `template-vhost.conf`.

Il existe deux sortes de templates :

- ceux qui créent un vhost
- ceux qui créent un sous-dossier

Les templates qui créent un sous-dossier (comme pma) permettent d'héberger plusieurs sites web sans avoir de nom de domaine.

Voici la liste complète des templates :

- `template-subdir.conf` : le document root est le dossier du projet
- `template-subdir-deployer-symfony.conf` : le document root est `[dossier-projet]/current/public`
- `template-subdir-symfony.conf` : le document root est `[dossier-projet]/public`
- `template-vhost.conf` : le document root est le dossier du projet
- `template-vhost-deployer-symfony.conf` : le document root est `[dossier-projet]/current/public`
- `template-vhost-symfony.conf` : le document root est `[dossier-projets]/public`

### Création d'un nom de domaine local

Attention : pas nécessaire sur votre VPS.

Ce script ajoute un nom de domaine associé à l'adresse `127.0.0.1` dans votre fichier `/etc/hosts`.

La création :

    ./mkdomain.sh [nom-domaine]

Exemple :

    ./mkdomain.sh foo.local

Après la création, le site web est accessible depuis l'url [http://foo.local](http://foo.local).

## Suppression d'un site web

Pour la suppression d'un site web, il sufit de passer par les même étapes que lors de la création mais dans l'ordre inverse.

### Suppression d'un nom de domaine local

Attention : pas nécessaire sur votre VPS.

La suppression :

    ./rmdomain.sh [nom-domaine]

Exemple :

    ./rmdomain.sh foo.local

### Suppression d'un vhost et d'un pool php-fpm associé

Attention : ce script ne supprime aucun fichier du dossier des projets, vous ne perdrez aucune donnée.

La suppression :

    ./rmwebsite.sh [dossier-projet]

Exemple :

    ./rmwebsite.sh foo

Après suppression, le site web du dossier `foo` ne sera plus accesible (mais les fichiers seront toujours là).

### Suppression d'une BDD

Attention : par contre, le script de suppression de BDD supprime définitivement la BDD.
À vous d'en faire une copie de sauvegarde avant de la détruire.

La suppression :

    ./rmdb.sh [dossier-projet]

Exemple :

    ./rmdb.sh foo

Après suppression, l'utilisateur `foo` et la BDD `foo` auront disparu.

## Les remote tools

Ce script installe des outils de prise en main de pc à distance.

Il installe :

- teamviewer
- anydesk

Attention : ce script désactive wayland au profit de xorg.
Cela est nécessaire pour que teamviwer et anydesk puissent afficher l'écran de l'hôte.

L'installation :

    ./install-remote-tools.sh

## Les teacher tools

Attention : si vous n'êtes pas formateur, il y a peu de chance que ces outils vous intéressent.

Ce script installe des outils qui facilite le travaille des formateurs à distance.

Il installe :

- gromit (dessin sur écran)
- obs-studio (streaming vidéo)
- un curseur de souris pour gnome

L'installation :

    ./install-teacher-tools.sh

L'activation des options de thème pour formateur :

    ./teacher-theme-enable.sh

La désactivation des options de thème pour formateur :

    ./teacher-theme-disable.sh

