# Github flow, mode basic

Ce gitflow est adapté pour des petites éqiupes de 2 à 6 personnes environ.

Le principe de base reprend celui du git flow solo avancé en y ajoutant du code review par les paires.

La branche master contient le code livrable.
Des branches de fonctionnalités sont créées par chaque développeur de l'équipe.
Avant de merger une branche de fonctionnalités dans la branche master, le développeur demande un code review à son équipe.
Si le code est validé par l'équipe, le code est mergé.
Les autres membres de l'équipes peuvent alors bénéficier des mises à jour la branche master.

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

Voici les étapes :

    # examinez liste des conflits
    git status

    # todo: résolvez les conflits en modifiant les fichiers

    # répêtez `git add` autant de fois que nécessaire
    git add [nom-de-fichier]

    # `git rebase` se comporte comme `git commit`, mais les messages sont préremplis
    git rebase --continue

La branche est à jour.
On peut reprendre le travail.

    # todo: créez ou modifiez des fichiers sources

    # examinez l'état du repo
    git status

    # répêtez `git add` autant de fois que nécessaire
    git add [nom-de-fichier]

    # si vous avez un doute, vérifiez le code qui sera commité
    git diff --staged

    # si tout est bon, vous pouvez commiter
    git commit

    # l'option --force-with-lease est nécessaire si on a rebasé sa branche sur master avant de reprendre le travail
    git push --force-with-lease

## Enregistrer le travail fait dans une branche

    # todo: créez ou modifiez des fichiers sources

    # examinez l'état du repo
    git status

    # répêtez `git add` autant de fois que nécessaire
    git add [nom-de-fichier]

    # si vous avez un doute, vérifiez le code qui sera commité
    git diff --staged

    # si tout est bon, vous pouvez commiter
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

Voici les étapes :

    # todo: sur github, créer une nouvelle pull request / merge request
    # todo: demandez à voss collègue de faire du code review
    # todo: sur github, lisez leurs commentaires
    # todo: s'il y a lieu, modifiez votre code

    # examinez l'état du repo
    git status

    # répêtez `git add` autant de fois que nécessaire
    git add [nom-de-fichier]

    # si vous avez un doute, vérifiez le code qui sera commité
    git diff --staged

    # si tout est bon, vous pouvez commiter
    git commit

    # pousser le code sur github
    git push

    # todo: répêtez la demande du code review aux collègues autant de fois que nécessaire
    # todo: s'il n'y a plus de modifications de code à faire, valider la pull request sur github

    # mettez à jour la branche master locale
    git checkout master
    git pull --rebase

## Ajouter des informations de numéro de version

Quand on commence à avoir beaucoup de commits, il est pratique de marquer certains commits avec un numéro de version.
Ce numéro de version correspond à une « release » (un livrable).

	git checkout master

	# taggez le dernier commit de la branche master avec un numéro de version sémantique
	git tag -a x.y.z

	# poussez les tags
	git push --tags

