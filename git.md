# Git

Git est un outil de gestion de code source. Il permet d'enregistrer du code source à un instant T et de le restituer plus tard, si nécessaire. Il permet également d'entretenir en parallèle plusieurs versions d'un même code source. Cette dernière faculté permet une collaboration plus fluide entre plusieurs développeur travaillant sur un même projet.

## Installation

### Linux

Avec Debian, en tant que `root` dans un terminal, taper :

    apt install git

### MacOS

Deux solutions :

1. installer XCode
2. installer git à partir du site : [Git - Downloads](https://www.git-scm.com/downloads)

Si vous décider d'installer git à partir du site, il faut aussi exécuter ces commandes dans un terminal :

    echo "PATH=/usr/local/git/bin:\$PATH" >> ~/.bash_profile
    source ~/.bash_profile

### Windows

Utiliser l'installeur :

- [Git - Downloads](https://www.git-scm.com/downloads)

Attention : il faut choisir un éditeur de code autre que `notepad` (qui servira à taper les messages de commit).

### Configuration

Git a besoin de savoir qui est le développeur qui fait des commits.

Ouvrir un terminal :

    git config --global user.name "[nom-du-développeur]"
    git config --global user.email "[email-du-développeur]"

Il peut aussi être judicieux de configurer un accès SSH, ce qui évite de devoir taper son login et son mot de passe à chaque fois que l'on veut pusher du code sur le repo distant.

## Notions à connaître

- repository ou repo : projet géré par git
- dossier `.git` : le dossier qui contient toutes les données du repo
- fichier `.gitignore` : fichier qui contient la liste des fichiers que git ne prendra pas en compte quand on fait un `git status` ou un `git diff`; on peut aussi forcer la prise en compte de fichiers qui se trouvent dans des dossiers ignorés
- remote : repo distant
- origin : alias / pseudo du repo distant (github ou framagit par exemple) - local : repo sur un poste de travail
- commiter : enregistrer des modifications faites sur des fichiers ou des dossiers
- message de commit : ce message permet de comprendre en quoi consiste les modifications
- zone de staging : sous-dossier du dossier `.git` dans lequel git copie les fichiers quand on utilise la commande `git add`; ce sont les fichiers de cette zone qui sont commités
- branches : versions différentes d'un même projet
- branche master : la branche principale, c'est la branche par défaut
- merger : mixer le code source de deux branches
- pull request / merge request / PR : demande de code review avant de merger le code
- rebaser : couper une branche et la « re-baser » (faire une bouture) sur un autre commit
- stash : la remise; zone où l'on peut stocker temporairement des modifications en cours

## Types de branches

Attention, ceci n'est pas à prendre au pied de la lettre.

Il y a quatre catégories de branches, celles de :

- fonctionnalité
- correction de bug
- documentation
- maintenance (modifications sans impact fonctionnel)

Personnellement, j'utilise les préfixes suivants pour le nom de mes branches :

- `f-` : fonctionnalité
- `b-` : correction de bug
- `d-` : documentation
- `m-` : maintenance

Cela permet de :

- repérer facilement le type de changement qui sera commité
- m'empêcher de corriger des bugs dans une branche de fonctionnalité
- plannifier mes activités

## Commandes à connaître

- `git status` : affiche le nom de la branche courante, la synchronisation avec `origin` et l'état des fichiers
- `git log` : affiche la liste des commits
- `git diff` : affiche les différences
- `git add` : notifie à git les fichier qu'on veut commiter, c-à-d copie les fichiers dans la zone de staging
- `git reset` : notifie à git les fichier qu'on veut plus commiter, c-à-d supprime les fichiers de la zone de staging
- `git commit` : enregistre les modification
- `git clone` : duplique un repo dans le dossier courant
- `git push` : pousse les modification du repo local dans le repo distant
- `git pull` : rappatrie dans le repo local les modification du repo distant
- `git checkout [nom-de-branche]` : change de branche pour aller dans `[nom-de-branche]`
- `git checkout [nom-de-fichier]` : restaure un fichier dans son état avant modification, c-a-d annule les modifications
- `git merge [nom-de-branche]` : mixe le code de la branche `[nom-de-branche]` dans la branche courante
- `git rebase master` : couper la branche courante et la rebaser sur le dernier commit de la branche master

## Utilisation

Pour des cas d'usage typiques, voir :

- [gitflow-solo-basic.md](gitflow-solo-basic.md)
- [gitflow-solo-advanced.md](gitflow-solo-advanced.md)
- [gitflow-github-flow-basic.md](gitflow-github-flow-basic.md)

### Création du repo en local

    # todo: créer le dossier du projet
    # todo: créer un fichier README.md dans le dossier du projet
    # todo: ouvrir un terminal
    cd [dossier-du-projet]
    # initialiser le repo git
    git init
    # ajouter tous les fichiers dans la zone de staging
    git add .
    # créer un premier commit
    git commit -m "Création du repo"

### Création du repo sur github / framagit / bitcuket

    # todo: s'il y a lieu, créer un compte sur le site
    # todo: sur le site, créer un nouveau repo (attention à donner le même nom que celui du dossier)
    # todo: sur le site, copier l'url du repo (de la forme https://github.com/[login]/[nom-du-repo].git ou git@github.com:[login]/[nom-du-repo].git)
    # todo: revenir dans le terminal
    git remote add origin https://github.com/[login]/[nom-du-repo].git
    git push -u origin master

### Enregistrer des modifications

Il est important d'enregistrer des ensembles de modifications cohérents.

Par exemple, un commit peut contenir :

- la création d'une fonctionnalité (mais surtout pas plusieurs)
- l'amélioration d'une fonctionnalité
- la suppression d'une fonctionnalité
- une correction de bug
- plusieurs corrections d'orthographe
- la création de documentation
- l'amélioration de documentation
- la suppression de documentation

D'abord on ajoute les fichiers dans la zone de staging.

    # ajouter un seul fichier
    git add [nom-du-fichier]

    # ajouter plusieurs fichiers :
    git add [nom-du-fichier1] [nom-du-fichier2] [nom-du-fichier3] [...]

    # ajouter des fichiers en mode interactif (usage avancé)
    # ce mode permet notamment de n'ajouter que certaines lignes d'un fichier dans la zone de commit, au lieu d'ajouter le fichier entier
    git add -i

On vérifie que tout ce qu'on veut commiter a bien été ajouté dans la zone de staging.

    # vérifier les fichiers qui seront commités
    git status
    # vérifier le code qui sera commité
    git diff --staged

Si on se rend compte qu'on a ajouter par erreur un fichier, on peut le supprimer de la zone de staging.

    # notifier git de ne pas commiter le fichier [nom-du-fichier]
    git reset [nom-du-fichier]

Puis on commit.

    # commiter en spécifiant juste un titre de commit
    git commit -m "Ajout de la fonctionnalité foo bar baz"

    # commiter en spécifiant un titre de commit et des commentaires
    git commit
    # note: un éditeur de code s'ouvre
    # todo: taper le titre du commit dans la première ligne
    # todo: laisser une ligne vide entre le titre et les commentaires
    # todo: taper les commentaires
    # todo: enregistrer et quitter totalement l'éditeur de code
    # warning: tant que l'éditeur de code reste ouvert, la commande ne termine pas son travail

### Publier des modifications

    # pousser les modifications de la branche courante sur la branche [nom-de-branche] du repo distant et demander à git de retenir cette configuration
    git push -u origin [nom-de-branche]

    # pousser les modifications de la branche courante sur la branche par défaut du repo distant (configurée au préalable)
    git push

### Vérifier l'état du repo local

    # synchroniser les dossiers `.git` du repo distant et du repo local
    git fetch
    # vérifier de la branche courante, des commits en avance (ou pas) et des modifications en cours
    git status

    # afficher des modifications en cours (qui ne seront pas commitées)
    git diff
    # afficher des modifications qui seront commitées
    git diff --staged

    # afficher la liste des commits (en ordre anti-chronologique) avec les commentaires
    git log
    # afficher la liste simplifiée des commits (sans les commentaires)
    git log --oneline

## Doc

### Utilisation

- [Git](https://git-scm.com/)
- [Git How To: Guided Git Tutorial](https://githowto.com/)

### Git flow

Notions de base :

- [Introduction to GitLab Flow | GitLab](https://docs.gitlab.com/ce/workflow/gitlab_flow.html)
- [Git Workflow | Atlassian Git Tutorial](https://www.atlassian.com/git/tutorials/comparing-workflows)

Le git flow préconisé par github :

- [GitHub Flow – Scott Chacon](http://scottchacon.com/2011/08/31/github-flow.html)

Le git flow utilisé pour le développement du kernel linux :

- [Using git-flow to automate your git branching workflow](https://jeffkreeftmeijer.com/git-flow/)

Notions avancées :

- [My Git Workflow](https://blog.osteele.com/2008/05/my-git-workflow/)
- [Git Workflow Guide with Examples for Pros | Toptal](https://www.toptal.com/git/git-workflows-for-pros-a-good-git-guide)

