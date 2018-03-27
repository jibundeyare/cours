# Webpack

Pour intégrer un package avec Webpack, il faut :

1. installer le package avec npm
2. si nécessaire, configurer Webpack pour pouvoir utiliser correctement le package
3. importer le package dans le code JavaScript (`import {foo} from 'bar';`)
4. créer un bundle
5. intégrer le bundle avec la balise `script`

Quand on ajouter un nouveau package, seules les étapes 1 à 4 sont nécessaires, le nom du bundle ne changeant pas.

## Arborescence d'un projet

Voir [html-css.md](html-css.md).

## Le fichier `package.json`

Voir [npm.md](npm.md).

## Installation de Webpack

Dans un terminal, à la racine du projet :

    npm install webpack

## Installation de nouveaux packages

Voir [npm.md](npm.md).

## Configuration de Webpack pour utiliser jQuery

Créer un fichier `webpack.config.js` à la racine du projet et y insérer le code suivant :

    module.exports = {
        resolve: {
            alias: {
                jquery: 'jquery/src/jquery'
            }
        }
    };

## Import des packages

Ajouter les lignes suivantes au début du fichier qui est le point d'entrée votre application (`js/main.js` par exemple) :

    import 'jquery';
    import 'bootstrap';

## Création d'un bundle

Dans le terminal, pour créer un bundle nommé `js/bundle.js` en partant du fichier `js/main.js` :

    npx webpack js/main.js js/bundle.js

## Création automatique d'un bundle

Il est possible de demander à Webpack de se mettre en mode `watch`.
C'est-à-dire de créer automatiquement le bundle dès qu'une modification dans un fichier JavaScript est détectée.

Dans le terminal :

    npx webpack --watch js/main.js js/bundle.js

## Création d'un bundle minifié

Il est possible de créer un bundle optimisé et minifié pour la production.

Dans le terminal :

    npx webpack -p js/main.js js/bundle.min.js

## Intégration du bundle

Dans votre fichier HTML, ajouter la ligne de code suivante juste avant la fermeture de la balise `body` :

    <script src="js/bundle.js" ></script>

## Doc

- [webpack](https://webpack.js.org/)
- [Getting Started](https://webpack.js.org/guides/getting-started/)
