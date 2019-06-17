# PhpMyAdmin

## Install

Connexion avec le compte root :

    sudo apt -y install phpmyadmin
    echo "UPDATE user SET plugin='' WHERE user='root';" | sudo mysql
    echo "FLUSH PRIVILEGES;" | sudo mysql
    ln -s /etc/phpmyadmin/apache.conf /etc/apache2/conf-available/phpmyadmin.conf
    a2enconf phpmyadmin.conf
    # @warning do not enable this on ploduction platform
    sudo sed -i "s/\\/\\/ \$cfg\\['Servers'\\]\\[\$i\\]\\['AllowNoPassword'\\] = TRUE;/\$cfg['Servers'][\$i]['AllowNoPassword'] = TRUE;/" /etc/phpmyadmin/config.inc.php
    sudo systemctl restart apache2.service

Connexion avec le phpmyadmin :

    sudo apt -y install phpmyadmin
    echo "GRANT ALL PRIVILEGES ON *.* TO 'phpmyadmin'@'localhost' WITH GRANT OPTION;" | sudo mysql
    echo "FLUSH PRIVILEGES;" | sudo mysql
    ln -s /etc/phpmyadmin/apache.conf /etc/apache2/conf-available/phpmyadmin.conf
    a2enconf phpmyadmin.conf
    sudo systemctl restart apache2.service

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

