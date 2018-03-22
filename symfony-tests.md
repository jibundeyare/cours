# Symfony tests

Il y a deux types de tests :

1. les tests unitaires
2. les tests fonctionnels

## Tests unitaires

Ces tests permettent de vérifier chaque les fonctions (ou méthode de classe) une à une et d'être sûr que leur comportement individuel est correct.

Pour faire ces tests, on utilise des paramètres connus lors de l'appel de la fonction et on compare ce qu'elle renvoie avec le résultat attendu.

## Tests fonctionnels

Même si chaque fonction de votre application se comporte correctement, votre application peut comporter des bugs. (Il suffit de mal combiner les fonctions entre elles.)

Les tests fonctionnels permettent de vérifier que l'application se comporte correctement lorsqu'elle utilisée par une vraie personne.

Pour faire ces tests, on pilote un client HTTP (une librairie PHP ou un vrai navigateur web) et on vérifie que chaque action demandée entraîne le résultat souhaité (affichage d'une chaîne de caractères ou d'une balise HTML par exemple).

## Doc

- [Testing (Symfony 3.4 Docs)](https://symfony.com/doc/3.4/testing.html)
- [How to Unit Test your Forms (Symfony 3.4 Docs)](https://symfony.com/doc/3.4/form/unit_testing.html)
- [How to Test Code that Interacts with the Database (Symfony 3.4 Docs)](https://symfony.com/doc/3.4/testing/database.html)
- [How to Test Doctrine Repositories (Symfony 3.4 Docs)](https://symfony.com/doc/3.4/testing/doctrine.html)
