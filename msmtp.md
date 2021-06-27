# MSMTP

Cette fiche vous montre comment installer MSMTP, le configurer et le
tester.

MSMTP est un service de transfert de mail à un serveur SMTP.

Cet outil peut vous être utile si vous souhaitez que votre serveur
puisse envoyer des mails sans avoir à installer un serveur SMTP en bonne
et due forme.

## Installation

```bash
sudo apt install msmtp-mta mailutils
```

## Configuration

La configuration ci-dessous est un exemple qui fonctionne avec un
serveur Gmail. Bien entendu d'autres configurations sont possibles.

Création du fichier `/etc/msmtprc` :

```default
defaults
account default
host smtp.gmail.com
port 587
aliases /etc/msmtp-aliases
tls on
tls_starttls on
tls_trust_file /etc/ssl/certs/ca-certificates.crt
syslog LOG_MAIL

from foo.bar@gmail.com

auth on
user foo.bar@gmail.com
password 123
```

Création du fichier `/etc/msmtp-aliases` :

```default
# les rapports d'activité de cron jobs exécutés par un user lambda
# seront envoyés à l'adresse suivante
default: contact@example.com
# les rapports d'activité de cron jobs exécutés par le user root
# seront envoyés à l'adresse suivante
root: contact@example.org
```

## Test

Pour tester votre service de redirection, utilisez la commande
suivante :

```bash
echo "Test message" | mail -s "Test sujet" contact@example.com
```

## Doc

- [msmtp - Debian Wiki](https://wiki.debian.org/msmtp)
- [msmtp 1.8.15](https://marlam.de/msmtp/msmtp.html)

