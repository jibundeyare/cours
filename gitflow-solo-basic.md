# Git en solo, mode basic

## Création du repo en local

    # créez le dossier du projet
    mkdir [dossier-du-projet]
    # allez dans le dossier du projet
    cd [dossier-du-projet]
    # créez un fichier `README.md` dans le dossier du projet
    nano README.md
    # todo: rédigez le contenu de la doc
    # initialisez le repo git
    git init
    # ajoutez le fichier `README.md` dans la zone de staging
    git add README.md
    # créez un premier commit
    git commit
    # todo: rédigez votre message de commit

## Création du repo sur github / framagit / bitbucket

    # s'il y a lieu, créez un compte sur le site
    # sur le site, créez un nouveau repo (attention à donner le même nom que celui du dossier)
    # sur le site, copiez l'url du repo au format SSH (de la forme `git@github.com:[login]/[nom-du-repo].git`)
    # exemple avec le login `foo` et le repo `bar` : git@github.com:foo/bar.git
    # revenez dans le terminal
    git remote add origin git@github.com:[login]/[nom-du-repo].git
    git push -u origin master

## Enregistrer son travail

    # todo: créez, modifier ou supprimez des fichiers sources
    # ajoutez les fichiers dans la zone de staging
    git add [nom-du-fichier]
    # répêtez `git add` autant de fois que nécessaire
    git status
    # si vous avez un doute, vérifiez le code qui sera commité
    git diff --staged
    git commit
    # todo: rédigez votre message de commit
    git push

