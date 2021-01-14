# Wordpress - Développement de thème

## Minimum requis

Pour créer un thème custom pour Wordpress, il faut au minimum les fichiers suivants :

- `style.css` qui contient les méta données du thème (nom, auteur, url, etc)
- `index.php` qui affiche les pages du site
- `screenshot.png` qui contient une capture d'écran du thème (au format 1200px * 900px)

## Au-delà du minimum

Si on veut un thème custom plus évolué, on peut commencer par rajouter les fichiers suivants :

- `functions.php` qui définit les paramètres du thème
- `header.php` qui contient l'entête du site (avec la navbar)
- `footer.php` qui contient le pied de page du site
- `page.php` qui affiche le contenu de type `page`
- `single.php` qui affiche le contenu de type `post`
- `404.php` qui affiche la page d'erreur 404
- `search.php` qui affiche le résultat d'une recherche

Voir la page [Organizing Theme Files | Theme Developer Handbook | WordPress Developer Resources](https://developer.wordpress.org/themes/basics/organizing-theme-files/) pour en savoir plus.

## Déboggage

À placer dans le fichier `wp-config.php` :

    @ini_set( 'display_errors', 'On' );
    define( 'WP_DISABLE_FATAL_ERROR_HANDLER', true ); // 5.2 and later
    define( 'WP_DEBUG', true );
    define( 'WP_DEBUG_DISPLAY', true );

## Page d'accueil personnalisée

Par défaut, la page d'accueil de Wordpress utilise la liste des articles.
Autrement dit, quand on appel l'URL `http://example.com/`, Wordpress utilise le fichier `index.php` pour afficher le contenu de la page.

Mais il est possible de modifier ce réglage par défaut :

![Réglages par défaut de la page d'accueil](img/home-page-default-settings.png)

### Exemple avec une page d'accueil et une page de news

L'idée est d'avoir un fichier PHP qui affiche la page d'accueil et un autre fichier PHP qui affiche la page des News.
Cela nous permettra de personnaliser l'affichage.

Pour faire cela, il faut tout d'abord :

- créer une page nommée « Accueil »
- créer une autre page nommée « News »
- créer un fichier nommé `front-page.php`
- créer un fichier nommé `home.php`
- configurer la page d'accueil et la page des articles de Wordpress dans les paramètres

La page News affichera une liste de news.
En réalité, ces news sont de simples articles.
La page News aurait pu s'appeler Foo, cela n'aurait rien changé, elle aurait tout de même affiché la liste des articles.

Voici un contenu possible du fichier `front-page.php` :

    <?php
    // wp-content/themes/my-theme/front-page.php

    get_header();

    if ( have_posts() ):
        while (have_posts()):
            the_post();
    ?>
        <article>
            <h1><?php the_title(); ?></h1>
            <div><?php the_content(); ?></div>
        </article>
    <?php
        endwhile;
    endif;

    get_footer();

Voici un contenu possible du fichier `home.php` :

    <?php
    get_header();

    if ( have_posts() ):
        while (have_posts()):
            the_post();
    ?>
        <article>
            <h1><a href="<?= get_permalink(); ?>"><?php the_title(); ?></a></h1>
            <div><?php the_content(); ?></div>

        </article>
    <?php
        endwhile;
    endif;

    get_footer();

Maintenant, il faut changer les paramètres de Wordpress :

- aller dans la page **Admin > Réglages > Lecture**
- choisir l'option **Une page statique**
- sélectionner la page Accueil comme page d'accueil et la page News comme page d'articles
- enregistrer les modifications

Voici un exemple des réglages personnalisés :

![Réglages personnalisés de la page d'accueil](img/home-page-custom-settings.png)

## Créer un template de page

Un template de page permet d'afficher une avec apparence différente des autres.
Normalement, quand on veut visualiser une page (`http://example.com/contact` par exemple), c'est le fichier `page.php` qui est utilisé pour afficher le document.
Mais si l'on définit un template de page, c'est le fichier contenant ce template (`template-contact.php` par exemple) qui sera utilisé pour l'affichage.

Pour créer un template de page, il faut créer un fichier contenant un bloc de commentaire spécial :

    <?php
    /**
     * Template Name: Template Contact
     */

C'est ce commentaire, et rien d'autre, qui notifie à Wordpress l'existence d'un template de page.

Lors de l'édition d'une page dans l'admin de Wordpress, ce template sera affiché comme étant `Template Contact` dans la section **Attributs de page** dans la colonne de droite :

![template de page personnalisé](img/custom-page-template.png)

Il est d'usage de nommer les templates de page avec le préfixe `template-` et d'ajouter le nom de la page le nom du type de page.
Par exemple, pour la page contact, le fichier devrait être nommé `template-contact.php`.

Le reste du contenu du fichier PHP obéit aux règles habituelle d'un fichier de thème Wordpress.
On peut utiliser la boucle pour afficher la page demandée ou utiliser `WP_Query()` pour afficher tout type de contenu.

#### WP_Query

Afficher tous les articles (`post`) :

    // WP_Query arguments
    $args = array(
        'post_type' => array( 'post' ),
    );

    // The Query
    $query = new WP_Query( $args );

    // The Loop
    if ( $query->have_posts() ) {
        while ( $query->have_posts() ) {
            $query->the_post();
            // do something
        }
    } else {
        // no posts found
    }

    // Restore original Post Data
    wp_reset_postdata();

Afficher un article (`post`) correspondant à un slug donné :

    // WP_Query arguments
    $args = array(
        'name' => 'foo',
    );

    // The Query
    $query = new WP_Query( $args );

    // The Loop
    if ( $query->have_posts() ) {
        while ( $query->have_posts() ) {
            $query->the_post();
            // do something
        }
    } else {
        // no posts found
    }

    // Restore original Post Data
    wp_reset_postdata();

Afficher une page (`page`) correspondant à un slug donné :

    // WP_Query arguments
    $args = array(
        'pagename' => 'foo',
        'post_type' => array( 'post' ),
    );

    // The Query
    $query = new WP_Query( $args );

    // The Loop
    if ( $query->have_posts() ) {
        while ( $query->have_posts() ) {
            $query->the_post();
            // do something
        }
    } else {
        // no posts found
    }

    // Restore original Post Data
    wp_reset_postdata();

## Créer un custom post type et l'afficher

1. Créer le custom post type
2. Activer la page d'archive pour ce custom post type
3. Créer le fichier `archive-$posttype.php`
4. Créer le fichier `single-$posttype.php`

## Doc

- [Theme Developer Handbook | WordPress Developer Resources](https://developer.wordpress.org/themes/)
- [Theme Development « WordPress Codex](https://codex.wordpress.org/Theme_Development)
- [WordPress theme - The Anatomy, an Infographic - Yoast](https://yoast.com/wordpress-theme-anatomy/)
- [Template Hierarchy | Theme Developer Handbook | WordPress Developer Resources](https://developer.wordpress.org/themes/basics/template-hierarchy/)
- [WordPress template hierarchy](https://developer.wordpress.org/files/2014/10/Screenshot-2019-01-23-00.20.04.png)
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

## Tutoriels

- [How to Build a WordPress Theme from Scratch: the Basics - SitePoint](https://www.sitepoint.com/build-wordpress-theme-from-scratch-basics/)

