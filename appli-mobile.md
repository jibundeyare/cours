# Application mobile

Il existe deux possiblités pour créer une appli mobile :

1. écrire du code natif
2. créer une appli hybride

## Appli en code natif

Avec iOS, vous devrez coder votre appli en Objective C ou en Swift.

Avec Anrdoid, vous devrez coder votre appli en Java et éventuellement du code C++ pour optimiser certaines parties.

## Appli hybride

Le principe est différent.
Au lieu de coder votre appli avec un language natif, vous allez la coder en HTML, CSS et Javascript.

Mais pour que ça fonctionne correctement vous allez ajouter un serveur web et un client web à votre appli.

Le serveur ne servira qu'un seul site web : votre appli.
Et le client ne demandera qu'un seul site web : voitre appli.

Au final c'est comme si vous aviez livré un navigateur web qui ne peut aller que sur un seul site web.
Et ce site web sera installé sur le mobile.

Dans ce domaine, deux frameworks sortent du lot :

- [Cross-Platform Mobile App Development: Ionic Framework](https://ionicframework.com/)
- [NativeScript](https://nativescript.org/)

Ils ont l'avantage de fonctionner avec les framework Javascript que tout le monde utilise :

- Angular
- React
- Vue JS

Pour utiliser les fonctionnalités du téléphone (GPS, appareil photo, NFC, etc) il faudra utiliser une API spécialé livrée avec le framework.
Mais l'avantage est que vous n'avez même pas à apprendre de nouveaux languages pour faire tourner des applis sur iOS ou Android.

Elle est pas belle la vie ?

