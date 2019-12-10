# Doc (`README.md`)

La documentation technique est d'une importance capitale.
Elle est malheureusement très souvent négligée par les développeurs.
Pourtant c'est avant tout la documentation qui conditionne le succès d'un projet open source, par exemple.

## Format

Pour le format de la doc :

- rédigez votre documentation au format markdown
- sinon utilisez un autre format comme [AsciiDoc](http://asciidoc.org/) ou [Sphinx](http://www.sphinx-doc.org/en/master/)

Pour en savoir plus sur la syntaxe de Markdown, voir [markdown.md](markdown.md).

## Contenu

Voici le genre d'informations qui doit figurer dans votre documentation :

- description en quelques mots
- pré-requis
  - type d'OS compatible
  - version des logiciels nécessaires
  - posséder un compte chez un fournisseur tiers (google, serveur de mails, etc)
  - pré-requis optionnels (serveur web local, etc)
- installation
  - démarrer à partir du `git clone`
  - détailler toutes les étapes nécessaires comm si c'était un script bash
  - idéalement, il n'y a qu'à copier-coller pour installer
- utilisation
	- démarrage d'un serveur web
	- quel page ouvrir dans le navigateur
- infos utiles
  - poid et taille recommandés des images
  - commandes pour lancer des procédures automatisées (script qui retaille des images, génération de sprite map, etc)
- mentions légales
  - auteur des images et source
  - licence qui s'applique au projet

## Gabarit / modèle / template de `README.md`

Voici un modèle de fichier `README.md` en français :

    # Nom du projet

    Description courte et non technique du projet

    ## Prérequis

    - PHP 7.x
    - MySQL 5.x
    - Apache 2.x
    - Npm x.x

    ## Installation

        git clone https://github.com/editeur/projet.git
        cd projet
        composer install
        php bin/console doctrine:migrations:migrate

    Pour charger les données nécessaires au bon fonctionnement :

        php bin/console doctrine:fixtures:load --group=prod

    ## Utilisation

    Lancez un serveur web :

        symfony serve

    Puis ouvrez le lien [http://localhost:8000](http://localhost:8000).

    - /admin : back office
    - /login : connexion
    - /register : inscription
    - ...

    ## Fixtures

    Pour charger les données de test :

        php bin/console doctrine:fixtures:load --group=test

    ## Tests

    Pour lancer tous les tests :

        php bin/phpunit

    Pour lancer les tests liés au front-office :

        php bin/phpunit tests/Front

    Pour lancer les tests liés au back-office :

        php bin/phpunit tests/Back

    ## États des lieux

    Fonctionnalités qui devraient être implémentées :

    - ceci
    - cela
    - ...

    Bugs connus :

    - ceci
    - cela
    - ...

    ## Mentions légales

    Tout le code de ce repository est sous licence [GPL v3.0](https://www.gnu.org/licenses/gpl-3.0.html).

    ## Contact

    Pour toute demande d'information, contactez foo.bar@example.com

