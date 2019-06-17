# Apache

La procédure d'installation se base sur une distribution Linux Debian Stretch.

## Installation d'Apache

    sudo apt-get install apache2

## Configuration d'un virtual host

### Étapes sur le serveur

- création du fichier virtual host pour le site web
- création d'un dossier pour le site web
- activation du site
- rechargement de la configuration d'Apache

### Étapes en local (c-à-d sur le poste de développement)

- mêmes étapes que sur le serveur
- configuration du nom de domaine dans le fichier `/etc/hosts`

## Fichier `.htaccess`






Pour annuler une redirection permanente, on est obligé de d'effacer le cache du navigateur. Cela veut dire qu'on n'a pas le droit à l'erreur car seul les utilisateurs du site peuvent effectuer cette action.

## Création de certificats SSL pour le poste de développement

Cette page décrit en détail la procédure à suivre : [How To Create a Self-Signed SSL Certificate for Apache in Ubuntu 16.04 | DigitalOcean](https://www.digitalocean.com/community/tutorials/how-to-create-a-self-signed-ssl-certificate-for-apache-in-ubuntu-16-04).

Il est possible « d'automatiser » la création de la clé privée et du certificat avec la commande suivante :

    sudo openssl req -x509 -nodes -days 365 -newkey rsa:2048 -keyout /etc/ssl/private/apache-selfsigned.key -out /etc/ssl/certs/apache-selfsigned.crt -subj "/C=FR/ST=Haut de France/L=Lille/O=Foo Bar Baz/OU=Lorem ipsum/CN=192.168.33.10"

## Création de certificats SSL pour le poste de production

## Utilisation de FastCGI et PHP-FPM

## Commandes utiles

### Service Apache

Démarrer Apache :

    sudo systemctl start apache2

Arrêter Apache :

    sudo systemctl stop apache2

Redémarrer Apache :

    sudo systemctl restart apache2

Recharger la configuration d'Apache :

    sudo systemctl reload apache2

### Site

Activer un site :

    sudo a2ensite [nom du virtualhost]

Exemple : `a2ensite 000-default.conf`

Désactiver un site :

    sudo a2dissite [nom du virtualhost]

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
- [Modules multi-processus (MPMs) - Serveur Apache HTTP Version 2.4](http://httpd.apache.org/docs/2.4/mpm.html)
- [Document de référence rapide des directives - Serveur Apache HTTP Version 2.4](http://httpd.apache.org/docs/2.4/mod/quickreference.html)
- [Tutoriel du serveur HTTP Apache : fichiers .htaccess - Serveur Apache HTTP Version 2.4](http://httpd.apache.org/docs/2.4/howto/htaccess.html)
- [Authentification et autorisation - Serveur Apache HTTP Version 2.4](http://httpd.apache.org/docs/2.4/howto/auth.html)
- [Oublions mod_php - Remi Collet - PHP Tour 2016 - YouTube](https://www.youtube.com/watch?time_continue=2454&v=onSzYyv4yj8)
- [Ma station de travail PHP - Remi's RPM repository - Blog](https://blog.remirepo.net/post/2016/04/16/Ma-station-de-travail-PHP)
- [How To Create a Self-Signed SSL Certificate for Apache in Ubuntu 16.04 | DigitalOcean](https://www.digitalocean.com/community/tutorials/how-to-create-a-self-signed-ssl-certificate-for-apache-in-ubuntu-16-04)
- [HowTo: Create CSR using OpenSSL Without Prompt (Non-Interactive) - ShellHacks](https://www.shellhacks.com/create-csr-openssl-without-prompt-non-interactive/)
- [Let's Encrypt - Free SSL/TLS Certificates](https://letsencrypt.org/)
- [Certificat SSL Let’s Encrypt gratuit avec votre hébergement - OVH](https://www.ovh.com/fr/hebergement-web/ssl_mutualise.xml)
- [Free TLS/SSL certificates with Let's Encrypt and Gandi - Nom de domaine et hébergement cloud - Gandi.net](https://v4.gandi.net/news/en/2016-01-12/6677-free_tlsssl_certificates_with_lets_encrypt_and_gandi/)
- [How To Secure Apache with Let's Encrypt on Ubuntu 16.04 | DigitalOcean](https://www.digitalocean.com/community/tutorials/how-to-secure-apache-with-let-s-encrypt-on-ubuntu-16-04)

