# Sécurité

## Cours de Philippe P.

Il existe un cours indépendant de Philippe P. (un collègue) qui explique des notions de base et pointe vers des resources intéressantes.

- [Margaret/Securite-audit.md · master · popschool-lens / cours-philippe · GitLab](https://framagit.org/popschool-lens/cours-philippe/blob/master/Margaret/Securite-audit.md)

## Politique de sécurité

- [La Politique de Sécurité des Systèmes d’Information de l’État (PSSIE) | Agence nationale de la sécurité des systèmes d'information](https://www.ssi.gouv.fr/entreprise/reglementation/protection-des-systemes-dinformations/la-politique-de-securite-des-systemes-dinformation-de-letat-pssie/)
- [Paris Web 2018 - 5 octobre matin - Auditorium Pascal - YouTube](https://www.youtube.com/watch?v=VnwzO0uNn6o&feature=youtu.be&t=3836)
- [Top 5 Ways to Handle a Data Breach](https://securitytrails.com/blog/top-5-ways-handle-data-breach)
- [How to Handle a Public Relations Crisis](https://www.businessnewsdaily.com/8935-recover-from-pr-crisis.html)

Réaction à chaud après un incident de sécurité :

1. faire des excuses
2. expliquer les causes
3. annoncer les solutions mises en place

## Veille de sécurité

### Général

- [National Cyber Awareness System | CISA](https://www.us-cert.gov/ncas)
- [CVE - Common Vulnerabilities and Exposures (CVE)](http://cve.mitre.org/)
- [CVE - Search CVE List](http://cve.mitre.org/cve/search_cve_list.html)
- [CVE - Download CVE List](http://cve.mitre.org/data/downloads/index.html)
- [NVD - Home](https://nvd.nist.gov/)
- [NVD - Search and Statistics](https://nvd.nist.gov/vuln/search)
- [NVD - Categories](https://nvd.nist.gov/vuln/categories)

### Debian

- [Debian -- Informations de sécurité](https://www.debian.org/security/)
- [Debian Mailing Lists -- Index for debian-security-announce](https://lists.debian.org/debian-security-announce/)

### Symfony

- [Symfony releases, notifications and release checker](https://symfony.com/releases)
- [Security Advisories posts on the Symfony blog](https://symfony.com/blog/category/security-advisories)
- [What we analyze - SymfonyInsight](https://insight.symfony.com/what-we-analyse)
- [The PHP Security Advisories Database | Articles - Fabien Potencier](http://fabien.potencier.org/the-php-security-advisories-database.html)
- [FriendsOfPHP/security-advisories: A database of PHP security advisories](https://github.com/FriendsOfPHP/security-advisories)

### JS

- [npm | Security advisories](https://www.npmjs.com/advisories)
- [npm-audit | npm Documentation](https://docs.npmjs.com/cli/audit.html)
- [Auditing package dependencies for security vulnerabilities | npm Documentation](https://docs.npmjs.com/auditing-package-dependencies-for-security-vulnerabilities)
- [Identifying Security Vulnerabilities Using NPM Audit](https://itnext.io/identifying-security-vulnerabilities-using-npm-audit-9c590eec9615)
- [10 npm Security Best Practices | Snyk](https://snyk.io/blog/ten-npm-security-best-practices/)

Audit de sécurité :

		$ npm audit
		npm ERR! code EAUDITNOLOCK
		npm ERR! audit Neither npm-shrinkwrap.json nor package-lock.json found: Cannot audit a project without a lockfile
		npm ERR! audit Try creating one first with: npm i --package-lock-only

		npm ERR! A complete log of this run can be found in:
		npm ERR!     /home/user-foo/.npm/_logs/2019-12-30T10_53_33_175Z-debug.log
    $ npm i --package-lock-only
    npm notice created a lockfile as package-lock.json. You should commit this file.
    npm WARN project-foo@1.0.0 No repository field.
    npm WARN project-foo@1.0.0 No license field.

    added 71 packages from 25 contributors and audited 851 packages in 8.091s
    found 52 vulnerabilities (26 low, 12 moderate, 14 high)
      run `npm audit fix` to fix them, or `npm audit` for details
    $ npm audit

                          === npm audit security report ===

    # Run  npm install --save-dev browser-sync@2.26.7  to resolve 35 vulnerabilities
    ┌───────────────┬──────────────────────────────────────────────────────────────┐
    │ Low           │ methodOverride Middleware Reflected Cross-Site Scripting     │
    ├───────────────┼──────────────────────────────────────────────────────────────┤
    │ Package       │ connect                                                      │
    ├───────────────┼──────────────────────────────────────────────────────────────┤
    │ Dependency of │ browser-sync [dev]                                           │
    ├───────────────┼──────────────────────────────────────────────────────────────┤
    │ Path          │ browser-sync > browser-sync-ui > weinre > express > connect  │
    ├───────────────┼──────────────────────────────────────────────────────────────┤
    │ More info     │ https://npmjs.com/advisories/3                               │
    └───────────────┴──────────────────────────────────────────────────────────────┘

    [...]

    # Run  npm install --save-dev chokidar@3.3.1  to resolve 1 vulnerability
    SEMVER WARNING: Recommended action is a potentially breaking change
    ┌───────────────┬──────────────────────────────────────────────────────────────┐
    │ Low           │ Regular Expression Denial of Service                         │
    ├───────────────┼──────────────────────────────────────────────────────────────┤
    │ Package       │ braces                                                       │
    ├───────────────┼──────────────────────────────────────────────────────────────┤
    │ Dependency of │ chokidar [dev]                                               │
    ├───────────────┼──────────────────────────────────────────────────────────────┤
    │ Path          │ chokidar > anymatch > micromatch > braces                    │
    ├───────────────┼──────────────────────────────────────────────────────────────┤
    │ More info     │ https://npmjs.com/advisories/786                             │
    └───────────────┴──────────────────────────────────────────────────────────────┘

    [...]

    # Run  npm update debug --depth 3  to resolve 1 vulnerability
    ┌───────────────┬──────────────────────────────────────────────────────────────┐
    │ Low           │ Regular Expression Denial of Service                         │
    ├───────────────┼──────────────────────────────────────────────────────────────┤
    │ Package       │ debug                                                        │
    ├───────────────┼──────────────────────────────────────────────────────────────┤
    │ Dependency of │ browser-sync [dev]                                           │
    ├───────────────┼──────────────────────────────────────────────────────────────┤
    │ Path          │ browser-sync > resp-modifier > debug                         │
    ├───────────────┼──────────────────────────────────────────────────────────────┤
    │ More info     │ https://npmjs.com/advisories/534                             │
    └───────────────┴──────────────────────────────────────────────────────────────┘

    [...]

    ┌──────────────────────────────────────────────────────────────────────────────┐
    │                                Manual Review                                 │
    │            Some vulnerabilities require your attention to resolve            │
    │                                                                              │
    │         Visit https://go.npm.me/audit-guide for additional guidance          │
    └──────────────────────────────────────────────────────────────────────────────┘
    ┌───────────────┬──────────────────────────────────────────────────────────────┐
    │ High          │ Regular Expression Denial of Service                         │
    ├───────────────┼──────────────────────────────────────────────────────────────┤
    │ Package       │ parsejson                                                    │
    ├───────────────┼──────────────────────────────────────────────────────────────┤
    │ Patched in    │ No patch available                                           │
    ├───────────────┼──────────────────────────────────────────────────────────────┤
    │ Dependency of │ browser-sync [dev]                                           │
    ├───────────────┼──────────────────────────────────────────────────────────────┤
    │ Path          │ browser-sync > socket.io > socket.io-client >                │
    │               │ engine.io-client > parsejson                                 │
    ├───────────────┼──────────────────────────────────────────────────────────────┤
    │ More info     │ https://npmjs.com/advisories/528                             │
    └───────────────┴──────────────────────────────────────────────────────────────┘

    [...]
    found 52 vulnerabilities (26 low, 12 moderate, 14 high) in 851 scanned packages
      run `npm audit fix` to fix 45 of them.
      1 vulnerability requires semver-major dependency updates.
      6 vulnerabilities require manual review. See the full report for details.

Packages obsolètes :

    $ npm outdated
    Package       Current  Wanted  Latest  Location
    bootstrap       3.3.7   3.4.1   4.4.1  project-foo
    browser-sync  2.18.13  2.26.7  2.26.7  project-foo
    chokidar        1.7.0   1.7.0   3.3.1  project-foo
    clean-css       4.1.9   4.2.1   4.2.1  project-foo
    jquery          3.2.1   3.4.1   3.4.1  project-foo
    uglify-js       3.1.2   3.7.3   3.7.3  project-foo

Vérification de la bonne santé de npm avec `npm doctor` :

- Vérifie que le registre npm officile est joignable et affiche la configuration actuelle du registre.
- Vérifie que Git est disponible.
- Passe en revue les versions installées de npm et Node.js.
- Vérifie les permissions de différents dossiers tels que le dossier local et global `node_modules` et celui du dossier utilisé pour le cache des packages.
- Vérifie l'exactitude des sommes de contrôle du cache de modules npm local.

    $ npm doctor
    npm notice PING https://registry.npmjs.org/
    npm WARN verifyCachedFiles Content garbage-collected: 1480 (30762254 bytes)
    npm WARN verifyCachedFiles Cache issues have been fixed
    Check                               Value                        Recommendation
    npm ping                            OK
    npm -v                              v6.9.0                       Use npm v6.13.4
    node -v                             v11.15.0                     Use node v12.14.0
    npm config get registry             https://registry.npmjs.org/
    which git                           /usr/bin/git
    Perms check on cached files         ok
    Perms check on global node_modules  ok
    Perms check on local node_modules   ok
    Verify cache contents               verified 4407 tarballs

### Audit de sécurité multilanguage

- [Snyk | Develop Fast. Stay Secure](https://snyk.io/)
- [Vulnerability DB | Snyk](https://snyk.io/vuln)

La base de données de vulnérabilité de Snyke recense des failles pour les langages, package manager ou OS suivants :

- Composer (PHP)
- Go
- Linux
- Maven (Java)
- npm (JS)
- NuGet (.Net, C#, etc)
- pip (Python)
- RubyGems (Ruby)

## Connaissance des attaques

### Généralités

- [OWASP](https://www.owasp.org/index.php/Main_Page)
- [Category:Attack - OWASP](https://www.owasp.org/index.php/Category:Attack)
- [Introduction · OWASP Cheat Sheet Series](https://cheatsheetseries.owasp.org/)

### Symfony

- [SymfonyLive Paris 2016 - Alain Tiemblo - Sécurité web : pirater pour mieux protéger - YouTube](https://www.youtube.com/watch?v=0RKu12T0r5g)
- [ninsuo/slides: Let's make web security subjects simple and enjoyable.](https://github.com/ninsuo/slides)
- [Sedona au SymfonyLive Paris 2016 – résumé complet des conférences – Sedona Solutions Blog](http://blog.sedona.fr/2016/04/sedona-au-symfonylive-paris-2016/#_Toc449204233)

- [Sécurité et http - Sedona au SymfonyLive Paris 2016 – résumé complet des conférences – Sedona Solutions Blog](http://blog.sedona.fr/2016/04/sedona-au-symfonylive-paris-2016/#_Toc449204248)
- [SymfonyLive Paris 2016 - Romain Neutron - Sécurité et HTTP - YouTube](https://www.youtube.com/watch?v=Vx1_BUohdho)
- [Sécurité & HTTP @ Symfony Live Paris 2016 - Speaker Deck](https://speakerdeck.com/romain/securite-and-http-at-symfony-live-paris-2016)

- [SymfonyLive Paris 2017 - Alain Tiemblo - Sécurité web : et si on continuait à tout casser - YouTube](https://www.youtube.com/watch?v=xzxPM6dnhVI)
- [ninsuo/slides: Let's make web security subjects simple and enjoyable.](https://github.com/ninsuo/slides)
- [Symfony Paris 2017: Les retours de l'équipe Novaway](https://www.novaway.fr/blog/tech/symfony-live-paris-2017-retours#twitter-widget-11)

### Attaque XSS et solutions

- [Cross-site Scripting (XSS) - OWASP](https://www.owasp.org/index.php/Cross-site_Scripting_(XSS))
- [OWASP / Cross-Site Scripting (XSS) - Le blog de Clever Age](https://blog.clever-age.com/fr/2014/02/10/owasp-xss-cross-site-scripting/)
- [X-XSS-Protection - HTTP | MDN](https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/X-XSS-Protection)
- [Content-Security-Policy - HTTP | MDN](https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/Content-Security-Policy)
- [Protéger votre site des attaques XSS avec l'en-tête X-XSS-Protection](https://www.justegeek.fr/proteger-votre-site-des-attaques-xss-avec-len-tete-x-xss-protection/)
- [escape - Documentation - Twig - The flexible, fast, and secure PHP template engine](https://twig.symfony.com/doc/3.x/filters/escape.html)

## Entêtes HTTP

- [HTTP headers - HTTP | MDN](https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers)

## PHP et les mots de passe

- [PHP: password_hash - Manual](https://www.php.net/manual/en/function.password-hash.php)

## Outils d'audit en mode whitebox

- [Symfony Security Monitoring](https://security.symfony.com/)
- [sensiolabs/security-checker: PHP frontend for security.symfony.com](https://github.com/sensiolabs/security-checker)
- [FloeDesignTechnologies/phpcs-security-audit: phpcs-security-audit is a set of PHP_CodeSniffer rules that finds vulnerabilities and weaknesses related to security in PHP code](https://github.com/FloeDesignTechnologies/phpcs-security-audit/)

## Outils d'audit en mode blackbox

### Zap

- [OWASP Zed Attack Proxy Project - OWASP](https://www.owasp.org/index.php/OWASP_Zed_Attack_Proxy_Project)
- [Devenir hacker en 10 minutes - Paul Molin - WEB2DAY 2017 - YouTube](https://www.youtube.com/watch?v=1ijGyVg4VuY)

### Scanner

- [Scanner](https://scanner.secapps.com/)

## Requêtes préparées contre les injections SQL

- [Data Retrieval And Manipulation - Doctrine Database Abstraction Layer (DBAL)](https://www.doctrine-project.org/projects/doctrine-dbal/en/2.10/reference/data-retrieval-and-manipulation.html#data-retrieval-and-manipulation)
- [https://github.com/jibundeyare/src-security](https://github.com/jibundeyare/src-security)

## Best practices

- [My First 5 Minutes On A Server; Or, Essential Security for Linux Servers](https://plusbryan.com/my-first-5-minutes-on-a-server-or-essential-security-for-linux-servers)

## Videos

- [How to Choose a Password - Computerphile - YouTube](https://www.youtube.com/watch?v=3NjQ9b3pgIg)
- [How Secure Shell Works (SSH) - Computerphile - YouTube](https://www.youtube.com/watch?v=ORcvSkgdA58)
- [Cookie Stealing - Computerphile - YouTube](https://www.youtube.com/watch?v=T1QEs3mdJoc)
- [Cracking Websites with Cross Site Scripting - Computerphile - YouTube](https://www.youtube.com/watch?v=L5l9lSnNMxg)
- [Cross Site Request Forgery - Computerphile - YouTube](https://www.youtube.com/watch?v=vRBihr41JTo)
- [Running an SQL Injection Attack - Computerphile - YouTube](https://www.youtube.com/watch?v=ciNHn38EyRc)
- [Hacking Websites with SQL Injection - Computerphile - YouTube](https://www.youtube.com/watch?v=_jKylhJtPmI)
- [Hashing Algorithms and Security - Computerphile - YouTube](https://www.youtube.com/watch?v=b4b8ktEV4Bg)

