# EasyAdmin Bundle

EasyAdmin est un bundle Symfony qui permet de construire des back-offices.

## Installation

Dans un terminal, si vous n'êtes pas déjà dans la dossier racine de votre projet, taper :

    cd [dossier du projet web]

Pour installer le bundle :

    composer require javiereguiluz/easyadmin-bundle

Pour activer le bundle, ouvrir le fichier `app/AppKernel.php` et modifier le bloc :

    public function registerBundles()
    {
        $bundles = [
            new Symfony\Bundle\FrameworkBundle\FrameworkBundle(),
            new Symfony\Bundle\SecurityBundle\SecurityBundle(),
            new Symfony\Bundle\TwigBundle\TwigBundle(),
            new Symfony\Bundle\MonologBundle\MonologBundle(),
            new Symfony\Bundle\SwiftmailerBundle\SwiftmailerBundle(),
            new Doctrine\Bundle\DoctrineBundle\DoctrineBundle(),
            new Sensio\Bundle\FrameworkExtraBundle\SensioFrameworkExtraBundle(),
            new AppBundle\AppBundle(),
        ];

afin d'obtenir :

    public function registerBundles()
    {
        $bundles = [
            new Symfony\Bundle\FrameworkBundle\FrameworkBundle(),
            new Symfony\Bundle\SecurityBundle\SecurityBundle(),
            new Symfony\Bundle\TwigBundle\TwigBundle(),
            new Symfony\Bundle\MonologBundle\MonologBundle(),
            new Symfony\Bundle\SwiftmailerBundle\SwiftmailerBundle(),
            new Doctrine\Bundle\DoctrineBundle\DoctrineBundle(),
            new Sensio\Bundle\FrameworkExtraBundle\SensioFrameworkExtraBundle(),
            new AppBundle\AppBundle(),
            new EasyCorp\Bundle\EasyAdminBundle\EasyAdminBundle(),
        ];

Pour configurer les routes du bundle, ouvrir le fichier `app/config/routing.yml` et ajouter le bloc suivant à la fin :

    easy_admin_bundle:
        resource: "@EasyAdminBundle/Controller/AdminController.php"
        type:     annotation
        prefix:   /admin

Pour déployer les fichiers CSS et JavaScript du bundle :

    php bin/console assets:install --symlink

## Configuration de l'admin

Partons du principe que nous avons trois entités et qu'elles ont les relations suivantes :

![Diagramme de classe Task Category Tag](img/class-diagram-task-category-tag.png)

Pour définir les entités que EasyAdmin peut manipuler, ouvrir le fichier `app/config/config.yml` et ajouter le bloc suivant à la fin :

    # javiereguiluz/easyadmin-bundle
    easy_admin:
        formats:
            date: 'd/m/Y'
            time: 'H:i'
            datetime: 'd/m/Y H:i:s'
        entities:
            Category:
                class: AppBundle\Entity\Category
                form:
                    fields:
                        - 'title'
                        - { property: 'tasks', type: 'entity', type_options: { choice_label: 'title', multiple: true }}
            Tag:
                class: AppBundle\Entity\Tag
                form:
                    fields:
                        - 'title'
                        - { property: 'tasks', type: 'entity', type_options: { choice_label: 'title', multiple: true }}
            Task:
                class: AppBundle\Entity\Task
                form:
                    fields:
                        - 'title'
                        - 'description'
                        - 'deadlineDate'
                        - 'deadlineTime'
                        - 'isDone'
                        - { property: 'tags', type: 'entity', type_options: { choice_label: 'title', multiple: true }}
                        - { property: 'category', type: 'entity', type_options: { choice_label: 'title' }}

Note : veiller à adapter le nom des entités et leurs champs dans la partie `entities`.

## Doc

- [EasyAdminBundle (Symfony Bundles Docs)](http://symfony.com/doc/current/bundles/EasyAdminBundle/index.html)
