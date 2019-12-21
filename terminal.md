# Terminal

Le terminal est un outil très important pour les développeurs.
Il permet de réaliser des tâches par lot ou de répéter très facilement.

Il est également possible d'écrire des scripts pour automatiser des tâches récurrentes.

## Types

### Windows

Windows est livré avec `cmd.exe` et le Power Shell.
Mais il est possible d'installer Bash sous Windows aussi, notamment en installant Git.
Voir [git.md](git.md).

### Linux et MacOS

Linux et MacOS sont livrés avec Bash.

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

### Windows

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

### Linux et MacOS

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

Afficher le contenu du dossier avec le fichier cachés :

    ls -A

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

## Fichiers cachés

### Macos et Linux

Les fichiers dont le nom commence par un point sont cachés.

Exemple : `.htaccess`

## Path (chemin)

Pour être à l'aise dans le terminal, vous devez connaître la différence entre un chemin absolu et un chemin relatif.

Pour en savoir plus, voir [path.md](path.md).

## Doc

### Windows

- [CMD.exe (Command Shell) - Windows CMD - SS64.com](https://ss64.com/nt/cmd.html)
- [PowerShell commands - PowerShell - SS64.com](https://ss64.com/ps/)
- [PowerShell.exe Command Line Help | Microsoft Docs](https://docs.microsoft.com/en-us/powershell/scripting/components/console/powershell.exe-command-line-help?view=powershell-6)
- [batch file - How do I run two commands in one line in Windows CMD? - Stack Overflow](https://stackoverflow.com/questions/8055371/how-do-i-run-two-commands-in-one-line-in-windows-cmd)
- [windows - How to execute multiple commands in a single line - Stack Overflow](https://stackoverflow.com/questions/13719174/how-to-execute-multiple-commands-in-a-single-line)

### Linux et MacOS

- [Bash Reference Manual](https://www.gnu.org/software/bash/manual/bash.html)

