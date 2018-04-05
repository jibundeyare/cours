# JavaScript

- syntaxe et concepts de base : variables, structures de contrôle, boucles et fonctions
- POO (Programmation Orientée Objet)
- intégration de code JavaScript dans une page web : balise `script` et webpack
- jQuery et manipulation du DOM (Document Object Model)
- balise `canvas` et Processing

## Arborescence d'un projet

Voir [html-css.md](html-css.md).

## Syntaxe

Commenter le code :

    // commentaire sur une seule ligne

    /*
    commentaire
    sur plusieurs
    lignes
    */

### Déclaration de variable

`var` permet de déclarer une variable à portée globale.
`let` permet de déclarer une variable à portée locale.

### Opérateurs de comparaison

Égalité : `==`

Supérieur : `>`

Inférieur : `<`

Supérieur ou égal : `>=`

Inférieur ou égal : `<=`

Différent : `!=`

Strictement identique (type et valeur) : `===`

Strictement différent (type et valeur) : `!==`

## Intégration de fichiers (plugins) JavaScript dans une page HTML

Il y a plusieurs étapes :

1. copier les fichiers JavaScript dans le dossier du projet (manuellement ou via npm)
2. intégrer les fichiers CSS et JavaScript du plugin dans votre code HTML
3. écrire du code (HTML, CSS, JavaScript) qui exploite les fonctionnalités du plugin
4. configurer puis démarrer le plugin (avec du code JavaScript)

Et plusieurs méthodes sont possibles :

1. intégration manuelle (chargement local)
2. liens vers un CDN (chargement distant )
3. intégration avec npm (chargement local)
4. intégration avec npm et Webpack (chargement local)

### Intégration manuelle

- allez sur le site du plugin puis téléchargez le fichier JavaScript
- si le fichier est une archive zip, dézipez le à la racine de votre projet
- ajouter les balises `link` et `script` qui permettent d'intégrer les feuilles de style et les fichiers JavaScript dans votre code HTML

### Liens vers un CDN

- allez sur le site du plugin et repérez la documenation concernant l'installation
- copiez les balises `link` et `script` qui pointent vers des nom de domaines de CDN (`https://maxcdn.foobarbaz.com/` par exemple)
- collez ces balises dans votre code HTML

### Intégration avec npm

Voir [npm.md](npm.md).

### Intégration avec npm et Webpack

Voir [webpack.md](webpack.md).

## Portée des variables (scope)

Le scope fonctionne comme des vitres teintées : de l'intérieur, on voit l'extérieur mais de l'extérieur, on ne voit pas l'intérieur.

Exemple : de l'intérieur d'une fonction, on voit les variables déclarées dans le scope global mais dans le scope global, on ne voit pas les variables déclarées dans une fonction.

## Doc

- [JavaScript Guide - JavaScript | MDN](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide)
- [JavaScript reference - JavaScript | MDN](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference)
- [The Modern Javascript Tutorial](http://javascript.info/)
- [Babel · The compiler for writing next generation JavaScript](https://babeljs.io/)
- [Node.js](https://nodejs.org/en/)
