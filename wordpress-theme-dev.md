# Wordpress - Développement de thème

## Page d'accueil personnalisée

Par défaut, la page d'accueil de Wordpress utilise la liste des articles.
Autrement dit, quand on appel l'URL `http://example.com/`, Wordpress utilise le fichier `index.php` pour afficher le contenu de la page.

Mais il est possible de modifier ce réglage par défaut :

![Réglages par défaut de la page d'accueil](img/home-page-default-settings.png)

### Exemple avec une page d'accueil et une page de news

L'idée est d'avoir un fichier PHP qui affiche la page d'accueil et un autre fichier PHP qui affiche la page des News.
Cela nous permettra de personnaliser l'affichage.

Pour faire cela, il faut tout d'abord :

- créer une page nommée Accueil
- créer une autre page nommée News
- créer un fichier nommé `front-page.php`
- créer un fichier nommé `home.php`
- configurer la page d'accueil et la page des articles de Wordpress

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

Maintenant, il faut changer les réglages de Wordpress :

- aller dans la page **Admin > Réglages > Lecture**
- choisir l'option **Une page statique**
- sélectionner la page Accueil comme page d'accueil et la page News comme page d'articles
- enregistrer les modifications

Voici un exemple des réglages personnalisés :

![Réglages personnalisés de la page d'accueil](home-page-custom-settings.png)

## Créer un custom post type et l'afficher

@todo compléter

1. Créer le custom post type
2. Activer la page d'archive pour ce custom post type
3. Créer le fichier `archive-$posttype.php`
4. Créer le fichier `single-$posttype.php`

