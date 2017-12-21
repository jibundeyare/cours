# Twig

Twig est un moteur de « template ». Un « template » est un fichier dans lequel on définit la façon dont des données vont être affichées.

Dans le modèle « MVC », le template correspond au « V », la vue.

## Sans framework (PHP brut)

Exemple en php sans framework.

`hello-twig.php`

    <?php

    require __DIR__.'/../vendor/autoload.php';

    $loader = new Twig_Loader_Filesystem(__DIR__.'/../templates');
    $twig = new Twig_Environment($loader);

    $greeting = 'Hello Twig!';

    $template = $twig->load('hello-twig.html.twig');
    echo $template->render([
        'greeting' => $greeting,
    ]);

`hello-twig.html.twig`

    <!DOCTYPE html>
    <html>
        <head>
            <meta charset="utf-8">
            <title>{{ greeting }}</title>
        </head>
        <body>
            <h1>{{ greeting }}</h1>
        </body>
    </html>

## Doc

- [Documentation - Twig - The flexible, fast, and secure PHP template engine](https://twig.symfony.com/doc/2.x/)
- [Installation - Documentation - Twig - The flexible, fast, and secure PHP template engine](https://twig.symfony.com/doc/2.x/installation.html)
- [Twig for Developers - Documentation - Twig - The flexible, fast, and secure PHP template engine](https://twig.symfony.com/doc/2.x/api.html)
- [verbatim - Documentation - Twig - The flexible, fast, and secure PHP template engine](https://twig.symfony.com/doc/2.x/tags/verbatim.html)
- [Filters - Documentation - Twig - The flexible, fast, and secure PHP template engine](https://twig.symfony.com/doc/2.x/filters/index.html)
- [for - Documentation - Twig - The flexible, fast, and secure PHP template engine](https://twig.symfony.com/doc/2.x/tags/for.html)
- [if - Documentation - Twig - The flexible, fast, and secure PHP template engine](https://twig.symfony.com/doc/2.x/tags/if.html)

- [PHP: date - Manual](http://php.net/manual/en/function.date.php)
- [PHP: number_format - Manual](http://php.net/manual/en/function.number-format.php)

