# Symfony 4.4 - Swiftmailer

Cette fiche montre comment utiliser Swiftmailer pour envoyer des mails
en utilisant directement un serveur SMTP ou un service de
transfert de mail.

## Exemple

[https://github.com/jibundeyare/src-symfony-4.4-swiftmailer](https://github.com/jibundeyare/src-symfony-4.4-swiftmailer)

## Installation de Swiftmailer

```bash
composer require symfony/swiftmailer-bundle
```

## Configuration type avec un serveur SMTP

Dans votre fichier `.env.local`, ajoutez la ligne suivante en prenant
soin de personnaliser les paramètres à votre cas :

```bash
MAILER_URL=smtp://smtp.example.org:465?encryption=ssl&auth_mode=login&username=foo.bar@example.com&password=123
```

### Exemple avec Gmail et un « app password » :

```diff-bash
- MAILER_URL=smtp://smtp.example.org:465?encryption=ssl&auth_mode=login&username=foo.bar@example.com&password=123
+ MAILER_URL=smtp://smtp.gmail.com:587?encryption=tls&auth_mode=login&username=john.doe@gmail.com&password=abc
```

## Configuration avec un service de redirection de mail

Dans votre fichier `.env.local`, ajoutez la ligne suivante :

```diff-bash
MAILER_URL=sendmail://localhost?command=/usr/sbin/sendmail%20-t
```

## Création d'un contrôleur

On créé un contrôleur sans template :

```bash
php bin/console make:controller --no-template MailController
```

On ajoute le code de Swiftmailer dans le contrôleur :

```diff-php
-     public function index(): Response
+     public function index(\Swift_Mailer $mailer): Response
      {
+         // Email du destinataire
+         $toEmail = 'bar.baz@example.com';
+
+         // Choix du sujet du mail
+         $message = (new \Swift_Message('Hello Swiftmailer!'))
+             // Choix de l'adresse de l'expéditeur
+             // Même si mettez une adresse "from" différente du compte SMTP,
+             // l'adresse du compte SMTP sera visible dans l'entête du mail
+             ->setFrom(['foo.bar@example.com' => 'Foo Bar'])
+             // Choix de l'adresse du destinataire
+             ->setTo([$toEmail])
+             // Choix du message au format HTML
+             ->setBody(
+                 $this->renderView('emails/hello.html.twig', [
+                     'toEmail' => $toEmail
+                 ]),
+                 'text/html'
+             )
+             // Choix du message au format texte
+             ->addPart(
+                 $this->renderView('emails/hello.txt.twig', [
+                     'toEmail' => $toEmail
+                 ]),
+                 'text/plain'
+             )
+         ;
+
+         $mailer->send($message);
+
          return $this->json([
-             'message' => 'Welcome to your new controller!',
+             'message' => 'Email envoyé',
              'path' => 'src/Controller/MailController.php',
          ]);
      }
```

## Création de templates

### Le template HTML

On créé le fichier `templates/emails/hello.html.twig` :

```twig
<h1>Hello <a href="mailto:{{ toEmail }}">{{ toEmail }}</a>!</h1>
```

### Le template TXT

On créé le fichier `templates/emails/hello.txt.twig` :

```twig
Hello {{ toEmail }}!
```

## Test

Vous pouvez lancer le serveur web :

```bash
symfony server
```

Et visiter la page [http://localhost:8000/mail](http://localhost:8000/mail).

## Doc

- [Swift Mailer (Symfony 4.4 Docs)](https://symfony.com/doc/4.4/email.html)
- [Swiftmailer: A feature-rich PHP Mailer – Documentation – Swift Mailer](https://swiftmailer.symfony.com/docs/introduction.html)

