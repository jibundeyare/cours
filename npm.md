# npm

npm (Node Packet Manager) est le gestionnaire de paquet de nodejs.
Il est livré avec nodejs.

## Le fichier `package.json`

Ce fichier recense la liste des packages déjà installés.
Bien sûr, au début cette liste est vide.

Pour pouvoir installer des packages, il faut que ce fichier soit présent.

### Création du fichier `package.json`

Dans un terminal, à la racine du projet :

    npm init -y

Éditez le fichier si besoin.

## Installation de packages déjà référencés

Si un fichier `package.json` est déjà présent dans le dossier, il est possible d'installer en une seule fois tous les packages qui y sont référencés.

Dans le terminal :

    npm install

## Recherche de nouveaux packages

Dans un terminal, pour rechercher jQuery :

    npm search jquery

## Installation de nouveaux packages

Installons par exemple jQuery et Bootstrap.

Dans le terminal, pour installer jQuery :

    npm install jquery

Dans le terminal, pour installer Bootstrap 4 :

    npm install bootstrap

Ou, dans le terminal, pour installer Bootstrap 3 (l'ancienne version) :

    npm install bootstrap@3.3.7

## Doc

- [npm Documentation](https://docs.npmjs.com/)
- [package.json | npm Documentation](https://docs.npmjs.com/files/package.json)
