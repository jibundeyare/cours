# Wamp Server

Allez sur le site [WampServer](https://www.wampserver.com/).

1. cliquez sur le lien « TÉLÉCHARGER »
2. cliquez sur le lien « WAMPSERVER 64 BITS (X64) 3.2.6 » (ou une version supérieure)
3. **ATTENTION** : si un formulaire vous demande des informations (email notamment), cliquez sur le lien (pas très visible) « passer au téléchargement direct »

Il est aussi possible de télécharger directement Wamp Server depuis Source Forge : [WampServer - Browse /WampServer 3 at SourceForge.net](https://sourceforge.net/projects/wampserver/files/WampServer%203/).
**Si vous passez par Source forge, évitez les versions beta.**

## Utilisation

Vous devez cliquer sur l'icône rose de Wamp Server à chaque démarrage (ou cocher l'option « lancer au démarrage »).
Si Apache et MySQL ont bien démarré, une icône avec un « W » vert devrait apparaître dans la barre windows, en bas à droite.

Si le bouton est orange c'est que certains services (Apache ou MySQL) n'ont pas pu démarrer.
Si le bouton est rouge c'est qu'aucun service (Apache et MySQL) n'a pu démarrer.
Reportez-vous à la section « Trouble shooting ».

## Tester le bon fonctionnement

Si tout est en place, le lien suivant devrait afficher la page d'accueil de Wamp Server :
[http://localhost/](http://localhost)

## Trouble shooting

Si Wamp démarre mais que Apache ou MySQL ne démarre pas, c'est problablement qu'il vous manque des DLL, vous pouvez téléchargez un fichier zip contenant toutes les DLL nécessaires à Wamp depuis la page suivante :

[http://wampserver.aviatechno.net/](https://wampserver.aviatechno.net/)

Allez tout en bas de la page et cliquez sur le lien :

- [All VC Redistribuable Packages (x86_x64) (32 & 64bits)](https://wampserver.aviatechno.net/files/vcpackages/all_vc_redist_x86_x64.zip) si votre PC fonctionne en 64 bit
- [All VC Redistribuable Packages (x86) (32bits)](https://wampserver.aviatechno.net/files/vcpackages/all_vc_redist_x86.zip) si votre PC fonctionne en 32 bit

C'est franchement long mais il faut installer toutes les DLL.
Oui, toutes.
Et à chaque fois que le programme d'installation vous demande de redémarrer, allez-y, redémarrez votre PC.
À la fin de l'installation, redémarrez une dernière fois votre PC.

*Note : il s'agit bien d'une page officile de Wamp.*

