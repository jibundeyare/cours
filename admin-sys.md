# Administration système

## Le compte Root

Le compte root permet de modifier le système.
Par exemple : installer un nouveau paquet, supprimer un paquet, mettre à jour un paquet, modifier la configuration de paquets installés, etc.

Il est *très fortement* recommandé d'utiliser la commande `sudo` au lieu d'utiliser la commande `su`.

Voici comment fonctionne `sudo` : [xkcd: Sandwich](https://www.xkcd.com/149/).

Par exemple, pour mettre à jour la liste des paquets :

    sudo apt update

Si nécessaire, pour devenir root :

    su

