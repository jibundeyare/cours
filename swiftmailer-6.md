# Swiftmailer 6

Cette fiche montre comment utiliser Swiftmailer pour envoyer des mails
en utilisant directement un serveur SMTP ou un service de
transfert de mail.

## Exemple

[https://github.com/jibundeyare/src-swiftmailer-6](https://github.com/jibundeyare/src-swiftmailer-6)

## Installation de Swiftmailer

```bash
composer require "swiftmailer/swiftmailer:^6.0"
```

## Utilisation avec un serveur SMTP

Créez un nouveau fichier nommé `mail-smtp.php` :

```php
<?php

require_once __DIR__.'/vendor/autoload.php';

// Choix d'un transport de type SMTP
// Pour utiliser le protocole :
// - SMTPS, c-à-d SMTP sur du TLS, mettez 'ssl' (recommandé)
// - SMTP avec STARTTLS, mettez 'tls'
$transport = (new Swift_SmtpTransport('smtp.example.org', 587, 'tls'))
    // Choix du compte mail
    ->setUsername('foo.bar@example.com')
    ->setPassword('123')
;

// Création du Mailer avec le Transport
$mailer = new Swift_Mailer($transport);

// Choix du sujet du mail
$message = (new Swift_Message('Hello Swiftmailer SMTP!'))
    // Choix de l'adresse de l'expéditeur
    // Même si mettez une adresse "from" différente du compte SMTP,
    // l'adresse du compte SMTP sera visible dans l'entête du mail
    ->setFrom(['foo.bar@example.com' => 'Foo Bar'])
    // Choix de l'adresse du destinataire
    ->setTo(['bar.baz@example.com'])
    // Choix du message au format texte
    ->setBody('Hello Swiftmailer SMTP!')
;

// Envoi du message
$result = $mailer->send($message);
```

Adaptez les codes d'accès au serveur SMTP :

```diff-php
- $transport = (new Swift_SmtpTransport('smtp.example.org', 587, 'ssl'))
+ $transport = (new Swift_SmtpTransport('smtp.gmail.com', 587, 'tls'))
      // Choix du compte mail
-     ->setUsername('foo.bar@example.com')
+     ->setUsername('john.doe@gmail.com')
-     ->setPassword('123')
+     ->setPassword('abc')
```

Adaptez l'adresse de l'expéditeur :

```diff-php
-     ->setFrom(['foo.bar@example.com' => 'Foo Bar'])
+     ->setFrom(['john.doe@gmail.com' => 'John Doe'])
```

Choisissez l'adresse du destinataire :

```diff-php
-     ->setTo(['bar.baz@example.com'])
+     ->setTo(['john.doe@lorem.org'])
```

Et testez via le terminal :

```bash
php mail-smtp.php
```

## Utilisation avec un service de transfert de mail

Créez un nouveau fichier nommé `mail-mta.php` :

```php
<?php

require_once __DIR__.'/vendor/autoload.php';

// Choix d'un transport de type sendmail adapté pour msmtp
$transport = new Swift_SendmailTransport('/usr/sbin/sendmail -t');

// Création du Mailer avec le Transport
$mailer = new Swift_Mailer($transport);

// Choix du sujet du mail
$message = (new Swift_Message('Hello Swiftmailer MTA!'))
    // Choix de l'adresse de l'expéditeur
    // Même si mettez une adresse "from" différente du compte SMTP,
    // l'adresse du compte SMTP sera visible dans l'entête du mail
    ->setFrom(['foo.bar@example.com' => 'Foo Bar'])
    // Choix de l'adresse du destinataire
    ->setTo(['bar.baz@example.com'])
    // Choix du message au format texte
    ->setBody('Hello Swiftmailer MTA!')
;

// Envoi du message
$result = $mailer->send($message);

```

Adaptez l'adresse de l'expéditeur :

```diff-php
-     ->setFrom(['foo.bar@example.com' => 'Foo Bar'])
+     ->setFrom(['john.doe@gmail.com' => 'John Doe'])
```

Choisissez l'adresse du destinataire :

```diff-php
-     ->setTo(['bar.baz@example.com'])
+     ->setTo(['john.doe@lorem.org'])
```

Et testez via le terminal :

```bash
php mail-mta.php
```

## Envoyer un mail avec du code HTML

Vous pouvez remplacez le message au format texte par un message au
format HTML.

```diff-php
-     // Choix du message au format texte
-     ->setBody('Hello Swiftmailer MTA!')
+     // Choix du message au format HTML
+     ->setBody('<h1>Hello Swiftmailer MTA!</h1>', 'text/html')
```

Mais il peut être judicieux d'ajouter un message alternatif au format
texte au cas où le client mail ne lit pas le HTML (c'est rare mais ça
arrive).

```diff-php
      ->setBody('<h1>Hello Swiftmailer MTA!</h1>', 'text/html')
+     // Choix du message au format texte
+     ->setPart('Hello Swiftmailer MTA!', 'text/plain')
```

## Ajouter une pièce jointe

```diff-php
      // Choix du message au format texte
      ->setBody('Hello Swiftmailer MTA!')
+     // Ajout d'une pièce jointe
+     ->attach(Swift_Attachment::fromPath('/home/projects/foo/assets/image.jpg'))
```

Vous pouvez retrouver le chemin absolu de l'image à partir d'un chemin
relatif :

```php
$path = realpath(__DIR__.'/assets/image.jpg');
```

## Envoyer un mail à plusieurs destinataires

Pour ajouter des destinataires, vous devez vous assurer que leurs
adresses sont syntaxiquement valides.

```php
<?php

use Egulias\EmailValidator\EmailValidator;
use Egulias\EmailValidator\Validation\RFCValidation;

$validator = new EmailValidator();
$validation = new RFCValidation();

// La fonction isValid() renvoit true ou false
$validator->isValid("example@example.com", $validation);
```

Maintenant vous pouvez utiliser la validation dans une boucle pour ajouter d'autres
destinataires.

```diff-php
+ use Egulias\EmailValidator\EmailValidator;
+ use Egulias\EmailValidator\Validation\RFCValidation;

  require_once __DIR__.'/vendor/autoload.php';

+ $validator = new EmailValidator();
+ $validation = new RFCValidation();

  // ...

+ // Liste des destinataires
+ $toRecipients = ...

+ foreach ($toRecipients as $toRecipient) {
+     if ($validator->isValid($toRecipient, $validation)) {
+         $message->addTo($toRecipient);
+     }
+ }

  // Envoi du message
  $result = $mailer->send($message);
```

Vous pouvez aussi ajouter des destinataires de copies carbone ou de
copies carbone cachées.

```diff-php
+ use Egulias\EmailValidator\EmailValidator;
+ use Egulias\EmailValidator\Validation\RFCValidation;

  require_once __DIR__.'/vendor/autoload.php';

+ $validator = new EmailValidator();
+ $validation = new RFCValidation();

  // ...

+ // Liste des copies carbone
+ $ccRecipients = ...

+ foreach ($ccRecipients as $ccRecipient) {
+     if ($validator->isValid($ccRecipient, $validation)) {
+         $message->addCc($ccRecipient);
+     }
+ }

+ // Liste des copies carbone cachées
+ $bccRecipients = ...

+ foreach ($bccRecipients as $bccRecipient) {
+     if ($validator->isValid($bccRecipient, $validation)) {
+         $message->addBcc($bccRecipient);
+     }
+ }

  // Envoi du message
  $result = $mailer->send($message);
```

## Doc

- [Swiftmailer: A feature-rich PHP Mailer – Documentation – Swift Mailer](https://swiftmailer.symfony.com/docs/introduction.html)

