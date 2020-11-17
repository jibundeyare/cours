# Git en solo, mode avancé

## Création du repo en local

Voir [gitflow-solo-basic.md](gitflow-solo-basic.md).

## Création du repo sur github / framagit / bitcuket

Voir [gitflow-solo-basic.md](gitflow-solo-basic.md).

## Commencer à travailler dans une nouvelle branche

On crée la nouvelle branche.

    git checkout master
    git checkout -b [nom-de-branche]

On peut commencer le travail.

    # todo: créez ou modifiez des fichiers sources
    git add [nom-de-fichier]
    # répêtez `git add` autant de fois que nécessaire
    git status
    # si vous avez un doute, vérifiez le code qui sera commité
    git diff --staged
    git commit
    # todo: rédigez votre message de commit

## Reprendre le travail dans une branche existante

Avant de reprendre le travail, il faut mettre la branche de fonctionnalité à jour par rapport à master.
Cette action est nécessaire à chaque fois qu'un merge a eu lieu dans la branche `master`.

    git checkout [nom-de-branche]
    # rebasez la branche courante sur la branche master
    git rebase master
    # s'il y a des conflits, il faut :
    # - vérifier quels fichiers posent problèmes
    # - résoudre les conflits
    # - ajouter les fichiers dans la zone de staging
    # - terminer le rebase
    # voir la liste des conflits
    git status
    # todo: résoudre les conflits en modifiant les fichiers
    git add [nom-de-fichier]
    # todo: répêter `git add` autant de fois que nécessaire
    git rebase --continue
    # note: chaque rebase se comporte comme un commit, mais les messages de commit sont déjà remplis

La branche est à jour.
On peut reprendre le travail.

    # todo: créez ou modifiez des fichiers sources
    git add [nom-de-fichier]
    # todo: répêtez `git add` autant de fois que nécessaire
    git status
    # si vous avez un doute, vérifiez le code qui sera commité
    git diff --staged
    git commit

## Enregistrer le travail fait dans une branche

    # todo: créer ou modifier des fichiers sources
    git add [nom-de-fichier]
    # todo: répêter `git add` autant de fois que nécessaire
    git status
    # si on a un doute, vérifier le code qui sera commité
    git diff --staged
    git commit
    # todo: rédigez votre message de commit

## Importer le code d'une branche dans la branche master

Cette action ne devrait jamais provoquer de conflits.
S'il y a des conflits, c'est qu'il n'ont pas été gérés correctement dans la branche.

    git checkout master
    git merge [nom-de-branche]
    git push

