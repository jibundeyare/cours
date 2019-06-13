# PHP language poo

@todo exo avec un jeu de carte

## Arborescence d'un projet

Voir la section « Arborescence d'un projet » dans [php-application.md](php-application.md).

## Autoloading

Voir la section « Autoloading » dans [composer.md](composer.md).

## Programmation orientée objet (POO)

Une classe est la définition d'un objet.

Un objet est une instance de classe.
Ou autrement dit, un objet est la matérialisation d'une définition abstraite.

Analogie : on trouve la définition du mot voiture dans le dictionnaire, c'est l'équivalent d'une classe.
On trouve de vraies voitures dans la rue, ce sont l'équivalent d'objets (des instances de la définition).

En POO :

- une variable se nomme « attribut » ou « membre »
- une fonction se nomme « méthode »

### La portée des variables (le scope, en anglais)

Il y a trois scopes possibles : `public`, `protected` ou `private`.

Ce qui est `public` est visible depuis l'extérieur.
Ce qui est `private` n'est visible que de l'intérieur.
Ce qui est `protected` est visible de l'intérieur et aussi pour les classes enfants.

Dans une classe, un attribut (une variable) doit toujours être `private`.
Il y a une exception : si la variable doit être accessible depuis une classe enfant, il faut qu'elle soit `protected`.

Les méthodes peuvent être `public`, `protected` ou `private` selon les cas d'usage.
En cas de doute, partez du principe qu'elles sont `public`.

### Les `getters` et les `setters`

Les méthodes (fonctions) qui permettent de lire un attribut s'appelent des `getters`.

Les méthodes qui permettent de changer la valeur d'un attribut s'appelent des `setters`.

## Syntaxe

### Création d'une classe

Attention : le nom des classes s'écrit toujours avec une majuscule.

Créons une classe `Vehicule` qui possède deux attributs : la vitesse et le nombre de passagers.
La classe possède également des getters et des setters pour modifier les attributs privés.

    class Vehicule
    {
      private $vitesse;
      private $passagers;

      public function getVitesse()
      {
        return $this->vitesse;
      }

      public function setVitesse($vitesse)
      {
        $this->vitesse = $vitesse;

        return $this;
      }

      public function getPassagers()
      {
        return $this->passagers;
      }

      public function setPassagers($passagers)
      {
        $this->passagers = $passagers;

        return $this;
      }
    }

### Instanciation d'une classe

Créons deux instances de la classe `Vehicule` : les objets `$voiture` et `$bus`.
Ensuite affectons leur une vitesse et un nombre de passagers puis affichons ces données.

    $voiture = new Vehicule();
    $voiture->setPassagers(2);
    $voiture->setVitesse(120);
    echo $voiture->getPassagers()."\n";
    echo $voiture->getVitesse()."\n";

    $bus = new Vehicule();
    $bus
      ->setPassagers(12)
      ->setVitesse(50);
    echo $bus->getPassagers()."\n";
    echo $bus->getVitesse()."\n";

## Doc

- [PHP: Hypertext Preprocessor](https://secure.php.net/)

- [PHP-FIG — PHP Framework Interop Group - PHP-FIG](https://www.php-fig.org/)
- [PSR-1: Basic Coding Standard - PHP-FIG](https://www.php-fig.org/psr/psr-1/)
- [PSR-2: Coding Style Guide - PHP-FIG](https://www.php-fig.org/psr/psr-2/)
- [PSR-0: Autoloading Standard - PHP-FIG](https://www.php-fig.org/psr/psr-0/)
- [PSR-4: Autoloader - PHP-FIG](https://www.php-fig.org/psr/psr-4/)

- [PHP: Classes and Objects - Manual](https://www.php.net/manual/en/language.oop5.php)

