# SSH

Le protocole SSH permet d'accéder au terminal d'un serveur distant.
On peut utiliser la ligne de commande du terminal comme si un clavier et un écran étaient directement branchés dessus.

Pensez à jeter un coup d'œil au cours sur l'admin sys [admin-sys.md](admin-sys.md) et la sécurité [security.md](security.md).

## Se connecter à un serveur distant

Avant toute connexion, le serveur vous demandera de vous authentifier, c-à-d prouver que vous êtes bien qui vous prétendez être.
Il y a deux façons très répandues de s'authentifier :

- par mot de passe
- par clé SSH

Si vous n'avez pas encore de clé SSH, le serveur vous demandera automatiquement un mot de passe.

Remplacez `user` par votre nom d'utilisateur et `host` par l'adresse ip ou le nom de domaine de votre serveur :

    ssh user@host

Exemple pour se connecter au serveur `185.71.10.24` avec le compte `root` :

    ssh root@185.71.10.24

Exemple pour se connecter au serveur `185.71.10.24` avec le compte `foo` :

    ssh foo@185.71.10.24

Exemple pour se connecter au serveur `example.com` avec le compte `foo` :

    ssh foo@example.com

**Attention** : lors de la première connexion, le client SSH vous demandera si vous acceptez ou non la signature du serveur distant.
Si vous décidez de l'accepter, il faudra bien taper `yes` en toute lettre, sans quoi le client SSH croira que vous ne voulez pas l'accepter.

Exemple d'écran lors d'une première connection au serveur `example.com` avec le compte `foo` :

    $ ssh foo@example.com
    The authenticity of host 'example.com (123.123.123.123)' can't be established.
    ECDSA key fingerprint is SHA256:bMAT6dKtxN3aplAiXdoFPISWKId+c2A/+LZ5pSE0Egl.
    Are you sure you want to continue connecting (yes/no/[fingerprint])? yes
    Warning: Permanently added 'example.com' (ECDSA) to the list of known hosts.
    foo@example.com's password:
    Linux vps123456 5.x.x-x-amd64 #1 SMP Debian 5.x.xx-x+debxux (2020-01-01) x86_64

    The programs included with the Debian GNU/Linux system are free software;
    the exact distribution terms for each program are described in the
    individual files in /usr/share/doc/*/copyright.

    Debian GNU/Linux comes with ABSOLUTELY NO WARRANTY, to the extent
    permitted by applicable law.
    You have new mail.
    Last login: Thu Jan 01 00:00:00 2020 from 234.234.234.234
    foo@vps123456:~$

## Générer des clés SSH

Dans le terminal :

    ssh-keygen -t rsa -b 4096
    ssh-keygen -t dsa
    ssh-keygen -t ecdsa -b 521
    ssh-keygen -t ed25519

Si c'est la première fois que vous créez des clés SSH, vous pouvez valider les choix par défaut.

Pour un usage qui ne nécessite pas de sécurité maximale (par exemple se connecter à son VPS perso qui n'héberge que quelques sites web en PHP), je recommande de laisser vide le champ `passphrase`.
Ainsi vous ne devrez pas renseigner le mot de passe à chaque fois que vous utiliserez vos clés SSH.
Par contre si vous utilisez votre clé SSH pour vos connectez à votre portefeuille de Bitcoin (par exemple), un mot de passe est **fortement recommandé**.

## Copier ses clés publiques sur un serveur distant

Pour bénéficier de l'authentification par clé SSH (et donc sans mot de passe), vous devez d'abord copier les clés publiques sur le serveur distant auquel vous voulez vous connecter.

Remplacez `user` par votre nom d'utilisateur et `host` par l'adresse ip ou le nom de domaine de votre serveur :

    ssh-copy-id user@host

Exemple pour copier la clé ssh dans le compte `root` du serveur `example.com` :

    ssh-copy-id root@example.com

Exemple pour copier la clé ssh dans le compte `foo` du serveur `example.com` :

    ssh-copy-id foo@example.com

### Copier sa clé publique pour l'utilisateur `root`

Ceci permet de se connecter au compte `root` avec sa clé ssh.

Dans la home de votre vps, tapez :

    cat .ssh/authorized_keys | sudo tee -a /home/root/.ssh/authorized_keys

## Sécuriser le service SSH

### Fail2ban

Voir la section « Sécurisation avec fail2ban » dans [admin-sys.md](admin-sys.md#Sécurisation) avec fail2ban.

### Configuration plus stricte

**Attention : quand vous faites les manipulations de cette section, veillez à toujours avoir un terminal `root` actif.**

Si vous faites des erreurs de manipulation dans la config et que vous n'arrivez plus à vous connecter en SSH, ce terminal vous permettra de revenir à une situation antérieure et vous sauver.

Si vous n'avez pas pris cette précaution et que n'arrivez plus à vous connecter, votre seule chance est de recourir à un démarrage du serveur en mode *rescue*.
L'admin de votre hébergeur devrait vous permettre de faire ça et de recevoir une adresse IP et un mot de passe root temporaires.
Après connexion, il faudra :

- monter le disque de votre serveur sur le système de fichier du système de sauvetage
- corriger les erreurs dans le fichier de config de SSH
- redémarrer le serveur en mode normal et tester votre accès SSH
- recommencer si ça ne fonctionne toujours pas

#### Changer le port SSH

*Cette config est optionnelle.*

Par défaut le port SSH est le port 22.
Vous pourrez éviter beaucoup d'attaques automatisées juste en changeant le numéro du port.

D'abord, choisissez un nombre entre `49152` et `65535`.

Sur le serveur, en tant que `root`, ouvrez le fichier `/etc/ssh/sshd_config` :

    sudo nano /etc/ssh/sshd_config

Retrouvez la ligne avec le paramètre désactivé `#Port 22`.

Juste en dessous de cette ligne, ajoutez le paramètre (en remplaçant `54321` par le nombre que vous avez choisi) :

    Port 54321

Enregistrez puis relancez votre service ssh :

    sudo systemctl restart sshd

Puis en local, vérifiez que vous arrivez à vous connecter avec le nouveau port (en remplaçant `54321` par le nombre que vous avez choisi) :

    ssh -p 54321 foo@example.com

Si ça ne fonctionne pas, vérifiez qu'il n'y a pas un firewall qui bloque ce port en local ou sur le serveur.

#### Désactiver l'authentification par mot de passe pour le compte `root`

L'authentification par mot de passe ouvre la voie aux attaques dites de force brute (*brute force* en anglais).
Pour cette raison, il vaut mieux la désactiver et n'autoriser que l'authentification par clé.

En local, si vous ne l'avez pas déjà fait, vous devez copier votre clé sur le serveur pour pouvoir vous connecter avec un compte `root` :

    ssh-copy-id root@example.com

*Pensez à ajouter l'option `-p` si vous avez customisé le port.*

Sur le serveur, en tant que `root`, ouvrez le fichier `/etc/ssh/sshd_config` :

    sudo nano /etc/ssh/sshd_config

Retrouvez la ligne avec le paramètre désactivé `#PermitRootLogin yes`.

Juste en dessous de cette ligne, ajoutez le paramètre :

    PermitRootLogin without-password

Enregistrez puis relancez votre service ssh :

    sudo systemctl restart sshd

Puis en local, vérifiez que vous arrivez à vous connecter avec votre compte `root` sans mot de passe :

    ssh root@example.com

*Pensez à ajouter l'option `-p` si vous avez customisé le port.*

#### Désactiver l'authentification par mot de passe pour tous les utilisateurs

En local, si vous ne l'avez pas déjà fait, vous devez copier votre clé sur le serveur pour pouvoir vous connecter avec votre compte utilisateur `foo` :

    ssh-copy-id foo@example.com

*Pensez à ajouter l'option `-p` si vous avez customisé le port.*

Sur le serveur, en tant que `root`, ouvrez le fichier `/etc/ssh/sshd_config` :

    sudo nano /etc/ssh/sshd_config

Retrouvez la ligne avec le paramètre désactivé `#PasswordAuthentication yes`.

Juste en dessous de cette ligne, ajoutez le paramètre :

    PasswordAuthentication no

Enregistrez puis relancez votre service ssh :

    sudo systemctl restart sshd

Puis en local, vérifiez que vous arrivez à vous connecter avec votre compte `foo` sans mot de passe :

    ssh foo@example.com

*Pensez à ajouter l'option `-p` si vous avez customisé le port.*

#### Autoriser l'accès à certains utilisateurs depuis certaines adresses IP seulement

*Cette config est optionnelle.*

Il est possible de n'autoriser l'accès SSH qu'à certains utilisateurs depuis certaines adresses IP.

Par exemple :

- le compte `root` seulement depuis l'adresse IP du travail
- le compte `foo` seulement depuis l'adresse IP du travail et du domicile
- tous les autres accès seront refusés

Sur le serveur, en tant que `root`, ouvrez le fichier `/etc/ssh/sshd_config` :

    sudo nano /etc/ssh/sshd_config

Pour n'autoriser que l'accès du compte `foo` depuis une seule adresse IP, ajoutez à la toute fin du fichier :

    AllowUsers foo@123.123.123.123

Ou pour autoriser l'accès avec le compte `foo` et le compte `root` que depuis une seule adresse IP, ajoutez à la toute fin du fichier :

    AllowUsers foo@123.123.123.123 root@123.123.123.123

Vous avez compris le système, je suppose.

Pour autoriser l'accès avec le compte `foo` et le compte `root` depuis deux adresses IP, ajoutez à la toute fin du fichier :

    AllowUsers root@123.123.123.123 root@234.234.234.234 foo@123.123.123.123 foo@234.234.234.234

Enregistrez puis relancez votre service ssh :

    sudo systemctl restart sshd

Puis en local, vérifiez que vous arrivez à vous connecter avec votre compte `root` sans mot de passe :

    ssh root@example.com

*Pensez à ajouter l'option `-p` si vous avez customisé le port.*

#### Autoriser l'accès par mot de passe à certains utilisateurs depuis certaines adresses IP seulement

*Cette config est optionnelle.*

Il est possible de customiser l'autorisation d'accès par mot de passe pour certains utilisateurs

Pour autoriser l'authentification par mot de passe pour le compte `foo` depuis une seule adresse IP, ajoutez à la toute fin du fichier :

    Match User foo Address 123.123.123.123
        PasswordAuthentication yes

Ou pour autoriser l'authentification par mot de passe pour le compte `foo` depuis deux adresses IP, ajoutez à la toute fin du fichier :

    Match User foo Address 123.123.123.123,234.234.234.234
        PasswordAuthentication yes

Si un autre utilisateur `bar` doit pouvoir avoir les mêmes privilèges, ajoutez-le au bloc :

    Match User foo bar Address 123.123.123.123,234.234.234.234
        PasswordAuthentication yes

Si un autre utilisateur doit avoir des privilèges différents, vous pouvez rajouter un nouveau bloc.

Pour autoriser l'authentification par mot de passe pour le compte `root` depuis une seule adresse IP, ajoutez à la toute fin du fichier :

    Match User root Address 123.123.123.123
        PermitRootLogin yes

Ou pour la même chose depuis deux adresses IP, ajoutez à la toute fin du fichier :

    Match User root Address 123.123.123.123,234.234.234.234
        PermitRootLogin yes

Enregistrez puis relancez votre service ssh :

    sudo systemctl restart sshd

Puis en local, vérifiez que vous arrivez à vous connecter avec votre compte `root` sans mot de passe :

    ssh root@example.com

*Pensez à ajouter l'option `-p` si vous avez customisé le port.*

## Création de clés SSH avec PuTTY dans Windows

Voir les guides suivante (en anglais) :

- [Using PuTTYgen on Windows to generate SSH key pairs](https://www.ssh.com/ssh/putty/windows/puttygen)
- un peu plus sécurisé et détaillé : [Use Public Key Authentication with SSH | Linode](https://www.linode.com/docs/security/authentication/use-public-key-authentication-with-ssh/#windows).

## Converssion d'une clé privée PuTTY en clé privée OpenSSH dans Windows

1. ouvrez PuTTYGen
2. chargez le fichier `.ppk` de la clé que vous voulez convertir
3. dans la barre de menu, cliquez sur l'entrée `Conversions`
4. dans le menu déroulant, cliquez sur l'entrée `Export OpenSSH key`
5. sauvez le fichier dans le dossier `.ssh` de votre home en lui donnant le nom de la clé
6. si PuTTYGen vous demande de confirmer qu'il n'y a pas de passphrase, répondez « oui »

Note : le nom du fichier doit être le type de clé sans aucune extension.
Exemple : pour une clé RSA, le nom du fichier est `id_rsa`.

## Converssion d'une clé publique PuTTY en clé publique OpenSSH dans Windows

1. avec un éditeur de code, ouvrez le fichier `.ppk` de la clé que vous voulez convertir
2. copiez dans le press-papier les six lignes de la clé publique
3. créez un nouveau fichier dans le dossier `.ssh` de votre home en lui donnant le nom de la clé et l'extension `.pub`
5. dans ce nouveau fichier, tapez le code qui correspond au type de clé
4. collez les six lignes de code de la clé publique
5. supprimez tous les saut de ligne de façon à ce que toute la clé publique soit écrite sur une seule ligne

Note : le code de la type de clé est créé en utilisant le préfixe `ssh-` et le type de la clé.
Exemple : pour une clé RSA, le code du type de la clé est `ssh-rsa`.

Note : le nom du fichier doit être le type de clé avec l'extension `.pub`.
Exemple : pour une clé RSA, le nom du fichier est `id_rsa.pub`.

Voici un exemple de contenu du fichier `.ppk` d'une clé RSA :

    PuTTY-User-Key-File-2: ssh-rsa
    Encryption: aes256-cbc
    Comment: rsa-key-20170126
    Public-Lines: 6
    AAAAB3NzaC1yc2EAAAABJQAAAQEAx+52s7vvaZ8rT2UdFZWlSUVDHDQ+5ZRFvgRW
    6nm2sO1c9WqNg7u2PQL4jeKSDX2XWcMnpleALz2x8Rr4rMy5E1iZzvWov8VtFd8l
    fa9HOkgEeJB3VFuYR3NlnD3eyCoYJYPVpHJHrIeui2WZs5vQ76HDe+th8+z5Ald4
    zPw3p2c6ZJpBrkSBM67hWokoBDi23c7NhszDHhJBrv+B98cQxnagI1PUKqN7E8Vg
    bNtBI8beIMHyI69up9G1AXSEi0cGIjYNx9RNUPau1mRk/SvfqxgWkAjM005lj7hc
    bOsjbdKK3T2NtrKTaYjEiXlEXcj1iGuApsD/m73pYaEJB3Nd7w==
    Private-Lines: 14
    MoaDbq0owouN/7Z9Pga0favDhM2bSEgMErJBxdDmNUXIVVcUoLiD/Ps1RA+BeBBX
    wxqKUt9PqLy/pnafPR/i2xjJiQtQ0CWkPxND16Gi1dqLzmbQYYl1ev4+LzuG0zNX
    HDGMvRiwagY7mY+F1tUjBYfOL6z8XHw4m40YcY1QorOO+0MMzAVT5Hkg8YyXW209
    B/V/LRADFMVA2BlL39y11cb5ZpFStPH/waYUMY+2w1ZmJZ7dcRoMjuKmY+YE/tUx
    n9X3P0qTNSbw6e6sMG3Dhr1vfoJUQWApUliD6GpUiCeIvXBcVqG8Vsfq9XADsPl0
    +nFAwjSZflywcB7/FwhGb7q5UmcJK06SzoMl7Og5e3g7NCs3yNNQIv+qCpDjhxrA
    hpT03mbipu7OXCZDeUwUhMGJAmYHE5iqm1rPCsSVbaMgpxhCWf01Cx4gLx3aMvn4
    MdylA31GuL3wSxcWTslrOI8+449lZN/qZEnGEZkYTrnlu123jTqsAWMMtuHSz2Ig
    6GA89oTdlppkNflhNH3OJ85kMUrc3p/ZBMdndz8jTDTljmJjHR5oNMoShFof115A
    nWjUHqBwCgcubLYyH3afDvBTOhtl0tJ9Oby0wJlOAGnCXiPSDbF/y7J7xml/PS9t
    XlSVNxtAY15NDO6Fp96sBVfKuJsfJ90PzdBom4ikIuf7sMwtElrHHLuYfcXJQYLp
    G5jBmqDgnirosVPEBIxlxFzz/HCRmdU+tsYg46gqI4R5UpKUe8WSaJoZkDGsrqhm
    e+1SJaBuafR4v2bx/bV414Hg7LGQosK2S3crxH4UgZl+g02vWznZfBH+9CmHvKDR
    AxfcKOTzsaILKJtQPV81lmJ45sARYMcB5jMiE4kBg56hiXouChsvKkm55WVcW1E+
    Private-MAC: 17512c9f7582c1d9c3ae2b01b4d67a6b1dbd1d0e

La clé publique est contenue entre les balises `Public-Lines: 6` et `Private-Lines: 14` :

    Public-Lines: 6
    AAAAB3NzaC1yc2EAAAABJQAAAQEAx+52s7vvaZ8rT2UdFZWlSUVDHDQ+5ZRFvgRW
    6nm2sO1c9WqNg7u2PQL4jeKSDX2XWcMnpleALz2x8Rr4rMy5E1iZzvWov8VtFd8l
    fa9HOkgEeJB3VFuYR3NlnD3eyCoYJYPVpHJHrIeui2WZs5vQ76HDe+th8+z5Ald4
    zPw3p2c6ZJpBrkSBM67hWokoBDi23c7NhszDHhJBrv+B98cQxnagI1PUKqN7E8Vg
    bNtBI8beIMHyI69up9G1AXSEi0cGIjYNx9RNUPau1mRk/SvfqxgWkAjM005lj7hc
    bOsjbdKK3T2NtrKTaYjEiXlEXcj1iGuApsD/m73pYaEJB3Nd7w==
    Private-Lines: 14

Le nouveau fichier `id_rsa.pub` devra ressembler à ça :

    ssh-rsa AAAAB3NzaC1yc2EAAAABJQAAAQEAx+52s7vvaZ8rT2UdFZWlSUVDHDQ+5ZRFvgRW6nm2sO1c9WqNg7u2PQL4jeKSDX2XWcMnpleALz2x8Rr4rMy5E1iZzvWov8VtFd8lfa9HOkgEeJB3VFuYR3NlnD3eyCoYJYPVpHJHrIeui2WZs5vQ76HDe+th8+z5Ald4zPw3p2c6ZJpBrkSBM67hWokoBDi23c7NhszDHhJBrv+B98cQxnagI1PUKqN7E8VgbNtBI8beIMHyI69up9G1AXSEi0cGIjYNx9RNUPau1mRk/SvfqxgWkAjM005lj7hcbOsjbdKK3T2NtrKTaYjEiXlEXcj1iGuApsD/m73pYaEJB3Nd7w==

## Doc

- [How to use ssh-keygen to generate a new SSH key | SSH.COM](https://www.ssh.com/ssh/keygen/)
- [Changer le mot de passe root sur un VPS | Documentation OVH](https://docs.ovh.com/fr/vps/root-password/)
- [My First 5 Minutes On A Server; Or, Essential Security for Linux Servers](https://plusbryan.com/my-first-5-minutes-on-a-server-or-essential-security-for-linux-servers)
- [How to allow root login from one IP address with ssh public keys only - nixCraft](https://www.cyberciti.biz/faq/match-address-sshd_config-allow-root-loginfrom-one_ip_address-on-linux-unix/)
- [Registered port - Wikipedia](https://en.wikipedia.org/wiki/Registered_port)
- [Installation of OpenSSH For Windows | Microsoft Docs](https://docs.microsoft.com/en-us/windows-server/administration/openssh/openssh_install_firstuse)

