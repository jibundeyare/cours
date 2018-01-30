# Remg Generator Bundle

Remg Generator Bundle est un plugin Symfony qui permet de générer des entités et des associations entre entités.

## Installation

Dans un terminal, si vous n'êtes pas déjà dans la dossier racine de votre projet, taper :

    cd [dossier du projet web]

Pour installer le bundle :

    composer require --dev remg/generator-bundle dev-master

Pour activer le bundle, ouvrir le fichier `app/AppKernel.php` et modifier le bloc :

    if (in_array($this->getEnvironment(), ['dev', 'test'], true)) {
        $bundles[] = new Symfony\Bundle\DebugBundle\DebugBundle();
        $bundles[] = new Symfony\Bundle\WebProfilerBundle\WebProfilerBundle();
        $bundles[] = new Sensio\Bundle\DistributionBundle\SensioDistributionBundle();

        if ('dev' === $this->getEnvironment()) {
            $bundles[] = new Sensio\Bundle\GeneratorBundle\SensioGeneratorBundle();
            $bundles[] = new Symfony\Bundle\WebServerBundle\WebServerBundle();
        }
    }

afin d'obtenir :

    if (in_array($this->getEnvironment(), ['dev', 'test'], true)) {
        $bundles[] = new Symfony\Bundle\DebugBundle\DebugBundle();
        $bundles[] = new Symfony\Bundle\WebProfilerBundle\WebProfilerBundle();
        $bundles[] = new Sensio\Bundle\DistributionBundle\SensioDistributionBundle();

        if ('dev' === $this->getEnvironment()) {
            $bundles[] = new Sensio\Bundle\GeneratorBundle\SensioGeneratorBundle();
            $bundles[] = new Symfony\Bundle\WebServerBundle\WebServerBundle();
            $bundles[] = new Remg\GeneratorBundle\RemgGeneratorBundle();
        }
    }

Pour configurer le bundle, ajouter à la fin du fichier `app/config/config_dev.yml` :

    remg_generator:
        entity:
            # available configuration formats are: 'annotation', 'yaml', 'xml' and 'php'.
            configuration_format: annotation

## Utilisation

Générer une nouvelle entité :

    php app/console remg:generate:entity

Modifier une entité existante :

    php app/console remg:regenerate:entity

Attention :

- un backup de l'ancienne antité est créé avant la regénération d'une nouvelle entité. Exemple : `Task.php~2018-01-18_14-21-39`
- la commande `remg:regenerate:entity` ne régénère pas le code ajouté à la main dans une entité, il faut l'ajouter soi-même avec copier-coller depuis le fichier sauvegardé

## Notions

Pour en savoir plus sur la question de la casse du nom des variables (majuscules, minuscules), voir [code-style.md](code-style.md).

Imaginons que l'on veut obtenir deux entités : `Foo` et `Bar`. Nous commençons par créer l'entité `Foo`.

`Field name` désigne le nom de la variable qui sera utilisée dans l'entité `Foo`.

`Nullable` veut dire « optionnel » :

 - un champ nullable est optionnel
 - un champ non nullable est obligatoire

`Unique` porte sur la valeur du champ. Par exemple, si un champ `email` est unique, il ne pourra y avoir qu'une seule adresse `lorem.ipsum@example.com`. Cela n'empêche pas de créer un champ `email` dans d'autres entités (par exemple dans `Bar`).

`Association name` désigne le nom de la variable qui contiendra la ou les entités associées à l'entité `Foo`. Si on veut assosier `Bar` à `Foo`, l'association doit être nommée `bar`.

`Bidirectional` veut dire « réciproque » :

- si l'association est bidirectionnelle, l'entité associée possède aussi une reférénce à l'entité en cours  
  `Foo` possède une référence à `Bar` et `Bar` possède aussi une référence à `Foo`
- si l'association n'est bidirectionnelle, seule une des deux entités associées possède une reférénce à l'autre entité  
  `Foo` possède une référence à `Bar` mais `Bar` ne possède pas de référence à `Foo`

`Inversed by` désigne le nom de la variable qui contient l'association réciproque, c-à-d, la ou les entités associées à l'entité dans l'autre entité. Dans l'entité `Bar`, l'association est inversée au travers de la variable `foo`.

## Doc

- [Remg/GeneratorBundle: Code generation tools for Symfony3.](https://github.com/Remg/GeneratorBundle)
