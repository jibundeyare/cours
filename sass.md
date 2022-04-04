# SASS (`node-sass`)

SASS c'est Super Awesome Style Sheet.
C'est du CSS++ !

La version `node-sass` est écrite en language JS.
Il est recommandé d'utiliser cette version plutôt que la version « classique » qui est écrite en Ruby.

Pour la version « classique » voir [sass-classique.md](sass-classique.md).

## Installation

```bash
npm install node-sass
```

## Compiler un fichier `sass` en un fichier `css`

```bash
npx node-sass style.sass > style.css
```

## Compiler un fichier `scss` en un fichier `css`

```bash
npx node-sass style.scss > style.css
```

## Auto-compiler le dossier `sass/` dans le dossier `css/` durant la pahse de dev

Les options permettent de :

- auto-compiler le code dès qu'un fichier est modifié
- rendre lisible le code compilé

```bash
npx node-sass --output-style expanded --watch --recursive --output css sass
```

## Compiler tout le dossier `sass/` dans le dossier `css/` pour la prod

Les options permettent de :

- minifié le code compilé
- générer les fichiers source maps utiles pour le déboggage

```bash
npx node-sass --output-style compressed --recursive --source-map true --source-map-contents --output css sass
```

## Doc

- [Sass: Syntactically Awesome Style Sheets](http://sass-lang.com/)

