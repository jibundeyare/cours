# VSCode

Visual Studio Code (VSCode pour les intimes) est un éditeur de code libre et open source développé par Microsoft.

Vscodium est une version précompilée à partir du code source original de VS Code mais sans les modules de télémétrie et de tracking ajoutés par Microsoft.

## Auto Save

Pour activer l'auto save :

Dans le menu `File` > cochez `Auto Save`

## Données personnelles

Pour désactiver la télémétrie :

Dans le menu `File` > `Preferences` > `Settings` > tapez `telemetry` dans le champ de recherche > décochez `Telemetry: Enable Telemetry` et `Code-runner: Enable App Insights`

## PHP

- [PHP Programming with Visual Studio Code](https://code.visualstudio.com/docs/languages/php)

## Plugins

Voici quelques plugins recommandés pour faire du développement web en JS, PHP et Python :

- Name: Better PHPUnit
  Id: calebporzio.better-phpunit
  Publisher: calebporzio
- Name: Code Runner
  Id: formulahendry.code-runner
  Publisher: Jun Han
- Name: DotENV
  Id: mikestead.dotenv
  Publisher: mikestead
- Name: ESLint
  Id: dbaeumer.vscode-eslint
  Publisher: Dirk Baeumer
- Name: Live Server
  Id: ritwickdey.liveserver
  Publisher: Ritwick Dey
- Name: Live Share
  Id: ms-vsliveshare.vsliveshare
  Publisher: Microsoft
- Name: php cs fixer
  Id: junstyle.php-cs-fixer
  Publisher: junstyle
- Name: PHP Debug
  Id: felixfbecker.php-debug
  Publisher: Felix Becker
  Note: installer XDebug (voir la doc du plugin)
- Name: PHP DocBlocker
  Id: neilbrayfield.php-docblocker
  Publisher: Neil Brayfield
- Name: PHP Getters & Setters
  Id: phproberto.vscode-php-getters-setters
  Publisher: phproberto
- Name: PHP IntelliSense
  Id: felixfbecker.php-intellisense
  Publisher: Felix Becker
  Note: affecter `false` au paramètre `php.suggest.basic` de vscode
- Name: PHP Namespace Resolver
  Id: mehedidracula.php-namespace-resolver
  Publisher: Mehedi Hassan
- Name: phpcs
  Id: ikappas.phpcs
  Publisher: Ioannis Kappas
  Note: `composer global require squizlabs/php_codesniffer`
- Name: Prettier - Code formatter
  Id: esbenp.prettier-vscode
  Publisher: Esben Petersen
- Name: Python
  Id: ms-python.python
  Publisher: Microsoft
- Name: SFTP
  Id: liximomo.sftp
  Publisher: liximomo
- Name: Twig
  Id: whatwedo.twig
  Publisher: whatwedo

Vous pouvez copier-coller l'id du plugin dans le champ de recherche pour le trouver avec exactitude.

## Comment utiliser le plugin SFTP

**Dans ce tutoriel nous allons voir comment utiliser VSCode pour travailler en local mais publier et l'exécuter le code sur son VPS.**

1. Dans VSCode, installez l'extension suivante :  
  Name: SFTP  
  Id: liximomo.sftp  
  Publisher: liximomo
2. Sur le poste de développement, dans la home de votre utilisateur, créez un dossier `projects` et encore dedans un dossier `python`  
  Par exemple, pour l'utilisateur `foo` : `C:\Users\foo\projects\python`
3. Dans le VPS, faire la même chose : dans la home de votre utilisateur, créez un dossier `projects` et encore dedans un dossier `python`  
  Par exemple, pour l'utilisateur `foo` : `/home/foo/projects/python`
4. Dans VSCode, ouvrez le dossier `python` : `File` > `Open Folder` > ouvrez le dossier `projects` > ouvrez le dossier `python` > validez
5. Dans VSCode, créez la configuration SFTP : appuyez sur la touche `F1` > tapez `sftp: config` > validez
6. Dans le fichier `sftp.json` renseignez les champs suivants :

    - `host` : adresse ip de votre VPS
    - `port` : port SSH
    - `username` : nom d'utilisateur de votre VPS
    - `remotePath` : le nom dossier python dans votre VPS

  Exemple :

    {
    	"name": "My Server",
    	"host": "123.123.123.123",
    	"protocol": "sftp",
    	"port": 54321,
    	"username": "foo",
    	"remotePath": "/home/foo/python",
    	"uploadOnSave": true
    }

7. Dans vscode, ouvrez une connexion SSH : appuyez sur la touche `F1` > tapez `sftp: open ssh in terminal` > validez
8. Dans VSCode, si le terminal vous demande un mot de passe, tapez le mot de passe de l'utilisateur de votre VPS
9. Dans VSCode, le terminal devrait être connecté à votr VPS
10. Dans VSCode, Créer un nouveau fichier nommé `hello.py` et tapez le code suivant :

    print("Hello World!")

11. Dans VSCode, depuis le terminal, tapez les commandes suivantes :

    cd python
    python hello.py

  Si un message `Hello World!` s'affiche dans le terminal, vous avez réussi !

## Doc

- [Visual Studio Code - Code Editing. Redefined](https://code.visualstudio.com/)
- [VSCodium - Open Source Binaries of VSCode](https://vscodium.com/)

