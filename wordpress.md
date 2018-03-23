# Wordpress

> WordPress est un système de gestion de contenu (SGC ou content management system (CMS) en anglais) gratuit, libre et open-source. Ce logiciel écrit en PHP repose sur une base de données MySQL et est distribué par l'entreprise américaine Automattic.
> [WordPress — Wikipédia](https://fr.wikipedia.org/wiki/WordPress)

- [Français — WordPress](https://fr.wordpress.org/)
- [Français « Téléchargement — WordPress](https://fr.wordpress.org/telechargement/)
- [Automattic](https://automattic.com/)

## Métiers du web

- le développeur crée des applications
- l'administrateur système s'occupe des plateformes sur lesquelles tournent les applications
- l'administrateur réseau s'occupe de l'organisation de tous les réseaux d'une structure
- l'administrateur de base de données s'occupe de la gestion des données

## Architecture logicielle

- serveur web (Apache)
- serveur de données (MySQL)
- serveur d'application (PHP)
- serveur de fichiers (FTP / SFTP)

## Concepts importants

- front office / back office (visiteur / admin)
- les pages (page de contact, qui sommes-nous ?)
- les articles (blog, brèves, événements)
- les customs types (page spécifiques avec des champs personnalisés, date, n° de la salle, etc)
- les tags
- les catégories
- les thèmes
- les plugins
- les widgets
- le menu
- les users
- l'affichage des URL

## Prérequis

Il faut d'abord installer un serveur web, un serveur de données et un serveur d'appplication (Apache, MySQL et PHP).

### Wampserver

- [Download WampServer from SourceForge.net](https://sourceforge.net/projects/wampserver/files/latest/download?source=files)

Wamp devrait être installé dans le dossier suivant :

	C:\wampserver64

#### PhpMyAdmin

Nom de la BDD : caractères de a à z, (en minuscule) 0 à 9 et le tiret du bas `_`.

#### Problème de fichier `dll` manquant

- [WAMPSERVER Homepage](http://wampserver.aviatechno.net/)

## Installation

1. Création de la BDD
2. Décompression du fichier zip
3. Transfert des fichiers par FTP / SFTP ou  déplacement dans un autre dossier
4. Configuration de l'accès à la BDD
5. Configuration de du nom du site et de l'accès admin
7. Connexion à l'admin
8. Paramétrage de Wordpress

NB le fichier `wp-config.php` contient la configuration de l'accès à la BDD.

### déplacement du dossier dézippé dans le dossier de Wamp

	C:\wampserver64\www

	http://localhost/phpmyadmin

- login : root
- password : aucun

## Voir le site

Pour accéder à la partie visiteur (front) de wordpress, ouvrir cette adresse :

	http://localhost/wordpress

NB Adapter la partie `wordpress` avec le nom de son site.

Pour accéder à l'admin (back) de wordpress, ouvrir cette adresse :

	http://localhost/wordpress/wp-admin

NB Idem, adapter la partie `wordpress` avec le nom de son site.

## Reset du password admin

Par requête SQL :

	UPDATE wp_users SET user_pass=MD5("123") WHERE ID = 1

NB remplacer le mot de passe `123` par le votre.

## Où trouver des thèmes ?

### Thèmes « classiques »

#### Officiels

- [Thèmes WordPress gratuits](https://fr.wordpress.org/themes/)

#### Sélection

- [15+ Best Free Responsive WordPress Business Themes 2018 | WPAll](https://wpall.club/wordpress-themes-roundup/best-free-wordpress-business-themes/)
- [Free - Learn WordPress with WPLift](https://wplift.com/theme-category/free)

#### Agence

- [aThemes - Awesome WordPress Themes](https://athemes.com/)
- [Free WordPress Themes and Templates](http://www.ilovewp.com/)
- [InkHive.com - We Produce Stunning WordPress Themes](https://inkhive.com/)
- [Premium Responsive WordPress Themes by ThemeGrill](https://themegrill.com/)
- [Premium WordPress Themes | WooCommerce Themes | Theme Fusion](https://theme-fusion.com/)
- [Theme Hybrid – Plugins & Themes](https://themehybrid.com/)
- [WordPress Themes Loved By Over 490k Customers](https://www.elegantthemes.com/)

#### Plateforme de vente

- [WordPress Themes & Website Templates from ThemeForest](https://themeforest.net/)

### Thèmes « ultra personnalisables »

- [Divi — The Ultimate WordPress Theme & Visual Page Builder](https://www.elegantthemes.com/gallery/divi/)
- [Jupiter WordPress Theme - Create WordPress Websites Everyday](https://themes.artbees.net/pages/jupiter-wordpress-theme-create-wordpress-websites/)

## Comment créer un child theme ?

Exemple avec twentyseventeen :

    twentyseventeen-child/	<= nom du child theme
        functions.php		<= configuration du child theme
        screenshot.png		<= capture d'écran du child theme (optionnel)
        style.css			<= feuille de style du child theme

Voir le repo [https://github.com/jibundeyare/src-wordpress-child-theme](https://github.com/jibundeyare/src-wordpress-child-theme) pour plus de détails.

## Ressources iconographiques

- [Unsplash | Free High-Resolution Photos](https://unsplash.com/)
- [Trouvez l’inspiration. | Flickr](https://www.flickr.com/)

## Lorem ipsum

- [Lorem Ipsum - All the facts - Lipsum generator](http://lipsum.com/)

## Les plugins

Les plugins permettent de rajouter des fonctionnalités supplémentaires à Wordpress.

### Comment désactiver un plugin ?

Pour désactiver un plugin sans passer par l'admin, vous pouvez rajouter un symbole underscore `_` au début du nom de son dossier (`wp-content/plugins/_monplugin` par exemple).

### Quels plugins utiliser ?

#### Pack

- [Jetpack by WordPress.com — WordPress Plugins](https://wordpress.org/plugins/jetpack/)

#### Backups

- [All-in-One WP Migration — WordPress Plugins](https://wordpress.org/plugins/all-in-one-wp-migration/)
- [UpdraftPlus WordPress Backup Plugin — WordPress Plugins](https://wordpress.org/plugins/updraftplus/)

#### Cache

Le cache permet d'accélérer l'affichage des pages.

- [W3 Total Cache — WordPress Plugins](https://wordpress.org/plugins/w3-total-cache/)
- [WP Super Cache — WordPress Plugins](https://wordpress.org/plugins/wp-super-cache/)

#### Carousel / Slider

- [Master Slider – Responsive Touch Slider — Extensions WordPress](https://fr.wordpress.org/plugins/master-slider/)
- [MetaSlider — Extensions WordPress](https://fr.wordpress.org/plugins/ml-slider/)
- [Shortcodes Ultimate — Extensions WordPress](https://fr.wordpress.org/plugins/shortcodes-ultimate/)
- [Slider by Soliloquy – Responsive Image Slider for WordPress — Extensions WordPress](https://fr.wordpress.org/plugins/soliloquy-lite/)

#### Custom fields

Les custom fields permettent de rajouter des champs dans une page, un article ou un custom type.

- [Advanced Custom Fields — WordPress Plugins](https://wordpress.org/plugins/advanced-custom-fields/)

#### E-Commerce

- [Contact Form 7 – PayPal & Stripe Add-on — WordPress Plugins](https://wordpress.org/plugins/contact-form-7-paypal-add-on/)
- [PayPal Donation — WordPress Plugins](https://wordpress.org/plugins/easy-paypal-donation/)
- [PayPal Events — WordPress Plugins](https://wordpress.org/plugins/easy-paypal-events-tickets/)
- [PayPal for WooCommerce — WordPress Plugins](https://wordpress.org/plugins/paypal-for-woocommerce/)
- [PayPal Plus for WooCommerce — WordPress Plugins](https://wordpress.org/plugins/woo-paypalplus/)
- [WooCommerce — WordPress Plugins](https://wordpress.org/plugins/woocommerce/)
- [WooCommerce PayPal Express Checkout Payment Gateway — WordPress Plugins](https://wordpress.org/plugins/woocommerce-gateway-paypal-express-checkout/)
- [WooCommerce Stripe Payment Gateway — WordPress Plugins](https://wordpress.org/plugins/woocommerce-gateway-stripe/)

#### Formulaires

Les formulaires sont utilisés pour permettre aux visiteurs de contacter le propriétaire du site ou de s'abonner à une newsletter.

- [Formidable Forms – Form Builder for WordPress — WordPress Plugins](https://wordpress.org/plugins/formidable/)
- [Contact Form 7 — WordPress Plugins](https://wordpress.org/plugins/contact-form-7/)

#### Forum

- [bbPress — WordPress Plugins](https://wordpress.org/plugins/bbpress/)

#### Mail

- [MailChimp for WordPress — WordPress Plugins](https://wordpress.org/plugins/mailchimp-for-wp/)
- [MailPoet Newsletters (New) — WordPress Plugins](https://wordpress.org/plugins/mailpoet/)

#### Maintenance

- [WP Maintenance Mode — Extensions WordPress](https://fr.wordpress.org/plugins/wp-maintenance-mode/)

#### Mise en page

Ces plugins permettent de mettre en page un site de façon « plus visuelle ».

- [Elementor Page Builder — WordPress Plugins](https://wordpress.org/plugins/elementor/)
- [Page Builder by SiteOrigin — WordPress Plugins](https://wordpress.org/plugins/siteorigin-panels/)
- [Page Builder: Live Composer – drag and drop website builder (visual front end site editor) — Extensions WordPress](https://fr.wordpress.org/plugins/live-composer-page-builder/)
- [TinyMCE Advanced — WordPress Plugins](https://wordpress.org/plugins/tinymce-advanced/)

#### Réseaux sociaux

- [Social Media Share Buttons & Social Sharing Icons (Ultimate Sharing) — Extensions WordPress](https://fr.wordpress.org/plugins/ultimate-social-media-icons/)
- [BuddyPress — WordPress Plugins](https://wordpress.org/plugins/buddypress/)

#### Sécurité

- [Loginizer — WordPress Plugins](https://wordpress.org/plugins/loginizer/)
- [Wordfence Security – Firewall & Malware Scan — WordPress Plugins](https://wordpress.org/plugins/wordfence/)

#### SEO

SEO : Search Engin Optimisation (optimisation pour les moteur de recherches).

- [Yoast SEO — WordPress Plugins](https://wordpress.org/plugins/wordpress-seo/)
- [All in One SEO Pack — WordPress Plugins](https://wordpress.org/plugins/all-in-one-seo-pack/)

#### Stats

- [Google Analytics for WordPress by MonsterInsights — WordPress Plugins](https://wordpress.org/plugins/google-analytics-for-wordpress/)
- [Google Analytics Dashboard for WP (GADWP) — WordPress Plugins](https://wordpress.org/plugins/google-analytics-dashboard-for-wp/)

## Tutoriels vidéo

### Généralités

- [Cours WordPress - YouTube](https://www.youtube.com/watch?v=cEGEmPJHBjc&list=PL5BcU-_5Oa_ocJ_cyNKqhqJICMEJM9uqy)
- [Web Marketing Tuto - YouTube - YouTube](https://www.youtube.com/user/julienWebM)
- [WP Marmite - YouTube - YouTube](https://www.youtube.com/channel/UCU_gPhU-eAI56oUeFzVyUUQ)

### Hébergement & mise en prod

- [Créer un Blog WORDPRESS avec OVH - YouTube](https://www.youtube.com/watch?v=frxVxsXr9Bw)
- [Tutoriel Hébergement - Héberger et Uploader son site web - YouTube](https://www.youtube.com/watch?v=ej-Fax5NuUw)

### Thèmes

- [Devez-vous utiliser le thème WordPress DIVI d'Elegant Themes ? - YouTube](https://www.youtube.com/watch?v=Q1jrpvqaNKw)

### WooCommerce

- [Créer une boutique en ligne avec Wordpress 4 & WooCommerce (1/8) - YouTube](https://www.youtube.com/watch?v=nEL8xCTGXhM&index=8&list=PLpfOedZZax4wl_l31ajvr0su_rUtZ25Dh)
- [e-Commerce: Créer un Site en 2h ! [WORDPRESS + WOOCOMMERCE] - YouTube](https://www.youtube.com/watch?v=B6opNgiDqvI)

### Programmation (HTML, CSS, JavaScript, PHP, MySQL)

- [Grafikart.fr - YouTube - YouTube](https://www.youtube.com/user/grafikarttv)

## Doc

### Wordpress

- [Main Page « WordPress Codex](https://codex.wordpress.org/)
- [Child Themes « WordPress Codex](https://codex.wordpress.org/Child_Themes)
- [Moving WordPress « WordPress Codex](https://codex.wordpress.org/Moving_WordPress)

### Programmation (HTML, CSS, JavaScript, PHP, MySQL)

- [Pierre Giraud - Apprendre à coder gratuitement](http://www.pierre-giraud.com/home.php)
