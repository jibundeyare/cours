# SASS « classique »

SASS c'est Super Awesome Style Sheet.
C'est du CSS++ !

La version « classique » est écrite en language Ruby.
Il est recommandé d'utiliser la version `node-sass` éctite en language JS plutôt que la version « classique ».

Pour la version `node-sass` voir [sass.md](sass.md).

## Installation

Voir [Sass: Install Sass](https://sass-lang.com/install).

## Compiler un fichier `sass` en un fichier `css`

```bash
sass style.sass style.css
```

## Compiler un fichier `scss` en un fichier `css`

```bash
sass style.scss style.css
```

## Auto-compiler le dossier `sass/` dans le dossier `css/` durant la pahse de dev

Les options permettent de :

- auto-compiler le code dès qu'un fichier est modifié
- rendre lisible le code compilé

```bash
sass --style expanded --watch sass:css
```

## Auto-compiler tout le dossier `sass/` dans le dossier `css/` pour la prod

Les options permettent de :

- auto-compiler le code dès qu'un fichier est modifié
- minifié le code compilé

```bash
sass --style compressed --watch sass:css
```

## Doc

- [Sass: Syntactically Awesome Style Sheets](http://sass-lang.com/)

