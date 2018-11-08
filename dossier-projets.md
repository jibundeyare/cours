# Dossier projets

Remplacez `[user-name]` par votre nom d'utilisateur dans la procédure.

Exemple :

Si votre nom d'utilisateur est `toto`,

    DocumentRoot /home/[user-name]/projets

devient

    DocumentRoot /home/toto/projets

## Procédure

    mkdir projets

    sudo nano /etc/apache2/envvars
    # remplacement à faire à la main :
    # export APACHE_RUN_USER=www-data
    # export APACHE_RUN_GROUP=www-data
    # ->
    # export APACHE_RUN_USER=[user-name]
    # export APACHE_RUN_GROUP=[user-name]

    # sauver le résultat puis sortir

    sudo cp /etc/apache2/sites-available/000-default.conf /etc/apache2/sites-available/000-projets.conf
    sudo nano /etc/apache2/sites-available/000-projets.conf
    # remplacement à faire à la main :
    # DocumentRoot /var/www/html
    # ->
    # DocumentRoot /home/[user-name]/projets

    # ajouter ceci après la ligne `DocumentRoot ...`
    # <Directory /home/[user-name]/projets/>
    #     Options Indexes FollowSymLinks
    #     AllowOverride All
    #     Require all granted
    # </Directory>

    # sauver le résultat puis sortir

    sudo a2dissite 000-default
    sudo a2ensite 000-projets
    systemctl restart apache2

