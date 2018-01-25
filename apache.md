# Apache

La procédure d'installation se base sur une distribution Linux Debian Stretch.

## Installation d'apache sur une box linux

    sudo apt-get update
    sudo apt-get install apache2

## Installation de php sur une box linux

    sudo apt-get install php7.0

## Fichier `.htaccess`


Pour annuler une redirection permanente, on est obligé de d'effacer le cache du navigateur. Cela veut dire qu'on n'a pas le droit à l'erreur car seul les utilisateurs du site peuvent effectuer cette action.

## Configuration de virtual hosts

## Création de certificats ssl pour le poste de développement

Cette page décrit en détail la procédure à suivre : [How To Create a Self-Signed SSL Certificate for Apache in Ubuntu 16.04 | DigitalOcean](https://www.digitalocean.com/community/tutorials/how-to-create-a-self-signed-ssl-certificate-for-apache-in-ubuntu-16-04).

Il est possible « d'automatiser » la création de la clé privée et du certificat avec la commande suivante :

    sudo openssl req -x509 -nodes -days 365 -newkey rsa:2048 -keyout /etc/ssl/private/apache-selfsigned.key -out /etc/ssl/certs/apache-selfsigned.crt -subj "/C=FR/ST=Haut de France/L=Lille/O=Foo Bar Baz/OU=Lorem ipsum/CN=192.168.33.10"

## Création de certificats ssl pour la production

## Installation de certificats et utilisation de https avec apache

## Commandes utiles

### Service apache

Démarrer apache :

    sudo service apache2 start

Arrêter apache :

    sudo service apache2 stop

Redémarrer apache :

    sudo service apache2 restart

### Site

Activer un site :

    a2ensite [nom du virtualhost]

Exemple : `a2ensite 000-default.conf`

Désactiver un site :

    a2dissite [nom du virtualhost]

Exemple : `a2dissite 000-default.conf`

### Module

Activer un module :

    sudo a2enconf [nom du module]

Exemple : `sudo a2enmod mpm_event`

Désactiver un module :

    sudo a2disconf [nom du module]

Exemple : `sudo a2dismod mpm_prefork`

### Configuration

Activer une configuration :

    sudo a2enconf [nom de la configuration]

Exemple : `sudo a2enconf php7.0-fpm`

Désactiver une configuration :

    sudo a2disconf [nom de la configuration]

Exemple : `sudo a2enconf php7.0-fpm`

## Doc

- [Welcome! - The Apache HTTP Server Project](http://httpd.apache.org/)
- [Document de référence rapide des directives - Serveur Apache HTTP Version 2.4](http://httpd.apache.org/docs/2.4/mod/quickreference.html)
- [Tutoriel du serveur HTTP Apache : fichiers .htaccess - Serveur Apache HTTP Version 2.4](http://httpd.apache.org/docs/2.4/howto/htaccess.html)
- [Authentification et autorisation - Serveur Apache HTTP Version 2.4](http://httpd.apache.org/docs/2.4/howto/auth.html)
- [Oublions mod_php - Remi Collet - PHP Tour 2016 - YouTube](https://www.youtube.com/watch?time_continue=2454&v=onSzYyv4yj8)
- [How To Create a Self-Signed SSL Certificate for Apache in Ubuntu 16.04 | DigitalOcean](https://www.digitalocean.com/community/tutorials/how-to-create-a-self-signed-ssl-certificate-for-apache-in-ubuntu-16-04)
- [HowTo: Create CSR using OpenSSL Without Prompt (Non-Interactive) - ShellHacks](https://www.shellhacks.com/create-csr-openssl-without-prompt-non-interactive/)
- [How To Secure Apache with Let's Encrypt on Ubuntu 16.04 | DigitalOcean](https://www.digitalocean.com/community/tutorials/how-to-secure-apache-with-let-s-encrypt-on-ubuntu-16-04)
