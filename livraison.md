# Livraison

Pour en savoir plus sur le déploiement de projets Symfony, voir [deployer.md](deployer.md).

En général « la livraison » et « la mise en production (MEP) » veulent dire la même chose.

Mais s'il faut les différencier, on peut dire que :

    Livraison = communication avec le client + livrables

    MEP = installation du projet sur un serveur

## Communication avec le client

Elle doit impérativement se faire par écrit, quitte à la doubler d'un appel ou d'un rdv.
L'avantage d'une communication écrite :

- le client peut facilement retrouver le projet le jour où il en a besoin
- vous avez une preuve formelle de la date de livraison

Dans votre message :

- faites un bilan rapide du projet
- ajoutez quelques recommandations (pistes d'amélioration)
- mettez-y les formes (vousvoiement, formules de politesse)

Astuce : pour marquer le coup, vous pouvez livrer par voie postale en joignant une clé USB contenant le projet.
N'hésitez pas à rajouter un petit bonus (bonbons par exemple) pour laisser une bonne impression.

Si vous livrez par email, utilisez un service comme [WeTransfer](https://wetransfer.com/) si les livrables sont trop lourds.

Et au lieu d'envoyer les fchiers du projet, vous pouvez aussi juste transmettre un lien vers un repo git.

## Livrables

- un fichier `README.md` contenant la documentation du projet
- les wireframes
- un schéma de la BDD
- un export SQL de la BDD
- les fichiers sources du projet
- un fichier `LICENCE` contenant la licence du projet

Attention : pensez à supprimer tous les mots de passe d'accès à des services sensibles (BDD, serveur mail, etc).

Note spéciale pour MacOS : attention de ne pas se tromper en zippant un raccourci vers le dossier du projet au lieu de zipper le dossier du projet.

## Documentation du projet

Voir [doc-readme.md](doc-readme.md).

## Licence

Vous avez le choix entre des licences libres et des licences propriétaires.

Si vous optez pour une license libre, voyez [Choose an open source license](https://choosealicense.com/) pour faire un choix judicieux.

## Doc

- [Choose an open source license | Choose a License](https://choosealicense.com/)

