# Github flow, mode avancé

Ce gitflow est adapté pour des petites équipes de 2 à 6 personnes environ.

Le principe de base reprend celui du git flow github basic mais avec une branche de `dev` en plus de la branche `master`.

La branche `master` contient le code livrable.

La branche `dev` contient le code à tester avant livraison.

Des branches de fonctionnalités sont créées par chaque développeur de l'équipe.

Avant de merger une branche de fonctionnalités dans la branche `dev`, le développeur demande un code review à son équipe.
Si le code est validé par l'équipe, le code est mergé dans la branche `dev`.
Les autres membres de l'équipes peuvent alors bénéficier des mises à jour la branche `dev`.

Quand un certain nombre de fonctionnalité sont prêtes dans la branche `dev`, le « lead dev » (le développeur principal) peut prendre la décision de merger la branche `dev` dans la branche `master`.

## Création du repo en local

Voir [gitflow-solo-basic.md](gitflow-solo-basic.md).

On peut créer la branche `dev`.

    git checkout -b dev

## Création du repo sur github / framagit / bitbucket

Voir [gitflow-solo-basic.md](gitflow-solo-basic.md).

Pensez aussi à pousser la branche `dev`.

    git checkout dev
    git push -u origin dev

## Commencer à travailler dans une nouvelle branche

Pour commencer une nouvelle branche, on part toujours de la branche `dev`.

On crée la nouvelle branche.

    git checkout dev
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

Après le commit, ne pas oublier de pousser sa branche sur le repo distant.

    git push -u origin [nom-de-branche]

## Reprendre le travail dans une branche existante

Avant de reprendre le travail, il faut :

- mettre à jour la branche `dev` par rapport au repo distant
- mettre la branche de travail à jour par rapport à la branche `dev`

Cette action est nécessaire à chaque fois qu'un merge (fait en local ou via une pull request) a lieu dans la branche `dev`.

Mettons la branche `dev` à jour.

    git checkout dev
    git pull --rebase

Maintenant, mettons notre branche de travail à jour.

    git checkout [nom-de-branche]
    # rebaser la branche courante sur la branche dev
    git rebase dev

S'il y a des conflits, il faut :

- vérifier quels fichiers posent problèmes
- résoudre les conflits
- ajouter les fichiers dans la zone de staging
- terminer le rebase

Voici les étapes :

    # examinez liste des conflits
    git status

    # todo: résolvez les conflits en modifiant les fichiers
    # utilisez `git mergetool` si vous avez configuré un outil de fusion

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

    # l'option --force-with-lease est nécessaire si on a rebasé sa branche sur dev avant de reprendre le travail
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

    # l'option --force-with-lease n'est pas nécessaire si on n'a pas rebasé sa branche sur dev avant de travailler
    git push

## Importer le code d'une branche de fonctionnalité dans la branche `dev`

La plupart des actions se font sur Github / Framagit / Bitbucket.
Il faut :

- créer une nouvelle pull request / merge request
- demander à ses collègue de faire du code review
- lire leurs commentaires
- s'il y a lieu, modifier son code et redemander un code review
- sinon valider la pull request / merge request
- mettre à jour la branche `dev` locale

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
    git checkout dev
    git pull --rebase

## Importer le code de la branche `dev` dans la branche `master`

Contrairement à l'import des branches de fonctionnalités dans la branche `dev` qui se fait sur github / framagit / bitbucket, l'import de la branche `dev` dans la branche `master` peut se faire sur le poste du lead dev.

Pourquoi ? Parce que l'équipe a déjà fait le code review, ce n'est pas la peine de le refaire.

On met à jour la branche `dev`.

    git checkout dev
    git pull --rebase

On met à jour la branche `master`.

    git checkout master
    git pull --rebase

On importe la branche `dev` dans la branche .
Cette action ne devrait jamais provoquer de conflits.
S'il y a des conflits, c'est qu'il n'ont pas été gérés correctement dans les branches de fonctionnalité.

    git merge dev

On peut publier.

    git push

## Ajouter des informations de numéro de version

Voir [gitflow-github-flow-basic.md](gitflow-github-flow-basic.md).

