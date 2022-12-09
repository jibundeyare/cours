# Git en solo, mode basic

Ce gitflow est adapté pour travailler seul sur un projet simple.

Simple ça veut dire, par exemple :

- qu'il y a une version du projet
- que dès qu'on rajoute du code, il est prêt à être publié
- qu'on a pas besoin d'avoir un historique nickel
- etc

## Création du repo en local

    # créez le dossier du projet
    mkdir [dossier-du-projet]
    # allez dans le dossier du projet
    cd [dossier-du-projet]
    # ouvrez le dossier avec vscode
    code .
    # initialisez le repo git
    git init
    # todo: créez un fichier `README.md` à la racine du projet
    # ajoutez le fichier `README.md` dans la zone de staging
    git add README.md
    # créez un premier commit
    git commit
    # todo: rédigez votre message de commit

## Création du repo sur Github

S'il y a lieu, créez un compte sur le site.
Sur le site, créez un nouveau repo (attention à donner le même nom que celui du dossier).

Attention : sur le site, sélectionnez le protocole SSH.
Remarquez que le chemin d'accès du repo est de la forme `git@github.com:[login]/[nom-du-repo].git`.
Exemple : avec le login `foo` et le repo `bar` ça donne `git@github.com:foo/bar.git`.

Dans le terminal, adaptez [login] et [nom-du-repo] et tapez :

    git remote add origin git@github.com:[login]/[nom-du-repo].git
    git branch -M main
    git push -u origin main

## Enregistrer son travail

Créez, modifier ou supprimez des fichiers sources.

    # optionnel: vérifier l'état du repo
    git status
    # optionnel: vérifiez le code qui sera commité
    git diff
    
    # ajoutez les fichiers dans la zone de staging
    git add [nom-du-fichier]
    # répêtez `git add` autant de fois que nécessaire
    
    # optionnel: vérifier l'état du repo
    git status
    # optionnel: vérifiez le code qui sera commité
    git diff --staged
    
    # créez un commit
    git commit
    # todo: rédigez votre message de commit
    # envoyez votre commit sur github
    git push

