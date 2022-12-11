# Git en solo, mode basic

Ce gitflow est adapté pour travailler seul sur un projet simple.

Simple ça veut dire, par exemple :

- qu'il y a une version du projet
- que dès qu'on rajoute du code, il est prêt à être publié
- qu'on a pas besoin d'avoir un historique nickel
- etc

## Création du repo en local

```bash
# créez le dossier du projet
mkdir [dossier-du-projet]
# allez dans le dossier du projet
cd [dossier-du-projet]
# ouvrez le dossier avec vscode
code .
# initialisez le repo git
git init
# todo: créez un fichier `README.md` à la racine du projet
# ajoutez le fichier `README.md` dans la zone de staging
git add README.md
# créez un premier commit
git commit
# todo: rédigez votre message de commit
```

## Création du repo sur Github

S'il y a lieu, créez un compte sur le site.
Sur le site, créez un nouveau repo (attention à donner le même nom que celui du dossier).

Attention : sur le site, sélectionnez le protocole SSH.
Remarquez que le chemin d'accès du repo est de la forme `git@github.com:[login]/[nom-du-repo].git`.
Exemple : avec le login `foo` et le repo `bar` ça donne `git@github.com:foo/bar.git`.

Dans le terminal, adaptez [login] et [nom-du-repo] et tapez :

```bash
git remote add origin git@github.com:[login]/[nom-du-repo].git
git branch -M main
git push -u origin main
```

## Enregistrer son travail

Créez, modifier ou supprimez des fichiers sources.

```bash
# optionnel: vérifier l'état du repo
git status
# optionnel: vérifiez le code qui a changé
git diff

# ajoutez les fichiers dans la zone de staging
git add [nom-du-fichier]
# todo: répétez `git add` autant de fois que nécessaire

# optionnel: vérifier l'état du repo
git status
# optionnel: vérifiez le code qui sera commité
git diff --staged

# créez un commit
git commit
# todo: rédigez votre message de commit
# envoyez votre commit sur github
git push
```

## Problème : j'ai ajouté un fichier ou un dossier par erreur

La commande suivante permet d'annuler un `git add [nom-du-fichier]` (fichier ou dossier) :

```bash
git restore --staged [nom-du-fichier]
```

Cette commande ne modifie pas le contenu du fichier.

## Problème : je veux annuler tous les ajouts

La commande suivante permet d'annuler tous les `git add [nom-du-fichier]` (fichier ou dossier) en une seule fois :

```bash
git restore --staged .
```

Cette commande ne modifie pas le contenu du fichier.

## Problème : j'ai modifié un fichier mais je voudrais le remettre dans l'état du dernier commit

**Attention : cette commande modifie le contenu du fichier.
Vous pouvez perdre du code si vous l'utilisez bêtement.**

Attention : cette technique n'est valable que si vous n'avez pas encore fait de `git commit` depuis la modification du fichier.

```bash
git checkout [nom-du-fichier]
```

Si vous aviez ajouté le fichier avec `git add [nom-du-fichier]`, voyez « Problème : j'ai ajouté un fichier ou un dossier par erreur ».

## Problème : j'ai créé un commit mais j'ai oublié d'ajouter des fichiers ou des dossiers

Attention : cette technique n'est valable que si vous n'avez pas encore fait de `git push` depuis le dernier commit.
Sinon il faudra se résoudre à créer un commit.
Mais rassurez-vous, ce n'est pas grave du tout.

```bash
# optionnel: vérifier l'état du repo
git status
# optionnel: vérifiez le code qui a changé
git diff

# ajoutez les fichiers dans la zone de staging
git add [nom-du-fichier]
# todo: répétez `git add` autant de fois que nécessaire

# optionnel: vérifier l'état du repo
git status
# optionnel: vérifiez le code qui sera commité
git diff --staged

# modifiez votre dernier commit
git commit --amend
```

## Problème : j'ai créé un commit mais je voudrais l'annuler

Attention : cette technique est valable que vous ayez déjà utilisé `git push` ou non.

Vous pouvez créer un nouveau commit qui supprime tout ce que vous avez ajouté avec :

```bash
git revert HEAD
```

Notez que le code que vous avez ajouté dans le précédent commit reste présent dans l'historique.

