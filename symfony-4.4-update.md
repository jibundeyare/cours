# Symfony 4.4 - Update

Avec Symfony, la mise à jour des packages peut nécessiter quelques petites opérations en plus d'un simple `composer update`.

Certains packages utilisent des `recipes`.
Ce sont tout simplement des scripts exécutés après l'installation d'un package.

Mais l'exécution de ces scripts ne se fait pas automatiquement après la mise à jour d'un package, il faut en demander explicitement l'exécution avec la commande `composer recipes:install vendor/package --force -v`.

```bash
# Mise à jour de tous les packages.
composer update

# Liste des recipes.
composer recipes

# Si des recipes sont marqués avec un "update available" on peut lancer
# l'exécution des recipes.
# Example d'exécution du recipe du package foo/bar.
composer recipes:install foo/bar --force -v

# Pour visualiser les actions qui ont été réalisé, on peut se servir de git.
git diff

# Si tout est en ordre, on peut commiter.
git add .
git commit

# Sinon on peut revenir en arrière.
git checkout composer.json composer.lock symfony.lock
composer install
# Il faut aussi veiller à remettre en l'état les fichiers modifiés par les recipes...
git checkout fichier-a-reseter
# et éventuellement supprimer des fichiers créés par des recipes.
rm nouveau-fichier
```

