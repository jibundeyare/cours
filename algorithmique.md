# Algorithmique

## Variables

Les variables permettent de stocker des données en mémoire le temps d'effectuer des opérations dessus (calcul, transformation, recherche, remplacement, etc).

Déclaration : réservation d'espace mémoire pour une variable

Affectation : stockage d'une valeur dans une variable

Initialisation : déclaration et affectation en une seule opération

Attention, les variables ont des types de données :

- chaîne de caractères (texte)
- nombre entier
- nombre à virgule (flottant ou fixe)
- booléen (vrai / faux)
- tableau (plusieurs valeurs dans une seule variable)

## Structures de contrôle

Les structures de contrôle sont le squelette d'un algorithme.
Il y a deux types de structures de contrôle :

- les blocs conditionnels
- les boucles

Astuce : on peut représenter graphiquement un algorithme avec un diagramme de flux.

- [diagramme de flux at DuckDuckGo](https://duckduckgo.com/?q=diagramme+de+flux&t=ffab&iax=images&ia=images)
- [diagramme de flux - Recherche Google](https://www.google.fr/search?q=diagramme+de+flux&source=lnms&tbm=isch&sa=X&ved=0ahUKEwifmc6jxs3eAhVLJBoKHTZOD5sQ_AUIDigB&biw=1580&bih=780)

## Blocs conditionnelles

Les blocs conditionnelles permettent de faire des choix.

Ils permettent comparer des valeurs numériques ou valider la présence d'un mot dans un texte par exemple.
Ce qui est vérifié avec un bloc conditionnelle, c'est si la condition est « vraie » ou « fausse ».

### Anglais

    If ... Then
    Else If ... Then
    Else
    End

    Switch ...
    Case ...
    Case ...
    Other
    End

### Français

    Si ... Alors
    Sinon Si ... Alors
    Sinon
    Fin

    Selon ...
    Cas ...
    Cas ...
    Autrement
    Fin

## Boucles

Les boucles permettent de répêter une action.

Un bloc de code permet de répêter 100 fois, 200, fois, 1000 fois la même série d'instructions sans devoir copier-coller le code.

### Anglais

    While ...
    End

    Do
    While ...

    For each ... Do
    End

    For ... from ... to ... step ...
    Fin

### Français

    Tant que ...
    Fin

    Faire
    Tant que ...

    Pour chaque ... Faire
    Fin

    Pour ... de ... à ... par pas de ...
    Fin

## Fonctions

Les fonctions permettent de regrouper plusieurs instructions dans un bloc de code et de nommer de ce bloc code.
Le développeur peut alors « appeler » (utiliser) la fonction dans différents endroits de son programme pour exécuter des instructions.

Dans un premier temps, il faut « définir » une fonction.
Dès que cela est fait, il devient possible « d'appeler » la fonction.

Utiliser une « librarie » c'est surtout utiliser des fonctions qui ont été écrites par d'autres développeurs.

### Anglais

    Function foo()
      // ...
    End

    Function foo(parameter1)
      // ...

      Return value
    End

    Function foo(parameter1, parameter2, parameter3)
      // ...

      Return value
    End

### Français

    Fonction foo()
      // ...
    Fin

    Fonction foo(paramètre1)
      // ...

      Renvoyer valeur
    Fin

    Fonction foo(paramètre1, paramètre2, paramètre3)
      // ...

      Renvoyer valeur
    Fin

## Doc

- [Control flow - Wikipedia](https://en.wikipedia.org/wiki/Control_flow)
- [Structure de contrôle — Wikipédia](https://fr.wikipedia.org/wiki/Structure_de_contr%C3%B4le)
- [Mémo Pseudo-codes](http://www.isn.codelab.info/ressources/algorithmique/memo-pseudo-codes/)

