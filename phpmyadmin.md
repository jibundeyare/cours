# phpMyAdmin

Je propose un ensemble de scripts pour faciliter l'installation de phpMyAdmin et la gestion de base de données avec MariaDB sur Debian : [https://github.com/jibundeyare/install-scripts](https://github.com/jibundeyare/install-scripts).

## Export d'une base de données

Cochez les options suivantes :

### Méthode d'exportation

- Personnalisée, afficher toutes les options possibles

### Options spécifiques au format

- Désactiver la vérification des clés étrangères

### Options de création d'objets

- Ajouter une instruction DROP TABLE / VIEW / PROCEDURE / FUNCTION / EVENT / TRIGGER
- IF NOT EXISTS (moins efficace car les index seront générés lors de la création de la table)

## Doc

- [debian - debconf selections for phpmyadmin unattended installation with no webserver installed and no dbconfig-common - Stack Overflow](https://stackoverflow.com/questions/30741573/debconf-selections-for-phpmyadmin-unattended-installation-with-no-webserver-inst)
- [apache2 - How to solve the phpmyadmin not found issue after upgrading php and apache? - Ask Ubuntu](https://askubuntu.com/questions/387062/how-to-solve-the-phpmyadmin-not-found-issue-after-upgrading-php-and-apache)

