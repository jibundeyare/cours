# Symfony tests

Il y a deux types de tests :

1. les tests unitaires
2. les tests fonctionnels

## Tests unitaires

Ces tests permettent de vérifier chaque les fonctions (ou méthode de classe) une à une et d'être sûr que leur comportement individuel est correct.

Pour faire ces tests, on utilise des paramètres connus lors de l'appel de la fonction et on compare ce qu'elle renvoie avec le résultat attendu.

## Tests fonctionnels

Même si chaque fonction de votre application se comporte correctement, votre application peut comporter des bugs. (Il suffit de mal combiner les fonctions entre elles.)

Les tests fonctionnels permettent de vérifier que l'application se comporte correctement lorsqu'elle est utilisée par une vraie personne.

Pour faire ces tests, on pilote un client HTTP (une librairie PHP ou un vrai navigateur web) et on vérifie que chaque action demandée entraîne le résultat souhaité (affichage d'une chaîne de caractères ou d'une balise HTML par exemple).

## Configuration

Une des premières choses à faire est de créer une BDD pour les tests.

Ouvrez le fichier `.env` ou `.env.local` contenant les code d'accès à votre BDD.
Copier la ligne qui commence par `DATABASE_URL=`.

Créez le fichier `.env.test.local` et collez la ligne que vous venez de copier (celle qui commence par `DATABASE_URL=`).
Ajoutez le mot clé `_test` à la fin du nom de la BDD.

Si la ligne que vous avez copié était :

    DATABASE_URL=mysql://root:123@127.0.0.1:3306/src_symfony_3_4

maintenant vous devriez avoir :

    DATABASE_URL=mysql://root:123@127.0.0.1:3306/src_symfony_3_4_test

## Script de lancement des tests

Ce script reconstruit la BDD et charge les fixtures avant de lancer les tests.

### Linux et MacOS

Créer un fichier nommé `tests.sh` dans le dossier `bin` :

    #!/bin/bash
    # bin/tests.sh

    php bin/console doctrine:fixture:load --env=test --purge-with-truncate --no-interaction
    bin/phpunit

Pour démarrer le script, taper la commande suivante :

    bin/tests.sh

### Windows

Créer un fichier nommé `tests.bat` dans le dossier `bin` :

    @echo off
    REM bin/tests.bat

    php bin/console doctrine:fixture:load --env=test --purge-with-truncate --no-interaction
    bin/phpunit

Pour démarrer le script, taper la commande suivante :

    bin/tests

## Snippets utiles

### WebTestCase

    // affiche le contenu HTML de la page en cours
    dump($client->getResponse()->getContent());

    // affiche le path de la page en cours (si l'URL entière est `http://localhost/my/path`, ceci affiche `/my/path`)
    dump($client->getRequest()->getPathInfo());

### PantherTestCase

    // sauvegarde une image en utilisant le nom de la classe et le nom de la fonction
    // exemple de nom de fichier : `App_Tests_StudentControllerTest-testEditStudent.png`
    $client->takeScreenshot(str_replace('\\', '_', __CLASS__).'-'.__FUNCTION__.'.png');

    // valider une alerte javascript
    $client->switchTo()->alert()->accept();

    // refuser une alerte javascript
    $client->switchTo()->alert()->dismiss();

    // récupérer le texte d'une alerte javascript
    $text = $client->switchTo()->alert()->getText();

    // @warning le sélecteur css `:contains()` ne fonctionne pas avec symfony/panther
    $link = $crawler
        ->filter('td:contains("lorem.ipsum@example.com")')
        ->parents()
        ->selectLink('edit')
        ->link()
    ;

    // alternative au sélecteur css `:contains()` qui fonctionne pas avec symfony/panther
    $link = $crawler
        ->filter('td')
        ->reduce(function (Crawler $node, $i) {
            if ($node->text() == 'lorem.ipsum@example.com') {
                return true;
            }

            return false;
        })
        ->parents()
        ->selectLink('edit')
        ->link()
    ;

## Doc

- [Testing (Symfony 3.4 Docs)](https://symfony.com/doc/3.4/testing.html)
- [How to Unit Test your Forms (Symfony 3.4 Docs)](https://symfony.com/doc/3.4/form/unit_testing.html)
- [How to Test Code that Interacts with the Database (Symfony 3.4 Docs)](https://symfony.com/doc/3.4/testing/database.html)
- [How to Test Doctrine Repositories (Symfony 3.4 Docs)](https://symfony.com/doc/3.4/testing/doctrine.html)

### BrowserKit, CssSelector, DomCrawler et Request

- [The BrowserKit Component (Symfony 3.4 Docs)](https://symfony.com/doc/3.4/components/browser_kit.html)
- [The CssSelector Component (Symfony 3.4 Docs)](https://symfony.com/doc/3.4/components/css_selector.html)
- [The DomCrawler Component (Symfony 3.4 Docs)](https://symfony.com/doc/3.4/components/dom_crawler.html)
- [Symfony\Component\HttpFoundation\Request | Symfony API](https://api.symfony.com/3.4/Symfony/Component/HttpFoundation/Request.html)

### symfony/panther et facebook/php-webdriver

- [Introducing Symfony Panther: a Browser Testing and Web Scrapping Library for PHP (Symfony Blog)](https://symfony.com/blog/introducing-symfony-panther-a-browser-testing-and-web-scrapping-library-for-php)
- [symfony/panther: A browser testing and web crawling library for PHP and Symfony](https://github.com/symfony/panther)
- [Symfony syntax not working ? · Issue #11 · symfony/panther](https://github.com/symfony/panther/issues/11)
- [facebook/php-webdriver: A php client for webdriver.](https://github.com/facebook/php-webdriver)
- [Namespaces | php-webdriver API](https://facebook.github.io/php-webdriver/latest/)
- [Facebook\WebDriver\WebDriverAlert | php-webdriver API](https://facebook.github.io/php-webdriver/latest/Facebook/WebDriver/WebDriverAlert.html)
