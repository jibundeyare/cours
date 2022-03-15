# Algorithmique

Ce cours présente les notions de base de l'algorithmique.

Pour résumer, on peut dire que l'algorithmique est la façon dont il faut parler à un ordinateur pour qu'il réalise ce qu'on veut.

Les pages suivantes présentent l'algorithmique de façon plus détaillée :

- [Mémo Pseudo-codes](http://www.isn.codelab.info/ressources/algorithmique/memo-pseudo-codes/)
- [Structure de contrôle — Wikipédia](https://fr.wikipedia.org/wiki/Structure_de_contr%C3%B4le)

## Exemples

Le site [Rosetta Code](http://rosettacode.org/wiki/Rosetta_Code) propose des algorithme dans pleins de languages différents.
Son nom est un clin d'œil à la [Pierre de Rosette — Wikipédia](https://fr.wikipedia.org/wiki/Pierre_de_Rosette) qui a permis de déchiffrer les hiéroglyphes, une écriture de l'Égypte antique.

## Variables

Les variables permettent de stocker des données en mémoire le temps d'effectuer des opérations avec (affichage, calcul, transformation, recherche, remplacement, etc).

Déclaration : réservation d'espace mémoire pour une variable. Ou encore autrement dit, notification du nom de la variable que l'on va utiliser.

Affectation : stockage d'une valeur dans une variable.

Initialisation : première affectation d'une valeur à une variable.

Réaffectation : affectation d'une nouvelle valeur à une variable qui en avait déjà une.

## Constantes

Une constante est presque comme une variable.
La différence est qu'une fois qu'elle a été initialisée, on ne peut plus changer sa valeur.

Les constantes peuvent servir pour les calculs (nombre pi, nombre d'or, etc) par exemple.

En général, on les notes avec des majuscules seulement.

Par exemple (en langage PHP) :

    const PI = 3.14;
    const HALF_PI = 1.57;

## Types de donnée

Les variables ont des types de données.

Type de données simple :

- nombre entier
- nombre à virgule flottante
- booléen (vrai / faux)
- chaîne de caractères (texte)
- valeur nulle

À propos des nombres à virgule flottante :

- [Pourquoi 0.1 + 0.2 n'est pas égal à 0.3 en informatique ??? - YouTube](https://www.youtube.com/watch?v=CDYiwshriWw&ab_channel=Grafikart.fr)
- [Floating Point Visually Explained](https://fabiensanglard.net/floating_point_visually_explained/)

Autre types de données :

- tableau (plusieurs valeurs dans une seule variable)
- objet / classe

Note : c'est important de connaître les types car il y a des opération qu'on ne peut pas faire avec certains types.
Par exemple, il est possible de diviser un nombre mais diviser un booléen n'a pas de sens.

Voici les noms en anglais :

- integer
- float
- boolean (true / false)
- string (text)
- null value

- array
- object / class

### Conversion de types (type casting en anglais)

Dans la plupart des langages, il est possible de convertir un type de données vers un autre type.

Par exemple, le nombre `0` converti en booléen vaut `false`.
Alors que tout autre nombre converti en booléen vaut `true`.

De même, il est possible de convertir de int en float, arrondir et convertir des float en int, convertir des string en int ou float, etc...

En PHP, quand on affiche un int avec l'instruction `echo`, on convertit implicitement l'int en string.

## Les opérateurs

Les opérateurs permettent de réaliser des actions sur des variables et des données.

- `=` : affectation
- `==` : égalité
- `>` : plus grand que
- `<` : plus petit que
- `>=` : plus grand ou égal à
- `<=` : plus petit ou égal à
- `!=` : différent de
- `===` : identité (type de données identique et valeur identique)
- `!==` : non identité (type de données différent ou valeur différente)
- `&&` : « et » logique
- `||` : « ou » logique

## Structures de contrôle

Les structures de contrôle sont le squelette d'un algorithme.
Ce sont les structure de contrôles qui permettent de définir la façon dont l'information va circuler dans un programme.

Les types de structures de contrôle les plus importantes sont :

- les structure conditionnelles ou structures alternatives
- les boucles

## Diagramme de flux

On peut représenter graphiquement un algorithme avec un diagramme de flux.
Certaies personnes préfèrent découvrir les algorithme avec, donc ne vous privez pas.

- [Flowchart - Wikipedia](https://en.wikipedia.org/wiki/Flowchart)
- [Expliquer l'algorithme et le logigramme avec des exemples](https://www.edrawsoft.com/fr/explain-algorithm-flowchart.php)
- [Introduction aux algorigrammes](https://troumad.developpez.com/C/algorigrammes/)

## Structures conditionnelles ou structures alternatives (control flow en anglais)

Les structures conditionnelles permettent de faire des choix.

Ils permettent comparer des valeurs numériques ou valider la présence d'un mot dans un texte par exemple.
Ce qui est vérifié avec un bloc conditionnelle, c'est si la condition est « vraie » ou « fausse ».

### Anglais

    if conditionA then
      // do something
    end

    if conditionA then
      // do something
    else
      // do something
    end

    if conditionA then
      // do something
    else if conditionB then
      // do something
    else
      // do something
    end

    switch true
    case conditionA
      // do something
      break
    case conditionB
      // do something
      break
    other
      // do something
    end

### Français

    si conditionA alors
      // faire quelque chose
    fin

    si conditionA alors
      // faire quelque chose
    sinon
      // faire quelque chose
    fin

    si conditionA alors
      // faire quelque chose
    sinon si conditionB alors
      // faire quelque chose
    sinon
      // faire quelque chose
    fin

    selon vrai
    cas conditionA
      // faire quelque chose
      interrompre
    cas conditionB
      // faire quelque chose
      interrompre
    autrement
      // faire quelque chose
    fin

## Boucles (loops en anglais)

Les boucles permettent de répêter une action.

Un bloc de code permet de répêter 100 fois, 200, fois, 1000 fois la même série d'instructions sans devoir copier-coller le code.

### Anglais

Cette boucle vérifie une condition avant de faire une action :

    while conditionA
      // do something
    end

Cette boucle vérifie une condition après avoir fait une action :

    do
      // do something
    while conditionA

Cette boucle permet de parcourir tous les éléments d'un tableau :

    for each item in list do
      // do something
    end

Cette boucle répête une action un nombre de fois déterminé :

    for index from start to end step x
      // do something
    fin

### Français

Cette boucle vérifie une condition avant de faire une action :

    tant que conditionA
      // faire quelque chose
    fin

Cette boucle vérifie une condition après avoir fait une action :

    faire
      // faire quelque chose
    tant que conditionA

Cette boucle permet de parcourir tous les éléments d'un tableau :

    pour chaque item dans liste faire
      // faire quelque chose
    fin

Cette boucle répête une action un nombre de fois déterminé :

    pour index de début à fin par pas de x
      // faire quelque chose
    fin

## Sauts, branchements inconditionnels (jumps en anglais)

Les instructions `continue` et `break` permettent de faire des sauts dans le code.
On appelle aussi cela un « branchement inconditionnel ».

### Continuer (continue en anglais)

Quand l'instruction `continue` est utilisée, la boucle recommence une itération en ignorant le code qui pourrait suivre dans la boucle.

Exemple d'utilisation de `continue` dans une boucle :

    i = 0

    while i < 10
      i++

      // demande d'itération en ignorant la suite
      continue

      // la ligne suivante ne sera jamais exécutée
      echo "Hello !"
    end

### Interrompre (break en anglais)

Quand l'instruction `break` est utilisée, la boucle ou le switch s'arrête en ignorant le code qui pourrait suivre.

Exemple d'utilisation de `break` dans une boucle :

    i = 0

    while i < 10
      i++

      // la ligne suivante n'est exécutée qu'une seule fois
      echo "Hello !"

      // car il y a demande d'interruption juste après
      break
    end

L'instruction `break` est obligatoire dans un `switch` (à moins de savoir ce qu'on fait) :

    switch true
    case conditionA
      // do something
      break
    case conditionB
      // do something
      break
    other
      // do something
      break
    end

## Fonctions (functions en anglais)

Les fonctions permettent de regrouper plusieurs instructions dans un bloc de code et de nommer ce bloc code.
Le développeur peut alors « appeler » (utiliser) la fonction dans différents endroits de son programme pour exécuter des instructions.

Dans un premier temps, il faut « définir » une fonction.
Dès que cela est fait, il devient possible « d'appeler » la fonction.

Utiliser une « librairie » c'est surtout utiliser des fonctions qui ont été écrites par d'autres développeurs.

### Renvoyer (return en anglais)

L'instruction `return` permet à une fonction de renvoyer une ou plusieurs valeurs (selon les possibilités du langage).
Quand cette instruction est utilisés, le programme sort de la fonction et le code qui pourrait suivre dans le fonction est ignoré.
Si l'instruction est utilisée sans valeur de retour, le programme sort de la fonction sans renvoyer de valeur (et le code qui pourrait suivre dans la fonction est ignoré).

### Anglais

Définition d'une fonction qui ne prend pas de paramètre et ne renvoit rien :

    function foo()
      // do something
    end

Appel de la fonction :

    foo()

Définition d'une fonction qui prend un paramètre et renvoit une valeur :

    function foo(parameter1)
      // do something

      return value
    end

Appel de la fonction et affectation de la valeur de retour dans une variable :

    result = foo(param1)

Définition d'une fonction qui prend trois paramètre et renvoit une valeur :

    function foo(parameter1, parameter2, parameter3)
      // do something

      return value
    end

Appel de la fonction et affectation de la valeur de retour dans une variable :

    result = foo(param1, param2, param3)

### Français

Définition d'une fonction qui ne prend pas de paramètre et ne renvoit rien :

    fonction foo()
      // faire quelque chose
    fin

Appel de la fonction :

    resultat = foo()

Définition d'une fonction qui prend un paramètre et renvoit une valeur :

    fonction foo(paramètre1)
      // faire quelque chose

      renvoyer valeur
    fin

Appel de la fonction et affectation de la valeur de retour dans une variable :

    resultat = foo(param1)

Définition d'une fonction qui prend trois paramètre et renvoit une valeur :

    fonction foo(paramètre1, paramètre2, paramètre3)
      // faire quelque chose

      renvoyer valeur
    fin

Appel de la fonction et affectation de la valeur de retour dans une variable :

    resultat = foo(param1, param2, param3)

## Doc

- [Mémo Pseudo-codes](http://www.isn.codelab.info/ressources/algorithmique/memo-pseudo-codes/)
- [Flowchart - Wikipedia](https://en.wikipedia.org/wiki/Flowchart)
- [Expliquer l'algorithme et le logigramme avec des exemples](https://www.edrawsoft.com/fr/explain-algorithm-flowchart.php)
- [Introduction aux algorigrammes](https://troumad.developpez.com/C/algorigrammes/)
- [Control flow - Wikipedia](https://en.wikipedia.org/wiki/Control_flow)
- [Structure de contrôle — Wikipédia](https://fr.wikipedia.org/wiki/Structure_de_contr%C3%B4le)

