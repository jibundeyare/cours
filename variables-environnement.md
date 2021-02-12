# Variables d'environnement

Les variables d'environnement sont des variables qui sont accessibles via le terminal par toutes les applications.
Cela permet de configurer certaines préférences (comme l'éditeur de code à utiliser dans le terminal) ou encore la variable `PATH`.

## La variable `PATH`

Cette variable contient une liste de répertoires dans lesquels le système d'exploitation peut chercher une commande s'il ne l'a pas trouvé dans le répertoire courant.

Exemple : j'ai ouvert un terminal et je suis dans le dossier de ma home.
Je lance la commande `foo` (ou `foo.exe` sous Windows) mais le terminal me répond par une erreur du type :

    bash: foo : command not found

Alors que je sais que la commande existe dans le sous-dossier `bar` de ma home.

Je pourrais taper `bar/foo` (ou `bar\foo.exe` sous Windows) pour que la commande soit trouvée.
Mais on peut faire mieux : ajouter le chemin du répertoire dans le `PATH` pour que le système d'exploitation trouve tout seul la commande `foo`.

Après ça, la commande `foo` fonctionnera sans devoir spécifier le chemin entier.

Voir ci-dessous pour ajouter un chemin dans le `PATH` sous Linux, MacOS et Windows.

## Linux

- [Learn How to Set Your $PATH Variables Permanently in Linux](https://www.tecmint.com/set-path-variable-linux-permanently/)

## MacOS

- [Mac OS X: Set / Change $PATH Variable - nixCraft](https://www.cyberciti.biz/faq/appleosx-bash-unix-change-set-path-environment-variable/)

## Windows

- [command line - Adding directory to PATH Environment Variable in Windows - Stack Overflow](https://stackoverflow.com/questions/9546324/adding-directory-to-path-environment-variable-in-windows)
- [Setting path in Windows 7 command prompt - Super User](https://superuser.com/questions/317631/setting-path-in-windows-7-command-prompt)
- [windows variables d'environnement à DuckDuckGo](https://duckduckgo.com/?q=windows+variables+d%27environnement&ia=web)

