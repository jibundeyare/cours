# Virtualbox & Vagrant

Virtualbox est une application d'émulation de machine (machine virtuelle).

Vagrant est un outil de gestion de machines virtuelles.

## Installation

Installer :

- [Git - Downloads](https://www.git-scm.com/downloads)
- [RubyInstaller](https://rubyinstaller.org/downloads/) (seulement pour Windows)
- [VirtualBox](https://www.virtualbox.org/) (seul « VirtualBox 5.x.x platform packages » est nécessaire)
- [Vagrant](https://www.vagrantup.com/downloads.html)
- [PuTTY](https://www.chiark.greenend.org.uk/~sgtatham/putty/latest.html) (seulement pour Windows et seul « MSI (‘Windows Installer’) » est nécessaire)

## Création d'une box linux Debian Stretch

Attention : prévoir au moins 3 Go d'espace libre sur la machine pour pouvoir travailler sereinement.

Ouvrez un terminal (pour Windows utilisez `Git Bash`). Puis se rendre dans le dossier des projets web.

Dans le dossier des projets, créer un nouveau répertoire puis initialiser une nouvelle box avec vagrant (adaptez le nom du projet) :

    mkdir my_project
    cd my_project
    vagrant init debian/stretch64
    vagrant up
    vagrant ssh

Si vous voyez le message suivant c'est que vous êtes parvenu à vous connecter à la box et que l'installation est réussie :

    Linux stretch 4.9.0-4-amd64 #1 SMP Debian 4.9.65-3 (2017-12-03) x86_64

    The programs included with the Debian GNU/Linux system are free software;
    the exact distribution terms for each program are described in the
    individual files in /usr/share/doc/*/copyright.

    Debian GNU/Linux comes with ABSOLUTELY NO WARRANTY, to the extent
    permitted by applicable law.
    Last login: Tue Jan  9 10:41:53 2018 from 10.0.2.2
    vagrant@stretch:~$

## Fermeture d'une box linux

Pour sortir de la box et l'éteindre :

    exit
    vagrant halt

## Trouble shooting

### Avec Windows, Vagrant bloque après `vagrant up`

Il se peut qu'il faille mettre à jour PowerShell.

La page [Install and configure WMF 5.1 | Microsoft Docs](https://docs.microsoft.com/en-us/powershell/wmf/5.1/install-configure) permet de télécharger PowerShell 5.1.

Dans le PowerShell, la commande `$PSVersionTable` affiche le numéro de version actuel.

## Doc

- [Git](https://www.git-scm.com/)
- [Oracle VM VirtualBox](https://www.virtualbox.org/)
- [Le langage Ruby](https://www.ruby-lang.org/fr/)
- [Vagrant by HashiCorp](https://www.vagrantup.com/)
- [Vagrant box debian/stretch64 - Vagrant Cloud](https://app.vagrantup.com/debian/boxes/stretch64)
- [PuTTY: a free SSH and Telnet client](https://www.chiark.greenend.org.uk/~sgtatham/putty/)
- [Determine installed PowerShell version - Stack Overflow](https://stackoverflow.com/questions/1825585/determine-installed-powershell-version)
- [Installing Windows PowerShell | Microsoft Docs](https://docs.microsoft.com/en-us/powershell/scripting/setup/installing-windows-powershell?view=powershell-5.1)
- [Install and configure WMF 5.1 | Microsoft Docs](https://docs.microsoft.com/en-us/powershell/wmf/5.1/install-configure)
