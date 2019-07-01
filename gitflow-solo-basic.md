# Git en solo, mode basic

## Création du repo en local

    # créez le dossier du projet
    # créez un fichier README.md dans le dossier du projet
    # ouvrez un terminal
    cd [dossier-du-projet]
    # initialisez le repo git
    git init
    # ajoutez tous les fichiers dans la zone de staging
    git add .
    # créez un premier commit
    git commit -m "Création du repo"

## Création du repo sur github / framagit / bitcuket

    # s'il y a lieu, créez un compte sur le site
    # sur le site, créez un nouveau repo (attention à donner le même nom que celui du dossier)
    # sur le site, copiez l'url du repo (de la forme https://github.com/[login]/[nom-du-repo].git ou git@github.com:[login]/[nom-du-repo].git)
    # revenez dans le terminal
    git remote add origin https://github.com/[login]/[nom-du-repo].git
    git push -u origin master

## Enregistrer son travail

    # créez ou modifier des fichiers sources
    git add [nom-du-fichier]
    # répêtez `git add` autant de fois que nécessaire
    git status
    # si vous avez un doute, vérifiez le code qui sera commité
    git diff --staged
    git commit
    git push

