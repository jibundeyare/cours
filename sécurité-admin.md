# Sécurité pour les admin-sys

## Gestion des risques

- Risques légaux
- Risques financiers
- Risques symboliques

## Types d'attaques

Usurpation d'identité
Escalade de privilèges
Téléversement de code arbitraire
Exécution de code arbitraire

Virus
Bot et botnet
Ver
Trojan horse : cheval de Troie
Spam
Phishing et attaque homographique : parade -> taper manuellement l'URL

Bombe logique
Logiciel espion : key loggers
Backdoor : porte dérobée

Injection SQL : parade -> librairie, échappement des caractères interdits
XSS : parade -> same origin policy
CSRF : parade -> token (jeton)

## Social engineering (ingénierie sociale)

## Parades

### Antivirus

- global pour le réseau
- sur chaque poste

Fonctionnement :

- mode statique : intervient à la demande de l'utilisateur
- mode actif : scan en permanence la mémoire vive, le disque dur et les communications réseau

Détection :

- par recherche de signature : séquence de code connue et remarquable
- par analyse spectrale : séquence de code rare et suspecte
- par analyse heuristique : enchaînement d'actions suspectes

### Pare-feu

Modes :

- configuration manuelle
- mode apprentissage automatique + audit manuel

Filtres :

- bloquage d'adresse IP
- bloquage de port
- bloquage de programme

## Sécurisation du réseau

- tous les appareils administrables : mot de passe
- tous les appareils ayant un rôle clé dans les communications réseau : accès physique verrouillé
- tous les appareils stockant des données clés pour l'entreprise : accès physique verrouillé, crypter les données, backups

## Sécurisation des données

- crypter les données clés pour l'entreprise
- redonder les données sur plusieurs lieux physiques
- attention à l'accès : en sens unique depuis le service de backup

## Sécurisation des postes de travail

- Windows
- MacOS
- Linux

- tous les appareils : mot de passe, pare-feu, antivirus, sauvegarde
- tous les appareils stockant des données clés pour l'entreprise : accès physique verrouillé, backups

### Sécurisation de Windows

#### Attaque : activer le compte admin caché sans mot de passe

Dans un terminal admin :

    net user administrator /active yes

Parade : activer le compte admin, se connecter avec ce compte admin puis lui attribuer un mot de passe

#### Attaque : accéder à un terminal admin

- [2020 How to Reset Windows 7 Password without any Software or Bootable USB/CD/DVD media. - YouTube](https://www.youtube.com/watch?v=5NckCxkZUnw)

## Mot de passe

- [xkcd: Password Strength](https://xkcd.com/936/)
- [How to Choose a Password - Computerphile - YouTube](https://www.youtube.com/watch?v=3NjQ9b3pgIg)
- [Diceware & Passwords - Computerphile - YouTube](https://www.youtube.com/watch?v=Pe_3cFuSw1E)

## Politique de sécurité

- [La Politique de Sécurité des Systèmes d’Information de l’État (PSSIE) | Agence nationale de la sécurité des systèmes d'information](https://www.ssi.gouv.fr/entreprise/reglementation/protection-des-systemes-dinformations/la-politique-de-securite-des-systemes-dinformation-de-letat-pssie/)
- [Top 5 Ways to Handle a Data Breach](https://securitytrails.com/blog/top-5-ways-handle-data-breach)
- [How to Handle a Public Relations Crisis](https://www.businessnewsdaily.com/8935-recover-from-pr-crisis.html)

Réaction à chaud après un incident de sécurité :

1. faire des excuses
2. expliquer les causes
3. annoncer les solutions mises en place

### Gestion des fuites de données

Résumé du talk « Votre base de données avec des informations personnelles sera piratée » de Stéphane Bortzmeyer.

Source : [Paris Web 2018 - 5 octobre matin - Auditorium Pascal - YouTube](https://www.youtube.com/watch?v=VnwzO0uNn6o&t=3850)

#### Avant

À ne pas faire : dire qu'on est sécurisé ou que les données sont anonymisées et qu'il n'arrivera rien.

À faire :

- minimiser la quantité de données
- prendre toutes les mesures possible pour sécuriser le système d'information
+ audit de sécurité

La collecte et le stockage de données entraînent des responsabilités et donc des risques

Exemple : est-il nécessaire de demander la date de naissance si l'on veut juste l'âge ? L'année seule ou une plage de 5 ans ne suffirait-elle pas ?

#### Pendant

À ne pas faire : ignorer les signaux d'alerte, niez les problèmes de sécurité, admettre mais en minimisant

À faire :

- faire circuler l'information en interne
- passer en mode crise : réagit vite, par rapport aux évènements, en court-circuitant les process si besoin
- prendre le temps de réfléchir, éviter faire de nouvelles erreurs
- prévenir les clients (obligation légale) en disant la vérité (ce qu'on sait et ce qu'on ne sait pas)
- en France, prévenr la CNIL

Le mode « crise » et le mode « réflexion » sont contradictoires.
Une solution est d'avoir deux équipes qui travaillent en parallèle : une qui travaille en réaction directe aux évènements et une autre qui travaille sur une échéance plus longue.

#### Le cas des petites boîtes

- il n'y a pas de chiffres officiels, contrairement au cambriolage physiques et aux accidents de voiture
- les petites boîtes n'ont parfois pas conscience d'avoir eu une fuite de données ou d'avoir subit un piratage
- le fait d'être petit ne protège quand il y a une faille systémique
  exemple : des attaques automatisées peuvent compromettre le site Wordpress de toutes petites structures

## PSSI

Exemple de PSSI d'une grande école : [Politique de Sécurité des Systèmes d’Information - PSSI.pdf](https://www.ssi.ens.fr/PSSI.pdf).

## Veille de sécurité

### Best practices

- [Agence nationale de la sécurité des systèmes d'information](https://www.ssi.gouv.fr/)
- [Bonnes pratiques | Agence nationale de la sécurité des systèmes d'information](https://www.ssi.gouv.fr/administration/bonnes-pratiques/)
- [Bonnes pratiques | Agence nationale de la sécurité des systèmes d'information](https://www.ssi.gouv.fr/entreprise/bonnes-pratiques/)
- [Bonnes pratiques | Agence nationale de la sécurité des systèmes d'information](https://www.ssi.gouv.fr/particulier/bonnes-pratiques/)
- [Notes d’information – CERT-FR](https://www.cert.ssi.gouv.fr/information/)

### Général

- [CERT-FR – Centre gouvernemental de veille, d'alerte et de réponse aux attaques informatiques](https://www.cert.ssi.gouv.fr/)
- [National Cyber Awareness System | CISA](https://www.us-cert.gov/ncas)
- [CVE - Common Vulnerabilities and Exposures (CVE)](http://cve.mitre.org/)
- [CVE - Search CVE List](http://cve.mitre.org/cve/search_cve_list.html)
- [CVE - Download CVE List](http://cve.mitre.org/data/downloads/index.html)
- [NVD - Home](https://nvd.nist.gov/)
- [NVD - Search and Statistics](https://nvd.nist.gov/vuln/search)
- [NVD - Categories](https://nvd.nist.gov/vuln/categories)

## Connaissance des attaques

### Généralités

- [OWASP](https://www.owasp.org/index.php/Main_Page)
- [Category:Attack - OWASP](https://www.owasp.org/index.php/Category:Attack)
- [Introduction · OWASP Cheat Sheet Series](https://cheatsheetseries.owasp.org/)

## Outils d'audit en mode blackbox

### Zap (open source)

- [OWASP Zed Attack Proxy Project - OWASP](https://www.owasp.org/index.php/OWASP_Zed_Attack_Proxy_Project)
- [Devenir hacker en 10 minutes - Paul Molin - WEB2DAY 2017 - YouTube](https://www.youtube.com/watch?v=1ijGyVg4VuY)

### Scanner (propriétaire)

- [Scanner](https://scanner.secapps.com/)

### WPScan (plugin Wordpress open source)

- [WPScan a WordPress Vulnerability Scanner](https://wpscan.org/)

## Best practices

- [My First 5 Minutes On A Server; Or, Essential Security for Linux Servers](https://plusbryan.com/my-first-5-minutes-on-a-server-or-essential-security-for-linux-servers)

## Videos

- [Micode - YouTube](https://www.youtube.com/channel/UCYnvxJ-PKiGXo_tYXpWAC-w)
- [How to Choose a Password - Computerphile - YouTube](https://www.youtube.com/watch?v=3NjQ9b3pgIg)
- [Diceware & Passwords - Computerphile - YouTube](https://www.youtube.com/watch?v=Pe_3cFuSw1E)
- [How Secure Shell Works (SSH) - Computerphile - YouTube](https://www.youtube.com/watch?v=ORcvSkgdA58)
- [Cookie Stealing - Computerphile - YouTube](https://www.youtube.com/watch?v=T1QEs3mdJoc)
- [Cracking Websites with Cross Site Scripting - Computerphile - YouTube](https://www.youtube.com/watch?v=L5l9lSnNMxg)
- [Cross Site Request Forgery - Computerphile - YouTube](https://www.youtube.com/watch?v=vRBihr41JTo)
- [Running an SQL Injection Attack - Computerphile - YouTube](https://www.youtube.com/watch?v=ciNHn38EyRc)
- [Hacking Websites with SQL Injection - Computerphile - YouTube](https://www.youtube.com/watch?v=_jKylhJtPmI)
- [Hashing Algorithms and Security - Computerphile - YouTube](https://www.youtube.com/watch?v=b4b8ktEV4Bg)

## Plateforme d'entraînement

[Bienvenue [Root Me : plateforme d'apprentissage dédiée au Hacking et à la Sécurité de l'Information]](https://www.root-me.org/)

## Doc

- [Have I Been Pwned: Check if your email has been compromised in a data breach](https://haveibeenpwned.com/)
- [This is what happens when you reply to spam email | James Veitch - YouTube](https://www.youtube.com/watch?v=_QdPW8JrYzQ)
- [Materiel.net - Votre expert informatique & High Tech - Nouveautés & Promotions !](https://www.materiel.net/)

