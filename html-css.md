# Html & Css

## Arborescence d'un projet

    my_project/
        css/
            *.css
        fonts/
        img/
            *.gif
            *.jpg
            *.png
        js/
            *.js
            bundle.min.js
            main.js
        node_modules/
        sass/
            *.sass
            *.scss
            main.sass
        package.json
        package.lock

## Ordre d'intégration des feuilles de style

Les feuilles de styles doivent être intégrées dans un ordre précis :

- d'abord les feuilles de style externes (Bootstrap par exemple)
- ensuite votre feuille de style

Ceci permet de surcharger les feuilles de style externe, c-à-d d'avoir le dernier mot.

## Doc

- [jQuery](http://jquery.com/)
- [Bootstrap · The world's most popular mobile-first and responsive front-end framework.](https://getbootstrap.com/docs/3.3/)
