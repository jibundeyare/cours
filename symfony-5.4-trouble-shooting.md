# Symfony 5.4 trouble shooting

## Flûte, j'ai oublié d'ajouter l'option `--webapp`

Si on oublie l'option `--webapp` quand on tape la commande :

    symfony new --version=lts foo

On se retrouve avec une version du framework avec juste le strict minimum...

Si on veut rattrapper son erreur sans supprimer le projet et le recréer, voici les commandes à taper :

    composer require asset expression-language form http-client intl logger mailer orm-pack process security sensio/framework-extra-bundle serializer-pack translation twig-pack validator web-link
    composer require --dev debug-pack maker-bundle profiler-pack test-pack

