# npm

`npm` (Node Packet Manager) est le gestionnaire de paquet de nodejs.
Il est livré avec nodejs.

## Installation

Voir [nodejs.md](nodejs.md).

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

## Utilisation de `npx` pour lancer des exécutables

La commande `npx` permet de lancer des exécutables sans devoir les installer de façon globale.
Évitez donc de faire des `npm install -g package_name` et préférez une installation locale avec `npm install package_name` puis son exécution avec `npx package_command`.

Attention toutefoi, car `npx` installe automatiquement les packages qu'il ne trouve pas.
Personnellement je n'aime pas ça, à cause de problèmes de sécurité assez évidents.
Heureusement, l'option `npx --no-install` permet d'empêcher ce comportement par défaut.

Astuce : j'ai rajouté `alias npx='npx --no-install'` dans mon fichier `~/.bashrc` pour ne pas avoir à taper `--no-install` à chaque fois.

## Installation de package globaux ou  l'erreur `EACCES permissions errors when installing packages globally`

Si vous essayez d'installer un package global sans ajouter `sudo` devant, vous verrez l'erreur `EACCES permissions errors when installing packages globally`.

Personnellement je vous déconseille d'installer des packages npm avec la commande `sudo`, vu qu'il est possible de faire autrement.
L'astuce est de configurer un dossier global dans sa home.

Voici la procédure :

```bash
# création du répertoire d'installation pour les packages globaux
mkdir -p ~/.npm-global

# configuration de npm
npm config set prefix '~/.npm-global'
# vérification
cat ~/.npmrc

# configuration de la variable d'environnement pour nodejs
echo 'export PATH=~/.npm-global/bin:$PATH' >> ~/.profile
# vérification
cat ~/.profile

# chargement du nouveau fichier ~/.profile
source ~/.profile
```

Maintenant vous pouvez installer des packages globaux sans utiliser `sudo` :

```bash
npm install -g sass
```

Vous pouvez vérifier que le package s'est bien installé dans le dossier `~/.npm-global` :

```bash
ls ~/.npm-global
```

## Doc

- [npm Documentation](https://docs.npmjs.com/)
- [package.json | npm Documentation](https://docs.npmjs.com/files/package.json)
- [Resolving EACCES permissions errors when installing packages globally | npm Docs](https://docs.npmjs.com/resolving-eacces-permissions-errors-when-installing-packages-globally)

