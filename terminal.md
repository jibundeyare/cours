# Terminal

Le terminal est un outil très important pour les développeurs.
Il permet de réaliser des tâches par lot ou de répéter très facilement.

Il est également possible d'écrire des scripts pour automatiser des tâches récurrentes.

## Types

### Linux et MacOS

Linux et MacOS sont livrés avec Bash.

### Windows

Windows est livré avec `cmd.exe` et le Power Shell.
Mais il est possible d'installer Bash sous Windows aussi, notamment en installant Git.
Voir [git.md](git.md).

## Historique et complétion automatique

Taper du code dans le terminal est relativement pénible.
C'est pourquoi deux choses peuvent énormément vous faciliter la vie :

- l'historique : tout ce que vous avez tapé et validé avec la touche « entrer » peut être réutilisé
- la complétion automatique : le terminal peut finir les mots que vous commencez

### Historique

Si vous utilisez les flèches du haut et du bas, vous verrez apparaître des commandes que vous avez déjà validé. En haut pour remonter dans le passé, en bas pour revenir dans le présent.

Après, vous pouvez également modifier une commande déjà validée en vous déplaçant de gauche à droite avec les flèches.

Vous pouvez valider la nouvelle commande sans aller à la fin de la ligne.
La touche « entrer » fonctionne depuis n'importe où dans la ligne.

### Complétion automatique

La touche de tabulation vous permet de compléter le nom des commandes, des dossiers ou des fichiers s'ils sont reconnus par votre terminal.

Par exemple, avec `cmd.exe`, au lieu de taper `cd C:\Wamp64\www\my_project` en entier, je peux taper `cd C:\Wa<tab>\w<tab>\my<tab>`.

Si la tabulation ne complète pas entièrement un nom, il y a deux possibilités :

1. Il y a deux fichiers ou dossiers qui portent des noms semblables (par exemple `my_symfony_3_4_project` et `my_symfony_4_2_project`)  
  Vous devez rajouter la lettre qui permettra au terminal de choisir le bon nom : `my<tab>3<tab>` pour obtenir `my_symfony_3_4_project`
2. Le fichier ou le dossier que vous cherchez ne se trouve pas à l'endroit  
  Appuyez plusieurs fois sur la touche tabulation pour voir toutes les propositions possibles

Avec `cmd.exe`, si votre terminal affiche vraiment des tabulation (plusieurs espaces) au lieu de faire de la complétion automatique, il doit y avoir moyen de configurer cela.
Mais je ne sais pas comment.

## Utilisation

### Linux et MacOS

En général, il est possible d'obtenir de l'aide à propos d'une commande en ajoutant l'option `--help`.

Exemple :

    cd --help

Pour afficher le manuel détaillé d'une commande, vous pouvez utiliser la commande `man` ou `info`.

Exemple :

    man cd

ou :

    info cd

*Note : pour sortir du manuel, tapez `q` comme `quit`.*

Se rendre dans un dossier :

    cd [nom du dossier]

Se rendre dans sa home :

    cd ~

Se rendre dans le dossier de travail avec Linux :

    cd projects

Se rendre directement dans le dossier `my_project` avec Linux :

    cd projects/my_project

Se rendre dans le dossier de travail de MAMP :

    cd /Applications/MAMP/htdocs

Se rendre directement dans le dossier `my_project` (avec MAMP) :

    cd /Applications/MAMP/htdocs/my_project

Si le nom du chemin comporte des espaces, il faut l'entourer de guillemets.

Par exemple, pour se rendre dans le dossier `/home/user/my project` :

    cd "/home/user/my project"

Afficher le chemin du dossier actuel :

    pwd

Afficher le contenu du dossier actuel :

    ls

Afficher le contenu du dossier actuel avec les droits :

    ls -l

Afficher le contenu du dossier avec les fichiers cachés :

    ls -A

Afficher le contenu du dossier de façon récursive :

    ls -R

Effacer tout l'écran :

    clear

Créer un dossier :

    mkdir [nom du dossier]

Supprimer un dossier

    rmdir [nom du dossier]

Copier un fichier :

    cp [nom de fichier source] [nom de fichier cible]

Déplacer un fichier :

    mv [nom de fichier] [dossier cible]

Renomer un fichier :

    mv [nom de fichier original] [nouveau nom de fichier]

Supprimer un fichier (attention : impossibilité de récupérer le fichier par après) :

    rm [nom de fichier]

Créer un fichier vide :

    touch [nom de fichier]

Afficher le contenu d'un fichier :

    cat [nom de fichier]

Écraser le contenu d'un fichier :

    echo "[texte]" > [nom de fichier]

Exemple : créer un fichier nommé `hello.txt` contenant le texte `"Hello World!"`

    echo "Hello World!" > hello.txt

Ajouter du contenu dans un fichier :

    echo "[texte]" >> [nom de fichier]

Exemple : ajouter le texte `"Hello Universe!"` dans le fichier `hello.txt`

    echo "Hello Universe!" >> hello.txt

Afficher l'arborescence des dossiers :

    tree -d -L [niveaux] [nom de dossier]

Exemple : afficher l'arborescence de la racine `/` jusqu'aux sous-dossiers de 1er niveau seulement

    tree -d -L 1 /

Lancer une commande avec des droits d'administrateur :

    sudo [commande]

Exemple : afficher la liste des fichiers du dossier `/root`

    sudo ls /root

Pour copier un ou des fichiers d'une machine à une autre, vous avez la commande `scp`.

Voici quelques exemples :

    # copie d'un fichier de la home locale vers la home d'un serveur
    scp ./foo 123.123.123.123:~/

    # copie d'un fichier de la home locale vers la home d'un serveur avec renommage
    scp ./foo 123.123.123.123:~/bar

    # copie d'un dossier de la home locale vers la home d'un serveur
    scp -r ./foo 123.123.123.123:~/

    # copie d'un dossier de la home locale vers la home d'un serveur avec renommage
    scp -r ./foo 123.123.123.123:~/bar

    # copie d'un fichier de la home d'un serveur vers la home locale
    # il suffit d'inverser la source et la cible
    scp 123.123.123.123:~/foo ./

    # copie d'un fichier de la home locale vers la home d'un serveur
    # mais avec un port SSH personnalisé
    scp -P 54321 ./foo 123.123.123.123:~/

#### Recherche de texte

La commande `grep` permet de rechercher un mot clé à l'intérieur d'un fichier.

La commande `fgrep` permet de rechercher un mot clé à l'intérieur de tous les fichiers contenus dans un dossier.

Recherche insensible à la casse (sans prise en compte des majuscules et minuscules) du mot clé `foo` à l'intérieur du fichier `bar.txt` :

    grep -i foo bar.txt

Recherche insensible à la casse du mot clé `foo` à l'intérieur de tous les fichiers du dossier `bar` :

    fgrep -ir foo bar

La même commande mais sans l'affichage du texte trouvé (seulement la liste des fichiers qui contiennent le mot clé) :

    fgrep -ilr foo bar

#### Historique des commandes tapées

Cette commande affiche l'historique des commandes tapées :

    history

En combinant `grep` et `history`, on peut rechercher un mot clé dans l'historique des commandes tapées.

Recherche du mot clé `foo` dans l'historique des commandes tapées :

    history | grep -i foo

Vous devriez obtenir un résultat du type :

     1749  foo
     2002  history | grep -i foo

Si vous voulez exécuter une commande précédemment tapée, vous pouvez utiliser le point d'exclamation `!` et le numéro de la ligne pour ne pas devoir tout retaper :

    !1749

#### J'ai oublié d'utiliser `sudo`

Si vous voulez relancer la commande précédente en ajoutant `sudo` devant, utiliser les double point d'exclamation `!!` :

    sudo !!

#### Recherche de fichiers dans des dossiers

La commande `find` permet de rechercher des fichiers en fonction de leur nom, leur type, etc.
C'est une commande très riche.
Le mieux est de consulter sa page man avec `man find`.

Recherche insensible à la casse (sans prise en compte des majuscules et minuscules) de tous les fichiers ayant l'extension `.txt` dans le dossier courant (le dossier `.`) :

    find . -type f -iname "*.txt"

Recherche insensible à la casse de tous les dossiers ayant le nom `src` dans le dossier `projects` :

    find projects -type d -iname "src"

### Windows

Cette section est censée reproduire à peu près la section Linux.
Mais il y a des différences, certaines commandes ne sont pas reproduites.
(Et puis je connais moins bien Windows.)

En général, il est possible d'obtenir de l'aide à propos d'une commande en ajoutant l'option `/?`.

Exemple :

    cd /?

Se rendre dans un dossier :

    cd [nom du dossier]

Se rendre dans sa home :

    cd %HOMEPATH%

Se rendre dans le dossier de travail de Wamp Server :

    cd C:\wamp64\www

Se rendre directement dans le dossier `my_project` :

    cd C:\wamp64\www\my_project

Si le nom du chemin comporte des espaces, il faut l'entourer de guillemets.

Par exemple, pour se rendre dans le dossier `C:\Users\user\my project`:

    cd "C:\Users\user\my project"

Changer de disque (sur Windows) :

    [lettre du disque]:

Par exemple, pour aller dans le disque `D:` avec Windows :

    D:

Afficher le chemin du dossier actuel :

    cd

Afficher le contenu du dossier actuel :

    dir

Afficher le contenu du dossier avec les fichiers cachés :

    dir /a

Afficher le contenu du dossier et des sous-dossiers avec les fichiers cachés :

    dir /s /a

Afficher les fichiers « système » d'un dossier :

    dir /a:s

Afficher lss fichiers « système » d'un dossier et des sous-dossiers :

    dir /s /a:s

Effacer tout l'écran :

    cls

Créer un dossier :

    mkdir [nom du dossier]

Supprimer un dossier

    rmdir [nom du dossier]

Copier un fichier :

    copy [nom de fichier source] [nom de fichier cible]

Déplacer un fichier :

    move [nom de fichier] [dossier cible]

Renomer un fichier :

    rename [nom de fichier original] [nouveau nom de fichier]

Supprimer un fichier (attention : impossibilité de récupérer le fichier par après) :

    del [nom de fichier]

Afficher le contenu d'un fichier :

    type [nom de fichier]

Créer un fichier vide :

    type nul >> [nom de fichier]

ou :

    copy nul [nom de fichier]

Écraser le contenu d'un fichier :

    echo [texte] > [nom de fichier]

Exemple : créer un fichier nommé `hello.txt` contenant le texte `"Hello World!"`

    echo Hello World! > hello.txt

Ajouter du contenu dans un fichier :

    echo [texte] >> [nom de fichier]

Exemple : ajouter le texte `"Hello Universe!"` dans le fichier `hello.txt`

    echo Hello Universe! >> hello.txt

### Recherche de texte

La commande `findstr`

Recherche insensible à la casse (sans prise en compte des majuscules et minuscules) du mot clé `foo` à l'intérieur du fichier `bar.txt` :

    findstr /i foo bar.txt

Recherche insensible à la casse du mot clé `foo` à l'intérieur de tous les fichiers du dossier `bar` :

    findstr /s /i foo bar

La même commande mais sans l'affichage du texte trouvé (seulement la liste des fichiers qui contiennent le mot clé) :

    findstr /s /i /m foo bar

- [findstr | Microsoft Docs](https://docs.microsoft.com/en-us/windows-server/administration/windows-commands/findstr)

#### Recherche de fichiers dans des dossiers

La commande `dir` permet de rechercher des fichiers en fonction de leur nom, leur type, etc.

Recherche insensible à la casse (sans prise en compte des majuscules et minuscules) de tous les fichiers ou dossiers ayant l'extension `.txt` dans le dossier courant :

    dir /s *.txt

Recherche insensible à la casse de tous les fichiers ou dossiers ayant l'extension `.txt` dans le sous-dossier `projects` (mais pas les sous-sous-dossiers) :

    dir /s projects\*.txt

- [dir | Microsoft Docs](https://docs.microsoft.com/en-us/windows-server/administration/windows-commands/dir)

## Les wildcards (joker en français)

Le symbole étoile `*` permet de remplacer une suite de caractères arbitraire.
On s'en sert la plupart du temps pour sélectionner un ensemble de fichier sans devoir taper tous leurs noms.

Par exemple, la commande suivante permet de lister tous les fichiers ayant l'extension `.png` :

    ls *.png

Autre exemple, la commande suivante permet de copier tous les fichiers commençant par `foo` dans le dossier `bar` :

    cp foo* bar/

**Note : Le comportement du symbole étoile `*` est le même sous Linux, MacOS et Windows.**

## Fichiers cachés

### MacOS et Linux

Les fichiers dont le nom commence par un point sont cachés.

Exemple : `.htaccess`

### Windows

Les fichiers cachés ont un attribut « fichier caché » que l'on peut modifier en faisant un click droit sur le fichier puis en choisissant « propriété ».

On peut aussi utiliser la commande `attrib` pour changer l'attribut « caché » ou « non caché ».

Pour cacher le fichier `foo` :

    attrib +h foo

Pour ne plus cacher le fichier `foo` :

    attrib -h foo

- [attrib | Microsoft Docs](https://docs.microsoft.com/en-us/windows-server/administration/windows-commands/attrib)

## Path (chemin)

Pour être à l'aise dans le terminal, vous devez connaître la différence entre un chemin absolu et un chemin relatif.

Pour en savoir plus, voir [path.md](path.md).

## Enchaîner plusieurs commandes en une seule ligne

Vous pouvez utiliser la double esperluette `&&` pour enchaîner deux commandes en une seule ligne.

    ls foo && ls bar

**Note : Le comportement du symbole double esperluette `&&` est le même sous Linux, MacOS et Windows.**

## Redirection de la sortie standard avec le symbole `>`

Ou comment enregistrer la sortie d'une commande dans un fichier.

Ceci fonctionne sous Windows, MacOS et Linux.

Vous pouvez taper votre commande comme d'habitude et ajouter le symbole de redirection `>` et spécifier le fichier.

Exemple pour rediriger la sortie de `ls -al` vers le fichier `list.txt` :

		ls -al > list.txt

## Chaînage de la sortie standard vers l'entrée standard avec le symbole « pipe » `|`

Ou comment utiliser la sortie d'une commande comme entrée pour une autre commande.

Cette fonctionnalité vous permet de chaîner des commandes et de traiter les données sortant d'une commande avec une autre commande.

Note : « pipe » veut dire tuyau en anglais.

Exemple pour compter le nombre de fichiers du répertoire courant :

		ls | wc -l

Explication : la commande `ls` affiche la liste des fichiers du dossier courant et la commande `wc -l` compte le nombre de lignes.

## Terminus (le jeu)

[Terminus](http://luffah.xyz/bidules/Terminus/)

Arg je suis bloqué.e dans Terminus pour trouver le mot de passe de sudo, je pense qu'il y a un problème dans le jeu avec la commande grep.

> Dans "PlusDeFichiersNoyau"
> grep password *.txt

## Doc

### Linux et MacOS

- [Bash Reference Manual](https://www.gnu.org/software/bash/manual/bash.html)
- [Apprendre à utiliser le shell Bash - Pierre Giraud](https://www.pierre-giraud.com/shell-bash/)
- [Terminus](http://luffah.xyz/bidules/Terminus/)
- [101 commandes indispensables sous linux – Buzut](https://buzut.net/101-commandes-indispensables-sous-linux/?fbclid=IwAR29L50ovusl2kC4gFs-ixJhq5ojQEH6jKWBNWNBCS285J4VnLjPSRORJ7w)

### Windows

- [CMD.exe (Command Shell) - Windows CMD - SS64.com](https://ss64.com/nt/cmd.html)
- [PowerShell commands - PowerShell - SS64.com](https://ss64.com/ps/)
- [PowerShell.exe Command Line Help | Microsoft Docs](https://docs.microsoft.com/en-us/powershell/scripting/components/console/powershell.exe-command-line-help?view=powershell-6)
- [batch file - How do I run two commands in one line in Windows CMD? - Stack Overflow](https://stackoverflow.com/questions/8055371/how-do-i-run-two-commands-in-one-line-in-windows-cmd)
- [windows - How to execute multiple commands in a single line - Stack Overflow](https://stackoverflow.com/questions/13719174/how-to-execute-multiple-commands-in-a-single-line)
- [findstr | Microsoft Docs](https://docs.microsoft.com/en-us/windows-server/administration/windows-commands/findstr)
- [dir | Microsoft Docs](https://docs.microsoft.com/en-us/windows-server/administration/windows-commands/dir)
- [attrib | Microsoft Docs](https://docs.microsoft.com/en-us/windows-server/administration/windows-commands/attrib)

