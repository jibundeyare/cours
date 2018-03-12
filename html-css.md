# HTML & CSS

## Arborescence d'un projet

    my_project/         <= document root
        css/            <= vos feuilles de style
            *.css
        fonts/          <= vos typos
        img/            <= vos images
            *.gif
            *.jpg
            *.png
            *.svg
        js/             <= vos fichiers JavaScript
            *.js
        node_modules/   <= paquets gérés par npm
        sass/           <= vos feuilles de style
            *.sass
            *.scss
            main.sass
        .gitignore      <= liste des fichiers que git doit ignorer
        *.html          <= tous les fichiers HTML
        package.json    <= liste des paquets gérés par npm
        package.lock    <= liste des paquets gérés par npm
        README.md       <= documentation du projet

## Ordre d'intégration des feuilles de style et des fichiers JavaScript

### Les feuilles de style

Les feuilles de styles doivent être intégrées dans un ordre précis :

- d'abord les feuilles de style externes (Bootstrap par exemple)
- ensuite votre feuille de style

Ceci permet de surcharger les feuilles de style externes, c-à-d d'avoir le dernier mot.

Attention aussi à respecter l'ordre de chargement recommandé pour les feuilles de style externes. Par exemple :

- d'abord `bootstrap.min.css`
- puis `bootstrap-theme.min.css`

### Les fichiers JavaScript

Les fichiers JavaScript doivent être intégrées dans un ordre précis :

- d'abord les fichiers JavaScript externes (jQuery par exemple)
- ensuite vos fichiers JavaScript

Ceci permet d'utiliser ou d'activer les libraires JavaScript externes dans vos fichiers JavaScript.

Attention aussi à respecter l'ordre de chargement des fichiers JavaScript externes en fonctione de leur dépendances. Par exemple :

- d'abord `jquery-3.2.1.min.js`
- puis `bootstrap.min.js`

## Web sémantique

Vous pouvez utiliser des balises sémantiques mais vous pouvez aussi intégrer les données en utilisant des micro-formats.

Voir [http://schema.org/](http://schema.org/) pour utiliser des formules toutes prêtes.

## Doc

- [jQuery](http://jquery.com/)
- [Bootstrap · The world's most popular mobile-first and responsive front-end framework.](https://getbootstrap.com/docs/3.3/)
- [Can I use... Support tables for HTML5, CSS3, etc](https://caniuse.com/)
- [CSS-Tricks](https://css-tricks.com/)
- [Alsacréations : Actualités et tutoriels web, HTML, CSS, JavaScript](https://www.alsacreations.com/)

- [transform - CSS | MDN](https://developer.mozilla.org/en-US/docs/Web/CSS/transform)
- [CSS3 : Transformations 2D - Alsacreations](https://www.alsacreations.com/article/lire/1418-css3-transformations-2d.html)
- [Using CSS transitions - CSS | MDN](https://developer.mozilla.org/en-US/docs/Web/CSS/CSS_Transitions/Using_CSS_transitions)
- [Transitions - CSS Reference](http://cssreference.io/transitions/)

- [Using CSS animations - CSS | MDN](https://developer.mozilla.org/en-US/docs/Web/CSS/CSS_Animations/Using_CSS_animations)
- [animation - CSS | MDN](https://developer.mozilla.org/en-US/docs/Web/CSS/animation)
- [animation | CSS-Tricks](https://css-tricks.com/almanac/properties/a/animation/)
- [Animatable CSS properties - CSS | MDN](https://developer.mozilla.org/en-US/docs/Web/CSS/CSS_animated_properties)
- [Intro to CSS 3D transforms · Intro to CSS 3D transforms](http://desandro.github.io/3dtransforms/)

- [A Complete Guide to Flexbox | CSS-Tricks](https://css-tricks.com/snippets/css/a-guide-to-flexbox/)
- [A Complete Guide to Grid | CSS-Tricks](https://css-tricks.com/snippets/css/complete-guide-grid/)

- [A Guide to SVG Animations (SMIL) | CSS-Tricks](https://css-tricks.com/guide-svg-animations-smil/)
- [An Intro to SVG Animation with SMIL by Noah Blon on CodePen](https://codepen.io/noahblon/post/an-intro-to-svg-animation-with-smil)
- [How SVG Shape Morphing Works | CSS-Tricks](https://css-tricks.com/svg-shape-morphing-works/)
- [SVG Animation and CSS Transforms: A Complicated Love Story | CSS-Tricks](https://css-tricks.com/svg-animation-on-css-transforms/)

- [Microformat - Wikipedia](https://en.wikipedia.org/wiki/Microformat)
- [Home - schema.org](http://schema.org/)
