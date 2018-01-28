# Vagrant

Vagrant est un outil de gestion de machines virtuelles. Cet outil permet de notamment de manipuler simplement VM Virtualbox.

VM = virtual machine (machine virtuelle)
box = boîte (c-à-d un serveur, ou l'image disque d'un serveur)

## Installation

Installer :

- [Git](git.md)
- [Ruby](ruby.md) (seulement pour Windows)
- [VirtualBox](https://www.virtualbox.org/) (seul « VirtualBox 5.x.x platform packages » est nécessaire)
- [Vagrant](https://www.vagrantup.com/downloads.html)
- [PuTTY](https://www.chiark.greenend.org.uk/~sgtatham/putty/latest.html) (seulement pour Windows et seul « MSI (‘Windows Installer’) » est nécessaire)

## Plugin

Installer un plugin qui installe automatiquement les `virtualbox-guest-additions` :

    vagrant plugin install vagrant-vbguest

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

## Commandes utiles

Construire la VM ou la démarrer si elle existe déjà :

    vagrant up

Arrêter la VM :

    vagrant halt

Afficher l'état de la VM :

    vagrant status

Se connecter en ssh avec la VM :

    vagrant ssh

Provisionner la VM :

    vagrant provision

Détruire définitivement la machine (attention : pas récupérable) :

    vagrant destroy -f

Afficher des informations sur la connexion ssh avec la VM :

    vagrant ssh-config

## Fichier `Vagrantfile`

### Réseau privé

    config.vm.network "private_network", ip: "192.168.33.10"

### Accès au répertoire du projet

    config.vm.synced_folder ".", "/var/www/html", owner: "root", group: "root"

### Provisionning

    config.vm.provision "shell", path: "provision/setup.sh"

## Test du serveur web

Ouvrir l'url suivante dans un navigateur web :

    http://192.168.33.10/

## Trouble shooting

### Avec Windows, Vagrant bloque après `vagrant up`

Solution : mettre à jour PowerShell.

La page [Install and configure WMF 5.1 | Microsoft Docs](https://docs.microsoft.com/en-us/powershell/wmf/5.1/install-configure) permet de télécharger PowerShell 5.1.

Dans le PowerShell, la commande `$PSVersionTable` affiche le numéro de version actuel.

### Affichage de l'écran de la box

D'abord stopez la vm si elle tourne.
Ensuite lancez la manuellement depuis virtualbox.

Dans virtualbox, la vm devrait porter un nom semblable à `my_project_default_...`.

Les messages d'erreur qui s'affichent permettent de diagnostiquer les problèmes.

### Erreur `VT-x is disabled` ou `VT-x is not available`

Solution : activer la virtualisation dans le BIOS (VT-x pour Intel et AMD-V pour AMD)

- [BIOS : Forum Aux Questions - Aide de Windows](http://windows.microsoft.com/fr-fr/windows/bios-faq#1TC=windows-7)
- [BIOS - Accéder au setup du Bios](http://www.commentcamarche.net/faq/389-bios-acceder-au-setup-du-bios)
- [How to Enable Intel Virtualization Technology (vt-x) and amd-v in BIOS - Updated](http://www.sysprobs.com/disable-enable-virtualization-technology-bios)

Si aucune option VT-x ou AMD-V n'est disponible dans le BIOS, c'est que le PC ne gère pas la virtualisation.

## Doc

- [Git](https://www.git-scm.com/)
- [Oracle VM VirtualBox](https://www.virtualbox.org/)
- [Le langage Ruby](https://www.ruby-lang.org/fr/)
- [Vagrant by HashiCorp](https://www.vagrantup.com/)
- [dotless-de/vagrant-vbguest: A Vagrant plugin to keep your VirtualBox Guest Additions up to date](https://github.com/dotless-de/vagrant-vbguest)
- [Vagrant box debian/stretch64 - Vagrant Cloud](https://app.vagrantup.com/debian/boxes/stretch64)
- [PuTTY: a free SSH and Telnet client](https://www.chiark.greenend.org.uk/~sgtatham/putty/)
- [Determine installed PowerShell version - Stack Overflow](https://stackoverflow.com/questions/1825585/determine-installed-powershell-version)
- [Installing Windows PowerShell | Microsoft Docs](https://docs.microsoft.com/en-us/powershell/scripting/setup/installing-windows-powershell?view=powershell-5.1)
- [Install and configure WMF 5.1 | Microsoft Docs](https://docs.microsoft.com/en-us/powershell/wmf/5.1/install-configure)
- [Laravel Homestead - Laravel - The PHP Framework For Web Artisans](https://laravel.com/docs/master/homestead)
