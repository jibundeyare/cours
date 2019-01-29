@ref ?/string-split.html

# JavaScript

- arborescence d'un projet
- intégration de code JavaScript dans une page web : balise `script` et webpack
- syntaxe
- concepts de base
  - déclaration de variables
  - variables de type simple (integer, float, booleann string)
  - variables de type Array (tableau)
  - variables de type Object
  - JSON
  - structures de contrôle (if, else, else if, switch )
  - boucles (for, for in, for of, while, do while)
  - fonctions (nommées, anonymes, paramètres, valeur de retour, closure, synchrone / asynchrone)
  - portée des variables (scope)
  - API DOM
  - évènements (events)
- POO (Programmation Orientée Objet)
- jQuery et manipulation du DOM (Document Object Model)
- balise `canvas` et Processing

## Arborescence d'un projet

Voir [html-css.md](html-css.md).

## Intégration de fichiers JavaScript dans une page HTML

Il y a deux étapes :

1. copier les fichiers JavaScript dans le dossier du projet
2. importer les fichiers JavaScript dans votre code HTML

Plusieurs méthodes sont possibles :

1. copie locale et import manuel
2. import avec un CDN
3. copie avec npm et import manuel
4. copie avec npm et import avec Webpack

### 1. Copie locale et import manuel

- allez sur le site du plugin puis téléchargez le fichier JavaScript
- si le fichier est une archive zip, dézipez le à la racine de votre projet
- ajouter les balises `link` et `script` qui permettent d'intégrer les feuilles de style et les fichiers JavaScript dans votre code HTML

### 2. Import avec un CDN

- allez sur le site du plugin et repérez la documenation concernant l'installation
- copiez les balises `link` et `script` qui pointent vers des nom de domaines de CDN (`https://maxcdn.foobarbaz.com/` par exemple)
- collez ces balises dans votre code HTML

### 3. Copie avec npm et import manuel

Voir [npm.md](npm.md).

### 4. Copie avec npm et import avec Webpack

Voir [webpack.md](webpack.md).

## Syntaxe

Commenter le code :

    // commentaire sur une seule ligne

    /*
    commentaire
    sur plusieurs
    lignes
    */

### Opérateurs de comparaison

Égalité : `==`

Supérieur : `>`

Inférieur : `<`

Supérieur ou égal : `>=`

Inférieur ou égal : `<=`

Différent : `!=`

Strictement identique (type et valeur) : `===`

Strictement différent (type et valeur) : `!==`

## Concepts de base

### Déclaration de variables

`var` permet de déclarer une variable à portée globale.

`let` permet de déclarer une variable à portée locale.

### Portée des variables (scope)

Le scope fonctionne comme des vitres teintées : de l'intérieur, on voit l'extérieur mais de l'extérieur, on ne voit pas l'intérieur.

Exemple : de l'intérieur d'une fonction, on voit les variables déclarées dans le scope global mais dans le scope global, on ne voit pas les variables déclarées dans une fonction.

## API

### `document.querySelectorAll()`

Sélectionner tous les `input` de type `checkbox` dont le `name` est `fruits[]` :

    var inputs = document.querySelectorAll('input[type="checkbox"][name="fruits[]"]');

Sélectionner tous les `input` de type `checkbox` dont le `name` n'est pas `fruits[]` :

    var inputs = document.querySelectorAll('input[type="checkbox"]:not([name="fruits[]"])');

## Doc

- [JavaScript Guide - JavaScript | MDN](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide)
- [JavaScript reference - JavaScript | MDN](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference)
- [The Modern Javascript Tutorial](http://javascript.info/)
- [Babel · The compiler for writing next generation JavaScript](https://babeljs.io/)
- [Node.js](https://nodejs.org/en/)
- [Strict mode - JavaScript | MDN](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Strict_mode)

- [String - JavaScript | MDN](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/String)
- [String.prototype.trim() - JavaScript | MDN](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/String/Trim)
- [String.prototype.search() - JavaScript | MDN](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/String/search)
- [String.prototype.replace() - JavaScript | MDN](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/String/replace)
- [String.prototype.split() - JavaScript | MDN](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/String/split)
- [RegExp - JavaScript | MDN](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/RegExp)
- [Array - JavaScript | MDN](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array)
- [Array.prototype.map() - JavaScript | MDN](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/map)
- [Array.prototype.reduce() - JavaScript | MDN](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/Reduce)
- [Array.prototype.sort() - JavaScript | MDN](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/sort)

- [javascript - How to use querySelectorAll only for elements that have a specific attribute set? - Stack Overflow](https://stackoverflow.com/questions/10777684/how-to-use-queryselectorall-only-for-elements-that-have-a-specific-attribute-set)

- [Understanding JavaScript Modules: Bundling & Transpiling — SitePoint](https://www.sitepoint.com/javascript-modules-bundling-transpiling/)
