# Bower deprecated

**Attention, Bower est considéré comme étant obsolète par ses créateurs.**

Voir la notice sur la home page de Bower : [https://bower.io/](https://bower.io/).

## Obtenir la liste des commandes de bower

    bower

## Installer automatiquement les paquets requis

Cette commande ne fonctionne que s'il y a déjà un ficier `bower.json`.

    bower install

## Initialiser un nouveau projet Bower (créer le fichier `bower.json`)

    bower init
    ? name nom-du-projet
    ? description
    ? main file
    ? keywords
    ? authors Foo Bar <foo.bar@example.com>
    ? license UNLICENSED
    ? homepage
    ? set currently installed components as dependencies? Yes
    ? add commonly ignored files to ignore list? Yes
    ? would you like to mark this package as private which prevents it from being ? would you like to mark this package as private which prevents it from being accidentally published to the registry? Yes

## Installer un paquet et l'ajouter à bower.json

    bower install -S [nom du paquet]

Exemple d'installation de Bootstrap :

    bower install -S bootstrap

## Désinstaller un paquet et l'enlever de bower.json

    bower uninstall -S [nom du paquet]

Exemple de désinstallation de Bootstrap:

    bower uninstall -S bootstrap

## Chercher un paquet

Cette commande permet de rechercher un package contenant le terme dans son nom ou sa description :

    bower search [terme]

Exemple de recherche du terme `calendar` :

    bower search calendar

## Obtenir des infos sur un paquet

    bower info [nom du paquet]

Exemple de demande d'infos sur la package jQuery :

    bower info jquery

