@todo commentaires sur certains articles seulement
@todo google font
@todo install wampserver

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
- serveur de données (MariaDB)
- serveur d'application (PHP)
- serveur de fichiers (FTP / SFTP)

## Concepts importants

- front office / back office (visiteur / admin)
- les pages (page de contact, qui sommes-nous ?)
- les articles (blog, brèves, événements)
- les tags
- les catégories
- les thèmes
- les plugins
- les widgets
- le menu
- les users
- l'affichage des URL
- les custom fields
- les customs types (page spécifiques avec des champs personnalisés, date, adresse, coordonnées GPS, n° de salle, etc)

## Prérequis

Il faut d'abord installer un serveur web, un serveur de données et un serveur d'appplication (Apache, MariaDB et PHP).

### Wampserver

- [Download WampServer from SourceForge.net](https://sourceforge.net/projects/wampserver/files/latest/download?source=files)

Wamp est installé dans le dossier suivant :

    C:\wamp64

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

### Déplacement du dossier dézippé dans le dossier de Wamp

    C:\wamp64\www

[http://localhost/phpmyadmin](http://localhost/phpmyadmin)

- login : root
- password : aucun

## Voir le site

Pour accéder à la partie visiteur (front) de wordpress, ouvrir cette adresse : [http://localhost/wordpress](http://localhost/wordpress)

NB Adapter la partie `wordpress` avec le nom de son site.

Pour accéder à l'admin (back) de wordpress, ouvrir cette adresse : [http://localhost/wordpress/wp-admin](http://localhost/wordpress/wp-admin)

NB Idem, adapter la partie `wordpress` avec le nom de son site.

## Reset du password admin

Par requête SQL :

    UPDATE wp_users SET user_pass = MD5("123") WHERE ID = 1

NB remplacer le mot de passe `123` par le votre.

[Resetting Your Password « WordPress Codex](https://codex.wordpress.org/Resetting_Your_Password)

## Où trouver des thèmes ?

### Thèmes « classiques »

#### Thèmes officiels

- [Free WordPress Themes](https://wordpress.org/themes/browse/popular/)

#### Sélection de thèmes

- [15+ Best Free Responsive WordPress Business Themes 2018 | WPAll](https://wpall.club/wordpress-themes-roundup/best-free-wordpress-business-themes/)
- [Free - Learn WordPress with WPLift](https://wplift.com/theme-category/free)

#### Thèmes créées par des agences

- [aThemes - Awesome WordPress Themes](https://athemes.com/)
- [Free WordPress Themes and Templates](http://www.ilovewp.com/)
- [InkHive.com - We Produce Stunning WordPress Themes](https://inkhive.com/)
- [Premium Responsive WordPress Themes by ThemeGrill](https://themegrill.com/)
- [Premium WordPress Themes | WooCommerce Themes | Theme Fusion](https://theme-fusion.com/)
- [Theme Hybrid – Plugins & Themes](https://themehybrid.com/)
- [WordPress Themes Loved By Over 490k Customers](https://www.elegantthemes.com/)

#### Thèmes payants livrés avec des « page builders »

- [Jupiter WordPress Theme - Create WordPress Websites Everyday](https://themes.artbees.net/pages/jupiter-wordpress-theme-create-wordpress-websites/)
- [Divi — The Ultimate WordPress Theme & Visual Page Builder](https://www.elegantthemes.com/gallery/divi/)

#### Thèmes gratuits livrés avec des « page builders »

- [Visual Composer Starter Theme for WordPress](https://visualcomposer.io/visual-composer-starter-theme/)
- [Orao – Portfolio Oriented • Live Composer](https://livecomposerplugin.com/downloads/orao-theme/)

#### Plateforme de distribution / vente de thèmes

- [ThemeForest](https://themeforest.net/)

### Thèmes pour développeur

- [Automattic/_s: Hi. I'm a starter theme called _s, or underscores, if you like. I'm a theme meant for hacking so don't use me as a Parent Theme. Instead try turning me into the next, most awesome, WordPress theme out there. That's what I'm here for.](https://github.com/automattic/_s)
- [Composer Starter Theme](https://visualcomposer.io/visual-composer-starter-theme/)
- [BLANK Theme • Live Composer](https://livecomposerplugin.com/downloads/blank-theme/)

## Comment créer un child theme ?

Exemple avec twentyseventeen :

    twentyseventeen-child/  <= nom du child theme
        functions.php       <= configuration du child theme
        screenshot.png      <= capture d'écran du child theme (optionnel)
        style.css           <= feuille de style du child theme

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

- [Jetpack](https://wordpress.org/plugins/jetpack/)

#### Backups / Migration

- [All-in-One WP Migration](https://wordpress.org/plugins/all-in-one-wp-migration/)
- [UpdraftPlus WordPress Backup Plugin](https://wordpress.org/plugins/updraftplus/)
- [Duplicator – WordPress Migration Plugin](https://wordpress.org/plugins/duplicator/)

Personnellement, je recommande Duplicator.

#### Cache

Le cache permet d'accélérer l'affichage des pages.

- [W3 Total Cache](https://wordpress.org/plugins/w3-total-cache/)
- [WP Super Cache](https://wordpress.org/plugins/wp-super-cache/)

#### Carousel / Slider

- [Master Slider – Responsive Touch Slider](https://wordpress.org/plugins/master-slider/)
- [MetaSlider](https://wordpress.org/plugins/ml-slider/)
- [Shortcodes Ultimate](https://wordpress.org/plugins/shortcodes-ultimate/)
- [Slider by Soliloquy – Responsive Image Slider for WordPress](https://wordpress.org/plugins/soliloquy-lite/)

#### E-Commerce

- [Contact Form 7 – PayPal & Stripe Add-on](https://wordpress.org/plugins/contact-form-7-paypal-add-on/)
- [PayPal Donation](https://wordpress.org/plugins/easy-paypal-donation/)
- [PayPal Events](https://wordpress.org/plugins/easy-paypal-events-tickets/)
- [PayPal for WooCommerce](https://wordpress.org/plugins/paypal-for-woocommerce/)
- [PayPal Plus for WooCommerce](https://wordpress.org/plugins/woo-paypalplus/)
- [WooCommerce](https://wordpress.org/plugins/woocommerce/)
- [WooCommerce PayPal Express Checkout Payment Gateway](https://wordpress.org/plugins/woocommerce-gateway-paypal-express-checkout/)
- [WooCommerce Stripe Payment Gateway](https://wordpress.org/plugins/woocommerce-gateway-stripe/)

#### Formulaires

Les formulaires sont utilisés pour permettre aux visiteurs de contacter le propriétaire du site ou de s'abonner à une newsletter.

- [Contact Form 7](https://wordpress.org/plugins/contact-form-7/)
- [Formidable Forms – Form Builder for WordPress](https://wordpress.org/plugins/formidable/)
- [Ninja Forms – The Easy and Powerful Forms Builder | WordPress.org](https://fr.wordpress.org/plugins/ninja-forms/)

#### Forum

- [bbPress](https://wordpress.org/plugins/bbpress/)

#### Mail

- [MailChimp for WordPress](https://wordpress.org/plugins/mailchimp-for-wp/)
- [MailPoet Newsletters (New)](https://wordpress.org/plugins/mailpoet/)

#### Maintenance

- [WP Maintenance Mode](https://wordpress.org/plugins/wp-maintenance-mode/)

#### TinyMCE

Le plugin TinyMCE permet de choisir la typographie du texte dans les moindres détails (titre, taille, couleur, police, etc).

- [TinyMCE Advanced](https://wordpress.org/plugins/tinymce-advanced/)

#### Page builders / Mise en page

Ces plugins permettent de mettre en page un site de façon « plus visuelle ».

##### Live mode

Le « live mode » permet de construire sa page tout en voyant (à peu près) ce que l'utilisateur final verra.

- [Visual Composer Website Builder](https://visualcomposer.io/)
- [Elementor Page Builder](https://wordpress.org/plugins/elementor/)
- [Live Composer](https://wordpress.org/plugins/live-composer-page-builder/)
- [Beaver Builder](https://wordpress.org/plugins/beaver-builder-lite-version/)

Astuce : pour migrer des pages créées avec Elementor : « Dashboard > Elementor > Tools > Regenerate CSS »

##### Classic mode

Le « classic mode » permet de construire sa page seulement dans l'éditeur de l'admin.
Bien que que visuel, ce mode est un peu moins intuitif que le « live mode ».

- [Page Builder by SiteOrigin](https://wordpress.org/plugins/siteorigin-panels/)
- [KingComposer](https://wordpress.org/plugins/kingcomposer/)
- [Fusion Page Builder](https://wordpress.org/plugins/fusion/) (fonctionne avec le framework CSS Bootstrap)

#### Réseaux sociaux

- [Social Media Share Buttons & Social Sharing Icons (Ultimate Sharing)](https://wordpress.org/plugins/ultimate-social-media-icons/)
- [BuddyPress](https://wordpress.org/plugins/buddypress/)

#### Sécurité

- [Loginizer](https://wordpress.org/plugins/loginizer/)
- [Wordfence Security – Firewall & Malware Scan](https://wordpress.org/plugins/wordfence/)
- [WPScan Vulnerability Database](https://wpvulndb.com/)

Voir l'article suivant pour se faire un avis sur le plugin WordFence :

- [WordFence Review – Is It Really The Best WordPress Security Plugin? | Elegant Themes Blog](https://www.elegantthemes.com/blog/resources/wordfence-review-is-it-really-the-best-wordpress-security-plugin)

#### SEO

SEO : Search Engin Optimisation (optimisation pour les moteur de recherches).

- [Yoast SEO](https://wordpress.org/plugins/wordpress-seo/)
- [All in One SEO Pack](https://wordpress.org/plugins/all-in-one-seo-pack/)

#### Stats

- [Google Analytics for WordPress by MonsterInsights](https://wordpress.org/plugins/google-analytics-for-wordpress/)
- [Google Analytics Dashboard for WP (GADWP)](https://wordpress.org/plugins/google-analytics-dashboard-for-wp/)

Si vous voulez conserver vos données de fréquentation sans passer par un service comme Google Analytics, vous pouvez installer Matomo et le plugin suivant :

- [#1 Free Web Analytics Software](https://matomo.org/)
- [WP-Matomo (WP-Piwik) | WordPress.org](https://fr.wordpress.org/plugins/wp-piwik/)

#### Traduction

- [Polylang](https://wordpress.org/plugins/polylang/)

#### Custom fields

Les custom fields permettent de rajouter des champs dans une page, un article ou un custom type.

- [Advanced Custom Fields](https://wordpress.org/plugins/advanced-custom-fields/)
- [Meta Box – WordPress Custom Fields Framework](https://wordpress.org/plugins/meta-box/)

#### Custom post type

Open source et libre :

- [Pods – Custom Content Types and Fields](https://wordpress.org/plugins/pods/)

À noter, le plugin « Pods Beaver Themer Add-On » qui permet d'intégrer des custom post type créés avec Pods dans [Beaver Builder](https://wordpress.org/plugins/beaver-builder-lite-version/) :

- [Pods Beaver Themer Add-On | WordPress.org](https://wordpress.org/plugins/pods-beaver-builder-themer-add-on/)

Le plugin de création de custom type est libre mais le plugin d'affichage de custom type est payant :

- [Custom Post Type UI — WordPress Plugins](https://wordpress.org/plugins/custom-post-type-ui/)
- [Toolset Types – Custom Post Types, Custom Fields and Taxonomies — WordPress Plugins](https://wordpress.org/plugins/types/)
- [Custom Post Types and Custom Fields creator – WCK — WordPress Plugins](https://wordpress.org/plugins/wck-custom-fields-and-custom-post-types-creator/)

### Outils

#### Générateurs de code

- [GenerateWP - User friendly tools for WordPress developers](https://generatewp.com/)

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

### Programmation (HTML, CSS, JavaScript, PHP, MariaDB)

- [Grafikart.fr - YouTube - YouTube](https://www.youtube.com/user/grafikarttv)

## Doc

### Wordpress

#### Power user

- [Main Page « WordPress Codex](https://codex.wordpress.org/)
- [Child Themes | Theme Developer Handbook | WordPress Developer Resources](https://developer.wordpress.org/themes/advanced-topics/child-themes/)
- [Moving WordPress « WordPress Codex](https://codex.wordpress.org/Moving_WordPress)

#### Développeur

- [WordPress theme - The Anatomy, an Infographic - Yoast](https://yoast.com/wordpress-theme-anatomy/)
- [Template_Hierarchy.png (PNG Image, 1453 × 1443 pixels) - Scaled (52%)](https://codex.wordpress.org/images/1/18/Template_Hierarchy.png)
- [Theme Development « WordPress Codex](https://codex.wordpress.org/Theme_Development)
- [Including CSS & JavaScript | Theme Developer Handbook | WordPress Developer Resources](https://developer.wordpress.org/themes/basics/including-css-javascript/)
- [JavaScript Best Practices | Theme Developer Handbook | WordPress Developer Resources](https://developer.wordpress.org/themes/advanced-topics/javascript-best-practices/)
- [The Loop in Action « WordPress Codex](https://codex.wordpress.org/The_Loop_in_Action)
- [Class Reference/WP Query « WordPress Codex](https://codex.wordpress.org/Class_Reference/WP_Query)
- [Conditional Tags « WordPress Codex](https://codex.wordpress.org/Conditional_Tags)
- [Shortcode API « WordPress Codex](https://codex.wordpress.org/Shortcode_API)
- [Function Reference/get the tag list « WordPress Codex](https://codex.wordpress.org/Function_Reference/get_the_tag_list)
- [Function Reference/get the category list « WordPress Codex](https://codex.wordpress.org/Function_Reference/get_the_category_list)
- [Post Types « WordPress Codex](https://codex.wordpress.org/Post_Types)
- [Custom Fields « WordPress Codex](https://codex.wordpress.org/Custom_Fields)

