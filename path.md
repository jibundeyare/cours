# Path

## Chemin relatif

Imaginez que vous êtes perdu dans une ville.
Vous demandez le chemin de la mairie à un passant.
Celui-ci vous donne le chemin à suivre à partir de la où vous êtes.
C'est un chemin relatif et ce chemin ne sera valable que depuis l'endroit où vous êtes.
Si vous êtes à un autre endroit, vous n'allez pas pouvoir utiliser ce chemin pour vous rendre à la mairie.

Un chemin relatif est le chemin d'un fichier ou d'un dossier en partant du dossier où l'on se trouve.

C'est pourquoi un chemin relatif ne commence jamais par un slash `/`.

## Chemin absolu

Imaginez que vous êtes dans un bâtiment administratif.
Vous voulez vous rendre à deux bureau différents et vous demandez votre le chemin à l'accueil.
La personne vous donne le chemin des deux bureaux en partant du hall d'entrée.
Vous allez au premier bureau mais il y a trop d'attente alors vous retournez dans le hall d'entrée et vous vous rendez au second bureau.
Ces deux chemins partent du hall d'entrée, vous pouvez donc réutiliser ces chemins si vous repartez du hall d'entrée.
Le hall d'entrée fait office de point fixe, l'équivalent de la racine (le symbole slash `/`).

Un chemin absolu est le chemin d'un fichier ou d'un dossier en partant de la racine (le symbole slash `/`).

C'est pourquoi un chemin absolu commence toujours par un slash `/`.

## Arborescence

Imaginons que nous sommes l'utilisateur `foo` et que notre dossier a la structure suivante :

    /home
        /foo
            /projets
                /baz
                    src/
                        Controller/
                        .../
                /lorem
                    .../
                .../

## Exemples

### Chemin relatif

Admettons que nous sommes dans le dossier suivant :

    /home/foo

Pour aller dans le dossier `/home/foo/projets/baz`, il faudrait utiliser le chemin relatif suivant :

    cd projets/baz

Maintenant, admettons que nous sommes dans le dossier suivant `/home/foo/projets/baz`.

Pour aller dans le dossier `/home/foo/projets/baz/src/Controller`, il faudrait utiliser le chemin relatif suivant :

    cd src/Controller

Pour aller dans le dossier `/home/foo/projets/lorem`, il faudrait utiliser le chemin relatif suivant :

    cd ../lorem

### Chemin absolu

Admettons que nous sommes dans le dossier suivant :

    /home/foo

Pour aller dans le dossier `/home/foo/projets/baz`, il faudrait utiliser le chemin absolu suivant :

    cd /home/foo/projets/baz

Maintenant, admettons que nous sommes dans le dossier suivant `/home/foo/projets/baz`.

Pour aller dans le dossier `/home/foo/projets/baz/src/Controller`, il faudrait utiliser le chemin absolu suivant :

    cd /home/foo/projets/baz/src/Controller

Pour aller dans le dossier `/home/foo/projets/lorem`, il faudrait utiliser le chemin absolu suivant :

    cd /home/foo/projets/lorem
