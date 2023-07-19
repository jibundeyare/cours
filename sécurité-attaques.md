# Sécurité - Les attaques

Cette fiche décrit quelques attaques types qui arrivent dans le développement web.
Mais attention, cette liste n'est pas exhaustive.
Si vous voulez voir une liste plus complète, référez-vous à [Attacks | OWASP Foundation](https://owasp.org/www-community/attacks/).

## Le maillon faible de la sécurité informatique

Il est impossible de lutter contre la stupidité.

Il existe même une expression en anglais pour parler de ce problème : PEBKAC (Problem Exists Between Chair And Keyboard).

[User error - Wikipedia](https://en.wikipedia.org/wiki/User_error)

Exemple : en 2015, trois étudiants de l'Université de Saarland en Allemagne ont découvert 40 000 instances de MongoDB connectées à internet et accessibles en lecture et écriture sans authentification.
Quarante mille instances dont **une société française de télécom**.
Bravo...

[Thousands of MongoDB Databases Found Exposed on the Internet - SecurityWeek](https://www.securityweek.com/thousands-mongodb-databases-found-exposed-internet/)

## Ingénierie sociale

La sécurité informatique commence par l'éducation des utilisateurs.

Le principe de cette méthode est l'exploitation de la faille de sécurité que représente les utilisateurs.

[Ingénierie sociale (sécurité de l'information) — Wikipédia](https://fr.wikipedia.org/wiki/Ing%C3%A9nierie_sociale_(s%C3%A9curit%C3%A9_de_l%27information))

Le pirate informatique Kevin Mitnick a écrit un livre sur le sujet, basé sur des histoires vraies : [L'art de la supercherie - Mitnick, Kevin D., Simon, William L., Wozniak, Steve](https://www.amazon.fr/Lart-supercherie-r%C3%A9v%C3%A9lations-c%C3%A9l%C3%A8bre-plan%C3%A8te/dp/2744015709/ref=sr_1_1)

[Kevin Mitnick — Wikipédia](https://fr.wikipedia.org/wiki/Kevin_Mitnick)

## Typosquatting

Le principe de cette méthode d'attaque est d'exploiter les fautes de frappe ou des raccourcis de lecture des utilisateurs.

Exemple : vous pensiez installer la package `node-sass` mais vous avez installé le package `node-zass`.
Ce dernier fournira les mêmes fonctionnalités mais il en profitera aussi pour siffoner toutes les données accessibles.

[Typosquatting - Wikipedia](https://en.wikipedia.org/wiki/Typosquatting)

Exemple : [Malicious Package in crossenv | CVE-2017-16074 | Snyk](https://security.snyk.io/vuln/npm:crossenv:20170802)

Une thèse sur le sujet : [incolumitas.com – Typosquatting programming language package managers](https://incolumitas.com/2016/06/08/typosquatting-package-managers/)

## Injection de code HTML et JS

Si vous avez un formulaire qui demande à un utilisateur de choisir son pseudo, a priori c'est pour afficher ce pseudo.
Imaginez qu'un utilisateur malveillant glisse du code HTML dans le champ `input`.
Quand une page voudra afficher le pseudo, si le développeur n'a pas pris certaines précautions, le code HTML sera interprêté au lieu d'être juste affiché.

Résultat : l'utilisateur malveillant pourra injecter du code HTML contenant du code JS et tous les utilisateurs (futures victimes) qui verront le pseudo malveillant exécuteront le code JS.

## Injection de code SQL

Cette attaque repose sur le même principe que la précédente.
Un utilisateur malveillant glisse du code SQL dans un champ `input` d'un formulaire.
Quand une page voudra effectuer une requête SQL impliquant le pseudo, la BDD exécutera le code SQL ajouté par l'utilisateur malveillant.

Résultat : l'utilisateur malveillant pourra exécuter des requêtes SQL pour modifier son propre compte ou accéder à des informations sensibles.

[SQL Injection | OWASP Foundation](https://owasp.org/www-community/attacks/SQL_Injection)

## Attaque XSS

Le principe de cette méthode est de faire télécharger par un navigateur un fichier JS qui contient du code malveillant.

[Cross Site Scripting (XSS) | OWASP Foundation](https://owasp.org/www-community/attacks/xss/)

## Attaque CSRF

Le principe de cette méthode est d'exploiter le fait que beaucup d'utilisateurs sont déjà connectés et authentifier à des services d'usage courant (réseaux sociaux, banque en ligne, etc).
Un pirate qui parviendrait à amener une victime à cliquer sur un lien pourrait cacher dans ce lien une action en rapport avec le service auquel la victime est déjà connectée.

[Cross Site Request Forgery (CSRF) | OWASP Foundation](https://owasp.org/www-community/attacks/csrf)

