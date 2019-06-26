# phpMyAdmin

## Install

Cette méthode d'installation permet de se connecter à phpMyAdmin avec le compte `phpmyadmin`.

    sudo apt -y install phpmyadmin
    echo "GRANT ALL PRIVILEGES ON *.* TO 'phpmyadmin'@'localhost' WITH GRANT OPTION;" | sudo mysql
    echo "FLUSH PRIVILEGES;" | sudo mysql
    sudo ln -s /etc/phpmyadmin/apache.conf /etc/apache2/conf-available/phpmyadmin.conf
    sudo a2enconf phpmyadmin.conf
    sudo systemctl restart apache2.service

**Attention** : en prod, il est fortement **déconseillé** d'activer la connexion avec le compte `root`.

## Ajouter une authentification HTTP pour améliorer la sécurité

L'authentification HTTP ajoute une étape supplémentaire avant de pouvoir accéder à phpMyAdmin.
Dès qu'une page de phpMyAdmin est demandée, un login et un mot de passe (indépendants de phpMyAdmin) sont demandés.

Remplacez `user` par un nom d'utilisateur que vous aurez choisi puis lancez cette commande :

    sudo htpasswd -c /etc/phpmyadmin/htpasswd user

Le mot de passe que vous voudrez utiliser vous sera demandé et enregistré dans le fichier `/etc/phpmyadmin/htpasswd`.

Ensuite, ces commandes `sed` vont activer l'authentification HTTP dans la configuration du vhost de phpMyAdmin :

    sudo sed -i "9i\AuthType Basic" /etc/apache2/apache2.conf
    sudo sed -i "10i\AuthName \"phpMyAdmin\"" /etc/apache2/apache2.conf
    sudo sed -i "11i\AuthUserFile /etc/phpmyadmin/htpasswd" /etc/apache2/apache2.conf
    sudo sed -i "12i\Require valid-user" /etc/apache2/apache2.conf
    sudo sed -i "13i\\\\" /etc/apache2/apache2.conf

Ces lignes permettent juste d'insérer le code suivant au bon endroit (à la ligne 9) dans le fichier `/etc/phpmyadmin/apache2.conf` :

    AuthType Basic
    AuthName "phpMyAdmin"
    AuthUserFile /etc/phpmyadmin/htpasswd
    Require valid-user

Note : pour totalement blinder phpMyAdmin, il faudrait en plus activer le protocole TLS (anciennement SSL) qui crypte les communications entre un client et un serveur web.
Avis aux amateurs d'admin sys.

## Export

Dans chacune des sections, cocher les options suivantes.

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

