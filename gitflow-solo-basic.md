# Git en solo, mode basic

## Création du repo en local

    # todo: créer le dossier du projet
    # todo: créer un fichier README.md dans le dossier du projet
    # todo: ouvrir un terminal
    cd [dossier-du-projet]
    git init
    git add .
    git commit

## Création du repo sur github / framagit / bitcuket

    # todo: s'il y a lieu, créer un compte sur le site
    # todo: sur le site, créer un nouveau repo (attention à donner le même nom que celui du dossier)
    # todo: sur le site, copier l'url du repo (de la forme https://github.com/[login]/[nom-du-repo].git ou git@github.com:[login]/[nom-du-repo].git)
    # todo: revenir dans le terminal
    git remote add origin https://github.com/[login]/[nom-du-repo].git
    git push -u origin master

## Enregistrer son travail

    # todo: créer ou modifier des fichiers sources
    git add [nom-du-fichier]
    # todo: répêter `git add` autant de fois que nécessaire
    git status
    # si on a un doute, vérifier le code qui sera commité
    git diff --staged
    git commit
    git push

