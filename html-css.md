# HTML & CSS

Code : [jibundeyare/src-html-css](https://github.com/jibundeyare/src-html-css)

## Arborescence d'un projet

    my_project/             <= document root
        css/                <= vos feuilles de style
            *.css
            main.css
        fonts/              <= vos typos
        img/                <= vos images
            *.gif
            *.jpg
            *.png
            *.svg
        js/                 <= vos fichiers JavaScript
            *.js
            p5-hello.js
        node_modules/       <= paquets gérés par npm
        sass/               <= vos feuilles de style
            *.sass
            *.scss
            main.sass
        .gitignore          <= liste des fichiers que git doit ignorer
        *.html              <= tous les fichiers HTML
        package.json        <= liste des paquets gérés par npm
        package-lock.json   <= liste des paquets gérés par npm
        README.md           <= documentation du projet au format markdown

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

## Les sélecteurs CSS

- sélection par balise
- sélection par classe
- sélection par id

## Responsive design / media queries

L'idée des media queries est de n'appliquer des règles CSS que si l'écran correspond à certains critères (comme l'orientation ou la largeur minimum par exemple).

Les breakpoints de Bootstrap 3 :

- Extra small devices : phones (<768px)
- Small devices : tablets (≥768px)
- Medium devices : desktops (≥992px)
- Large devices : desktops (≥1200px)

Les breakpoints de Bootstrap 4 :

- Extra small devices : portrait phones (<576px)
- Small devices : landscape phones (≥576px)
- Medium devices : tablets (≥768px)
- Large devices : desktops (≥992px)
 - Extra large devices : large desktops (≥1200px)

Le [responsive meta tag de base de MDN](https://developer.mozilla.org/en-US/docs/Mozilla/Mobile/Viewport_meta_tag#Viewport_basics) :

    <meta name="viewport" content="width=device-width, initial-scale=1" />

Le [responsive meta tag de Bootstrap 4](https://getbootstrap.com/docs/4.0/getting-started/introduction/#responsive-meta-tag)

    <meta name="viewport" content="width=device-width, initial-scale=1, shrink-to-fit=no" />

## Doc

- [HTML elements reference - HTML: HyperText Markup Language | MDN](https://developer.mozilla.org/en-US/docs/Web/HTML/Element)
- [CSS reference - CSS: Cascading Style Sheets | MDN](https://developer.mozilla.org/en-US/docs/Web/CSS/Reference)
- [Can I use... Support tables for HTML5, CSS3, etc](https://caniuse.com/)
- [CSS-Tricks](https://css-tricks.com/)
- [Alsacréations : Actualités et tutoriels web, HTML, CSS, JavaScript](https://www.alsacreations.com/)

- [Specificity - CSS | MDN](https://developer.mozilla.org/en-US/docs/Web/CSS/Specificity)
- [At-rule - CSS: Cascading Style Sheets | MDN](https://developer.mozilla.org/en-US/docs/Web/CSS/At-rule)
- [Box Sizing | CSS-Tricks](https://css-tricks.com/box-sizing/)

- [Using media queries - CSS | MDN](https://developer.mozilla.org/en-US/docs/Web/CSS/Media_Queries/Using_media_queries)
- [<link>: The External Resource Link element - HTML | MDN](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/link)
- [Using the viewport meta tag to control layout on mobile browsers - Mozilla | MDN](https://developer.mozilla.org/en-US/docs/Mozilla/Mobile/Viewport_meta_tag)

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
- [A Visual Guide to CSS3 Flexbox Properties ― Scotch](https://scotch.io/tutorials/a-visual-guide-to-css3-flexbox-properties)
- [Sticky Footer — Solved by Flexbox — Cleaner, hack-free CSS](https://philipwalton.github.io/solved-by-flexbox/demos/sticky-footer/)
- [css - Flex-box: Align last row to grid - Stack Overflow](https://stackoverflow.com/questions/18744164/flex-box-align-last-row-to-grid)

- [Getting Started with CSS Grid | CSS-Tricks](https://css-tricks.com/getting-started-css-grid/)
- [A Complete Guide to Grid | CSS-Tricks](https://css-tricks.com/snippets/css/complete-guide-grid/)
- [The Difference Between Explicit and Implicit Grids | CSS-Tricks](https://css-tricks.com/difference-explicit-implicit-grids/)
- [Does CSS Grid Replace Flexbox? | CSS-Tricks](https://css-tricks.com/css-grid-replace-flexbox/)

- [A Guide to SVG Animations (SMIL) | CSS-Tricks](https://css-tricks.com/guide-svg-animations-smil/)
- [An Intro to SVG Animation with SMIL by Noah Blon on CodePen](https://codepen.io/noahblon/post/an-intro-to-svg-animation-with-smil)
- [How SVG Shape Morphing Works | CSS-Tricks](https://css-tricks.com/svg-shape-morphing-works/)
- [SVG Animation and CSS Transforms: A Complicated Love Story | CSS-Tricks](https://css-tricks.com/svg-animation-on-css-transforms/)

- [ARIA - Accessibility | MDN](https://developer.mozilla.org/en-US/docs/Web/Accessibility/ARIA)
- [Keyboard-navigable JavaScript widgets - Accessibility | MDN](https://developer.mozilla.org/en-US/docs/Web/Accessibility/Keyboard-navigable_JavaScript_widgets)
- [Microformat - Wikipedia](https://en.wikipedia.org/wiki/Microformat)
- [Home - schema.org](http://schema.org/)

- [Bootstrap · The most popular HTML, CSS, and JS library in the world.](https://getbootstrap.com/)
- [Introduction · Bootstrap](https://getbootstrap.com/docs/4.0/getting-started/introduction/#responsive-meta-tag)
- [Responsive utilities · Bootstrap 3](https://getbootstrap.com/docs/3.3/css/#responsive-utilities)
- [Responsive breakpoints · Bootstrap 4](https://getbootstrap.com/docs/4.0/layout/overview/#responsive-breakpoints)

- [jQuery](http://jquery.com/)

