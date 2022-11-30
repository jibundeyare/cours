# Git

Git est un outil de gestion de code source. Il permet d'enregistrer du code source à un instant T et de le restituer plus tard, si nécessaire. Il permet également d'entretenir en parallèle plusieurs versions d'un même code source. Cette dernière faculté permet une collaboration plus fluide entre plusieurs développeur travaillant sur un même projet.

## Pourquoi utiliser git ?

Git est utile pour :

- avoir une sauvegarde sur internet de son travail
- avoir un historique de son travail (et pouvoir revenir en arrière)
- travailler en équipe (ou tout simplement partager son code)
- gérer facilement des variantes d'une application

## Travail en solo ou en groupe

- pour travailler en solo avec une méthode simple, voir [gitflow-solo-basic.md](gitflow-solo-basic.md)
- pour travailler en solo avec une méthode plus évoluée, voir [gitflow-solo-advanced.md](gitflow-solo-advanced.md)
- pour travailler en groupe avec une méthode simple, voir [gitflow-github-flow-basic.md](gitflow-github-flow-basic.md)
- pour travailler en groupe avec une méthode plus évluée, voir [gitflow-github-flow-advanced.md](gitflow-github-flow-advanced.md)

## Installation

### Linux

Avec Debian, dans un terminal, tapez :

    sudo apt install git

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

#### Utilisateur

Git a besoin de savoir qui est le développeur qui fait des commits.

Ouvrir un terminal :

    git config --global user.name "[nom-du-développeur]"
    git config --global user.email "[email-du-développeur]"

#### Éditeur

Vous pouvez aussi choisir l'éditeur de code avec lequel vous écrirez vos messages de commit.

Notes pour :
- MacOS : Nano et Vim sont livrés avec le système d'exploitation.
- Windows : Git Bash est livré avec Nano et Vim.
- Linux : Nano est livré avec le système d'exploitation, Vim est installable avec `apt`

Dans un terminal, tapez :

    git config --global core.editor [nom-de-l-éditeur]

Exemple avec l'éditeur de code Nano :

    git config --global core.editor nano

Exemple avec l'éditeur de code Vim :

    git config --global core.editor vim

Exemple avec l'éditeur de code gedit :

    git config --global core.editor gedit

Exemple avec l'éditeur de code Atom :

    git config --global core.editor "atom --wait"

Exemple avec l'éditeur de code Sublime Text 3 :

    git config --global core.editor "subl -n -w"

Exemple avec l'éditeur de code Visual Studio Code (version open source) :

    git config --global core.editor "code --wait"

Exemple avec l'éditeur de code Notepad++ (version 64 bit) :

    git config --global core.editor "'C:/Program Files/Notepad++/notepad++.exe' -multiInst -notabbar -nosession -noPlugin"

Exemple avec l'éditeur de code Notepad++ (version 32 bit) :

    git config --global core.editor "'C:/Program Files (x86)/Notepad++/notepad++.exe' -multiInst -notabbar -nosession -noPlugin"

#### Diff & merge

Il est aussi possible de définir un outil personnalisé pour comparer (diff) et fusionner (merge) des fichiers.

L'outil de diff s'utilise comme `git diff`, pour voir les différences avec une version précédente du code.
L'outil de merge ne peut être utilisé que quand il y a un conflit après un git `git pull`.
Il sert alors à résoudre les conflits.

Si vous utilisez git dans un VPS, vous n'avez pas le choix, vous devez opter pour un outil de diff ou de merge qui fonctionne dans le terminal.
Dans ce cas, `vimdiff` est idéal.

Si vous utilisez git sur votre poste de travail, vous pouvez utilisez un outil de diff graphique.
Privilégiez un outil qui est capable de faire un 3-way merge.
Ce mode de fusion, utilisé lorsqu'il y a un conflit, vous permet de voir le fichier original, la version remote, la version locale et le résultat final que vous allez conserver.
Personnellement, je recommande `kdiff3` qui est capable de résoudre tout seul des conflits simples et d'utiliser un 3-way merge sinon.
En second choix, vous pouvez utiliser `meld`.
Utiliser `vscode` n'est pas idéal car il n'est pas capable de faire de 3-way merge, même s'il ajoute la coloration syntaxique.

Exemple avec Vimdiff :

    git config --global merge.tool vimdiff
    git config --global diff.tool vimdiff

Exemple avec KDiff3 :

    git config --global merge.tool kdiff3
    git config --global diff.tool kdiff3

Exemple avec Meld :

    git config --global merge.tool meld
    git config --global diff.tool meld

Exemple avec Visual Studio Code :

    git config --global merge.tool vscode
    git config --global mergetool.vscode.cmd "code --wait \$MERGED"
    git config --global diff.tool vscode
    git config --global difftool.vscode.cmd "code --wait --diff \$LOCAL \$REMOTE"

Les commandes suivantes permettent de savoir quels outils sont supportés par git sans configuration particulière :

    # liste des outils de diff
    git difftool --tool-help

    # liste des outils de merge
    git mergetool --tool-help

**Attention** : pour utiliser l'outil de diff que vous venez de configuré, vous devez utiliser :

    git difftool

**Attention** : et pour utiliser l'outil de merge que vous venez de configuré, vous devez utiliser :

    git mergetool

La commande `git diff` utilisera toujours l'outil de diff par défaut de git (dans le terminal).

La commande `git config --global difftool.prompt false` permet en plus de zapper la question `Launch 'vimdiff|kdiff3|meld|vscode|else' [Y/n]?`avant chaque fichier.

Pour plus de détails, voir les liens dans la section [Diff et merge](#diff-et-merge) de la doc, ci-dessous.

#### Vérifier la configuration

La commande suivante permet d'afficher la configuration de git :

    cat ~/.gitconfig

Vous devriez voir quelque chose qui ressemble au résultat ci-dessous.
Ici l'utilisateur s'appelle `Foo Bar`, son email est `foo.bar@example.com` et il utilise `vscode` pour fusionner son code et visualiser les différences.

    [user]
    	email = foo.bar@example.com
    	name = Foo Bar
    [merge]
    	tool = vscode
    [mergetool "vscode"]
    	cmd = code --wait $MERGED
    [diff]
    	tool = vscode
    [difftool "vscode"]
    	cmd = code --wait --diff $LOCAL $REMOTE

#### Accès SSH

Il peut aussi être judicieux de configurer un accès SSH, ce qui évite de devoir taper son login et son mot de passe à chaque fois que l'on veut pusher du code sur le repo distant.

Voir les liens dans la section [Github ou Framagit (Gitlab) et SSH](#github-ou-framagit-gitlab-et-ssh) de la doc, ci-dessous.

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

- `git add` : notifie à git les fichier qu'on veut commiter, c-à-d copie les fichiers dans la zone de staging
- `git branch` : manipule les branches (création, suppression, liste)
- `git checkout [nom-de-fichier]` : restaure un fichier dans son état avant modification, c-a-d annule les modifications
- `git checkout [nom-de-branche]` : change de branche pour aller dans `[nom-de-branche]`
- `git clone` : duplique un repo dans le dossier courant
- `git commit` : enregistre les modification
- `git diff` : affiche les différences entre deux commits
- `git log` : affiche la liste des commits
- `git merge [nom-de-branche]` : mixe le code de la branche `[nom-de-branche]` dans la branche courante
- `git pull` : rappatrie dans le repo local les modification du repo distant
- `git push` : pousse les modification du repo local dans le repo distant
- `git rebase master` : couper la branche courante et la rebaser sur le dernier commit de la branche master
- `git reset` : notifie à git les fichier qu'on veut plus commiter, c-à-d supprime les fichiers de la zone de staging
- `git show` : affiche les modifications enregistrées dans le dernier commit
- `git status` : affiche le nom de la branche courante, la synchronisation avec `origin` et l'état des fichiers
- `git tag` : ajoute un tag (le plus souvent un numéro de version) à un commit

## Pourquoi il ne faut pas utiliser `git commit -m`

Dans la plupart des tutos, vous verrez `git commit -m` mais ce n'est pas une bonne idée.

Quand vous travaillez sur un projet réel, et surtout quand vous travaillez à plusieurs, vous avez besoin de savoir exactement ce qu'un commit apporte comme modifications.

Donner seulement un titre à votre commit, dans la ligne de commande avec l'option `-m`, ne suffira pas à expliquer le détails des modifications.
Cela va contraindre vos collaborateurs (ou vous-même) à devoir lire le code source pour comprendre ce que fait le commit.
Alors que quelques lignes d'explications auraient suffis...

Pour ajouter ces informations, vous devez laisser tomber l'option `-m` et utiliser un éditeur de code pour rédiger le message de commit.
Cela vous permettra de donner un titre à votre commit mais aussi une description détaillée.

## Messages de commit

Le message de commit se compose au minimum d'un titre.

Mais il est de bon ton de rajouter des explications supplémentaires.
Il est très important que le message de commit explique « quoi » et « pourquoi ».
Le « quoi » indique ce qui a été fait dans le commit.
Le « pourquoi » indique la raison des modifications.

Voici un exemple de message de commit :

    F Ajoute un champ "sujet" dans le formulaire de contact

    Le champ "sujet" permet aux visiteurs de préciser la raison de leur demande de contact.
    Les sujets étaient inscrits dans le corps du mail et le client n'arrivait pas à trier les mails en fonction de leur priorité.

    Github issue #123

    [php] [html]

La lettre `F` indique qu'il s'agit d'une fonctionnalité.
Ce code indique le type de tâche réalisée dans le commit.

Voici des codes possibles :

- `F` : fonctionnalité
- `B` : correction de bug
- `D` : documentation
- `M` : maintenance

Vous pouvez inventer vos propres codes.
Soyez créatifs mais surtout, soyez cohérents.

Le terme `Github issue #123` fait référence à l'identifiant d'un « ticket » (une « issue ») créé dans l'outil « Issue tracker » de Github.

Les termes `[php]` et `[html]` sont des tags qui permettent de faire une recherche rapide de tous les commits en rapport avec un type de modification de code.

Voici des tags possibles :

- `[build]` : système de build
- `[db]` : database
- `[config]`
- `[controller]`
- `[css]`
- `[design]`
- `[form]`
- `[fs]` : file system
- `[html]`
- `[js]` : Javascript
- `[model]`
- `[php]`
- `[route]`
- `[sass]`
- `[service]`
- `[text]` : contenu éditorial
- `[vue]`

Pareil qu'avec les codes de tâches, vous pouvez inventer les vôtres.
Soyez créatifs mais cohérents.

## Focus sur le contenu précis du message de commit

- [How to Write a Git Commit Message](https://chris.beams.io/posts/git-commit/)
- [How to write professional messages EFFICIENTLY?](https://driggl.com/blog/a/how-to-write-professional-commits-efficiently)
- [Online Git Commit Message Editor | Git Praise](https://gitpraise.com/)
- [tbaggery - A Note About Git Commit Messages](https://tbaggery.com/2008/04/19/a-note-about-git-commit-messages.html)
- [joelparkerhenderson/git_commit_template: Git commit template for better commit messages](https://github.com/joelparkerhenderson/git_commit_template)

## Le fichier `.gitignore`

Ce fichier dit à Git de ne pas tenir compte de certain fichier.
Ou il force la prise en compte de fichiers se trouvant dans un dossier ignoré.

Exemple d'un fichier `.gitignore` :

    /config/db.yml
    /config/mail.yml
    /cache/*
    !cache/.gitkeep

Le slash `/` qui précède les chemins désigne le dossier du repo.
Si le repo se trouve dans `~/projects/foo`, alors `/config/db.yml` désigne `~/projects/foo/config/db.yml`.

Explications :

- `/config/db.yml` : demande d'ignorer le fichier `db.yml` qui se trouve dans le dossier `config`
- `/cache/*` : demande d'ignorer tous les fichiers qui se trouve dans le dossier `cache`
- `!cache/.gitkeep` : demande de forcer la prise en compte du fichier `.gitkeep` qui se trouve dans le dossier `cache`  
  C'est donc une demande d'exception par rapport à la règle précédente

## Utilisation

Pour des cas d'usage typiques, voir :

- [gitflow-solo-basic.md](gitflow-solo-basic.md)
- [gitflow-solo-advanced.md](gitflow-solo-advanced.md)
- [gitflow-github-flow-basic.md](gitflow-github-flow-basic.md)
- [gitflow-github-flow-advanced.md](gitflow-github-flow-advanced.md)

### Création du repo en local

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

### Création du repo sur github / framagit / bitbucket

Créez le repo sur le site :

- s'il y a lieu, créez un compte sur le site
- sur le site, créez un nouveau repo (attention à donner le même nom que celui du dossier en local)

#### Pour lier votre repo local à un repo distant en utilisant le protocole HTTPS :

Sur le site, copiez l'url du repo au format HTTPS (de la forme https://github.com/[login]/[nom-du-repo].git)

Puis dans le terminal :

    git remote add origin https://github.com/[login]/[nom-du-repo].git
    git push -u origin master

#### Pour lier votre repo local à un repo distant en utilisant le protocole SSH :

Sur le site, copiez l'url du repo au format SSH (de la forme git@github.com:[login]/[nom-du-repo].git)

Puis dans le terminal :

    git remote add origin git@github.com:[login]/[nom-du-repo].git
    git push -u origin master

#### Flûte ! Je me suis trompé, il fallait du HTTPS / SSH :

Pas de panique, on peut changer le repo `origin` quand on veut.

Pour lier le repo local à un repo distant en HTTPS

    git remote set-utl origin https://github.com/[login]/[nom-du-repo].git

Ou pour lier le repo local à un repo distant en SSH

    git remote set-utl origin git@github.com:[login]/[nom-du-repo].git

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

    # ajouter des portions de fichier en mode interactif
    # ce mode permet d'ajouter des parties de fichier (des patches), au lieu d'ajouter le fichier entier
    git add -p

    # ajouter des fichiers en mode interactif (usage avancé)
    # ce mode permet de contrôler l'ajout de fichier entier ou de partie de fichier avec une interface interactive
    git add -i

On vérifie que tout ce qu'on veut commiter a bien été ajouté dans la zone de staging.

    # vérifiez les fichiers qui seront commités
    git status
    # vérifiez le code qui sera commité
    git diff --staged

Si on se rend compte qu'on a ajouter par erreur un fichier, on peut le supprimer de la zone de staging.

    # notifier git de ne pas commiter le fichier [nom-du-fichier]
    git reset [nom-du-fichier]

Puis on commit.

    # commiter en spécifiant juste un titre de commit
    git commit
    # rédiger votre message de commit
    # exemple : Ajout de la fonctionnalité foo bar baz

    # ou commiter en spécifiant un titre de commit et une description détaillée
    git commit
    # note: un éditeur de code s'ouvre
    # tapez le titre du commit dans la première ligne
    # laissez une ligne vide entre le titre et les commentaires
    # tapez les commentaires
    # enregistrez et quitter totalement l'éditeur de code
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

## J'ai un warning quand je tente un `git pull`

En effet, si vous n'avez pas configuré comment la commande `git pull` doit procédrer pour synchroniser un repo local et un repo distant, vous allez avoir le message d'erreur suivant :

    $ git pull
    warning: Tirer sans spécifier comment réconcilier les branches divergentes
    est découragé. Vous pouvez éliminer ce message en lançant une des
    commandes suivantes avant votre prochain tirage :
      git config pull.rebase false  # fusion (stratégie par défaut)
      git config pull.rebase true   # rebasage
      git config pull.ff only       # avance rapide seulement
    Vous pouvez remplacer "git config" par "git config --global" pour que
    ce soit l'option par défaut pour tous les dépôts. Vous pouvez aussi
    passer --rebase, --no-rebase ou --ff-only sur la ligne de commande pour
    remplacer à l'invocation la valeur par défaut configurée.Déjà à jour.

Mais que choisir parmi ces options ? Réponses ci-dessous.

Pour plus de détails voir l'article suivant (en anglais) : [Git warning: Pulling without specifying how to reconcile divergent branches is discouraged | Sal Ferrarello](https://salferrarello.com/git-warning-pulling-without-specifying-how-to-reconcile-divergent-branches-is-discouraged/)

#### Dans tous les cas

Optez pour l'option globale suivante et jetez un coup d'œil ci-dessous pour les options locales :

    git config --global pull.ff only

Cette option est bien dans la mesure où elle tenter d'utiliser le mode de synchronisation `fast forward` (c'est à dire un mode chronologique simple) quand c'est possible.
Si ce n'est pas possible, `git pull` affichera un message d'erreur et ne fera rien.
Ce qui vous laissera la possibilité de choisir entre le mode `git pull --ff-no` (qui force un `merge`) et le mode `git pull --rebase` (qui force un `rebase`).

#### Vous travaillez seul et uniquement sur la branche master

Dans ce cas, l'option `git config pull.ff only` est tout indiquée.

#### Vous travaillez seul sur plusieurs branches

Dans ce cas, l'option `git config pull.rebase true` est tout indiquée.

#### Vous travaillez à plusieurs (sur une ou plusieurs branches)

Pareil que dans le cas précédent, l'option `git config pull.rebase true` est tout indiquée.

## Doc

### Utilisation

- [Git](https://git-scm.com/)

### Tutriels en ligne

- [Git How To: Guided Git Tutorial](https://githowto.com/)
- [Learn Git Branching](https://learngitbranching.js.org/)

### Git flow

#### Configuration

- [Associating text editors with Git - User Documentation](https://help.github.com/articles/associating-text-editors-with-git/)
- [Git - Git Configuration](https://www.git-scm.com/book/en/v2/Customizing-Git-Git-Configuration)

#### Diff et merge

- [git - Configuring diff tool with .gitconfig - Stack Overflow](https://stackoverflow.com/questions/6412516/configuring-diff-tool-with-gitconfig)
- [Set Visual Studio Code as default git editor and diff tool – Soltys Blog](https://blog.soltysiak.it/en/2017/01/set-visual-studio-code-as-default-git-editor-and-diff-tool/)
- [How to use Visual Studio Code as the default editor for Git MergeTool - Stack Overflow](https://stackoverflow.com/questions/44549733/how-to-use-visual-studio-code-as-the-default-editor-for-git-mergetool)

#### Notions de base

- [Introduction to GitLab Flow | GitLab](https://docs.gitlab.com/ce/workflow/gitlab_flow.html)
- [Git Workflow | Atlassian Git Tutorial](https://www.atlassian.com/git/tutorials/comparing-workflows)

#### Github ou Framagit (Gitlab) et SSH

- [Connecting to GitHub with SSH - User Documentation](https://help.github.com/articles/connecting-to-github-with-ssh/)
- [GitLab and SSH keys | GitLab](https://docs.gitlab.com/ee/ssh/)

#### Le git flow préconisé par github

- [GitHub Flow – Scott Chacon](http://scottchacon.com/2011/08/31/github-flow.html)

#### Le git flow utilisé pour le développement du kernel linux

- [Using git-flow to automate your git branching workflow](https://jeffkreeftmeijer.com/git-flow/)

#### Notions avancées

- [My Git Workflow](https://blog.osteele.com/2008/05/my-git-workflow/)
- [Git Workflow Guide with Examples for Pros | Toptal](https://www.toptal.com/git/git-workflows-for-pros-a-good-git-guide)

#### Messages de commit

- [How to Write a Git Commit Message](https://chris.beams.io/posts/git-commit/)
- [How to write professional messages EFFICIENTLY?](https://driggl.com/blog/a/how-to-write-professional-commits-efficiently)
- [Online Git Commit Message Editor | Git Praise](https://gitpraise.com/)
- [tbaggery - A Note About Git Commit Messages](https://tbaggery.com/2008/04/19/a-note-about-git-commit-messages.html)
- [joelparkerhenderson/git_commit_template: Git commit template for better commit messages](https://github.com/joelparkerhenderson/git_commit_template)

