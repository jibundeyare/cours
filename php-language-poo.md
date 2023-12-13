# PHP language poo

PHP est un langage informatique conçu pour créer des applications web.

## Arborescence d'un projet

Voir la section « Arborescence d'un projet » dans [php-application.md](php-application.md).

## Autoloading

Voir la section « Autoloading » dans [composer.md](composer.md).

## Programmation orientée objet (POO)

### Classes et objets

Les notions de classe et d'objet sont primordiales en POO.

Une classe est la déclaration des attributs et des méthodes d'un objet.
Un objet est une instance d'une classe.

Autrement dit, une classe est comme la définition d'un objet.
Et un objet est la matérialisation de la définition abstraite qu'est la classe.

Analogie : on trouve la définition du mot voiture dans le dictionnaire, c'est l'équivalent d'une classe.
On trouve de vraies voitures dans la rue, ce sont l'équivalent d'objets (des instances de la définition).

### Attributs et méthodes

En POO une variable qui est rattachée à une classe se nomme « attribut », « propriété » ou « membre », et une fonction qui est rattachée à une classe se nomme « méthode ».

## Constructeur et destructeur

Quand on créé une nouvelle instance d'une classe, il peut s'avérer nécessaire de transmettre des paramètres. La fonction qui est responsable de la récupération de ces paramètres et de leur utilisation lors de la création de l'instance, s'appelle le constructeur.

Par exemple : si vous créer un nouvel objet de type Voiture, vous aurez peut-être besoin d'indiquer la marque, le modèle et la couleur du véhicule. Le constucteur peut récupérer les valeurs transmises et les affecter aux attributs de l'instance en utilisant les setters.

Quand on détruit explicitement un objet ou qu'un objet est déréferencé par une application et détruit par le garbage collector, une méthode spéciale est appelée, c'est le destructeur. Le destructeur est responsable de la procédure de nettoyage / rangement avant destruction définitive de l'objet. Par exemple : quand un objet de type ConnexionBaseDeDonnées est détruit, le destrcuteur s'assure que la connexion à la BDD est bien fermée, pour éviter qu'on ne puisse pas créer de nouvelle connexion.

### Héritage

L'héritage permet réduire la quantité de code qu'il faut écrire dans plusieurs classes si elles possèdent des fonctionnalités semblables.

On dit qu'une classe hérite d'une autre classe. Par exemple : la classe Voiture hérite de la classe Véhicule.

La classe qui hérite s'appelle la classe enfant. Par exemple : la classe Voiture est une classe enfant de la classe Véhicule. Notez qu'on dit « une classe enfant » et pas « la classe enfant » car il peut y en avoir plusieurs.

La classe dont la classe enfant hérite s'appelle la classe parent ou la classe mère. Par exemple : la classe Véhicule est la classe parent (ou classe mère) de la classe Voiture. Notez qu'on dit « la classe parent » et pas « une classe parent » car il ne peut y en avoir qu'une seule (sauf so language utilisé permet l'héritage multiple).

Quand une classe hérite d'une autre classe, elle récupère tous les attributs (variables) et méthodes (fonctions) de la classe parent. C'est comme si on faisait un copier-coller du code de la classe parent vers la classe enfant. Sauf qu'il faut tout de même prendre en compte la portée des variables et des fonctions.

### La portée des variables et des fonctions (scope, en anglais)

Il y a trois scopes possibles : `public`, `protected` ou `private`.

Ce qui est `public` est visible depuis l'extérieur.
Ce qui est `private` n'est visible que de l'intérieur.
Ce qui est `protected` est visible de l'intérieur et aussi par les classes enfants (c-à-d les classes qui héritent de la classe courante).

Dans une classe, un attribut (une variable) doit toujours être `private`.
Il y a une exception : si la variable doit être accessible depuis une classe enfant, il faut qu'elle soit `protected`.

Les méthodes peuvent être `public`, `protected` ou `private` selon les cas d'usage.
En cas de doute, partez du principe qu'elles sont `public`.

### Les getters et les setters

Les méthodes (fonctions) qui permettent de lire un attribut s'appelent des getters.

Les méthodes qui permettent de changer la valeur d'un attribut s'appelent des setters.

### Classes abstraites, classes concrètes et classes finales

Une classe abstraite est une classe dont on interdit la possibilité de créer une instance. Par contre l'héritage est autorisé.

Signaler qu'une classe est abstraite permet d'éviter qu'un développeur créé un objet à partir d'une classe qui n'a pas de sens. Par exemple : créer un objet à partir de la classe Voiture a du sens car il existe des voiture. Mais créer un objet à partir de la classe Véhicule n'a pas de sens car il n'existe pas d'objet de type Véhicule qui ne soit pas en même temps une voiture, une moto, un vélo, etc. Alors autant créer un objet à partir de la classe Voiture, Moto ou Vélo.

Une classe concrète est une classe qui hérite d'une classe abstraite et qui n'est pas elle-même une classe abstraite.

Une classe finale est une classe dont on interdit la possibilité d'en hériter. Par contre la création d'instance est autorisée.

Signaler qu'une classe est finale permet d'éviter qu'un développeur réutilise une classe.

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

## PHP 8

- [What's new in PHP 8 - stitcher.io](https://stitcher.io/blog/new-in-php-8)

### Généralités

- [PHP: Hypertext Preprocessor](http://php.net/)

### Code style

- [PSR-1: Basic Coding Standard - PHP-FIG](https://www.php-fig.org/psr/psr-1/)
- <del>[PSR-2: Coding Style Guide - PHP-FIG](https://www.php-fig.org/psr/psr-2/)</del>
- [PSR-12: Extended Coding Style - PHP-FIG](https://www.php-fig.org/psr/psr-12/)
- [PSR-0: Autoloading Standard - PHP-FIG](https://www.php-fig.org/psr/psr-0/)
- [PSR-4: Autoloader - PHP-FIG](https://www.php-fig.org/psr/psr-4/)

### POO

- [PHP: Classes and Objects - Manual](https://www.php.net/manual/en/language.oop5.php)

### WTF

- [PHP Sadness](http://phpsadness.com/)

### Best practices

- [PHP-FIG — PHP Framework Interop Group - PHP-FIG](https://www.php-fig.org/)
- [PHP: The Right Way](http://www.phptherightway.com/)
- [PHP Best Practices: a short, practical guide for common and confusing PHP tasks](https://phpbestpractices.org/)

