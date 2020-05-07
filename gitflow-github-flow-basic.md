# Github flow, mode basic

## Création du repo en local

Voir [gitflow-solo-basic.md](gitflow-solo-basic.md).

## Création du repo sur github / framagit / bitcuket

Voir [gitflow-solo-basic.md](gitflow-solo-basic.md).

## Commencer à travailler dans une nouvelle branche

Voir [gitflow-solo-advanced.md](gitflow-solo-advanced.md).

Après le commit, ne pas oublier de pousser sa branche sur le repo distant.

    git push -u origin [nom-de-branche]

## Reprendre le travail dans une branche existante

Avant de reprendre le travail, il faut :

- mettre à jour la branche master par rapport au repo distant
- mettre la branche de travail à jour par rapport à la branche master

Cette action est nécessaire à chaque fois qu'un merge (fait en local ou via une pull request) a lieu dans la branche `master`.

Mettons la branche master à jour.

    # mettre à jour la branche master
    git checkout master
    git pull --rebase

Maintenant, mettons notre branche de travail à jour.

    git checkout [nom-de-branche]
    # rebaser la branche courante sur la branche master
    git rebase master

S'il y a des conflits, il faut :

- vérifier quels fichiers posent problèmes
- résoudre les conflits
- ajouter les fichiers dans la zone de staging
- terminer le rebase


    # voir la liste des conflits
    git status
    # todo: résoudre les conflits en modifiant les fichiers
    git add [nom-de-fichier]
    # todo: répêter `git add` autant de fois que nécessaire
    git rebase --continue
    # note: chaque rebase se comporte comme un commit, mais le message de commit sont déjà remplis

La branche est à jour.
On peut reprendre le travail.

    # todo: créer ou modifier des fichiers sources
    git add [nom-de-fichier]
    # todo: répêter `git add` autant de fois que nécessaire
    git status
    # si on a un doute, vérifier le code qui sera commité
    git diff --staged
    git commit
    # l'option --force-with-lease est nécessaire si on a rebasé sa branche sur master avant de reprendre le travail
    git push --force-with-lease

## Enregistrer le travail fait dans une branche

    # todo: créer ou modifier des fichiers sources
    git add [nom-de-fichier]
    # todo: répêter `git add` autant de fois que nécessaire
    git status
    # si on a un doute, vérifier le code qui sera commité
    git diff --staged
    git commit
    # l'option --force-with-lease n'est pas nécessaire si on n'a pas rebasé sa branche sur master avant de travailler
    git push

## Importer le code d'une branche dans la branche master

La plupart des actions se font sur Github / Framagit / Bitbucket.
Il faut :

- créer une nouvelle pull request / merge request
- demander à ses collègue de faire du code review
- lire leurs commentaires
- s'il y a lieu, modifier son code et redemander un code review
- sinon valider la pull request / merge request
- mettre à jour la branche master locale


    # todo: sur le site, créer une nouvelle pull request / merge request

    # todo: demander à ses collègue de faire du code review
    # todo: sur le site, lire les commentaires
    # todo: s'il y a lieu, modifier son code
    git add [nom-de-fichier]
    # todo: répêter `git add` autant de fois que nécessaire
    git status
    # si on a un doute, vérifier le code qui sera commité
    git diff --staged
    git commit
    git push

    # todo: répêter « demander à ses collègue de faire du code review » autant de fois que nécessaire

    # todo: s'il n'y a plus de modifications de code à faire, valider la pull request

    # mettre à jour la branche master locale
    git checkout master
    git pull --rebase

## Ajouter des informations de numéro de version

Quand on commence à avoir beaucoup de commits, il est pratique d'ajouter un numéro de version à certain commit pour savoir quelles sont les étapes importantes.

	git checkout master

	# tagger la branche master avec une numéro de version sémantique
	git tag -a x.y.z

	# pousser les tags
	git push --tags

