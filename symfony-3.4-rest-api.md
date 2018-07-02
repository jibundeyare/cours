# Symfony 3.4 REST API

## Serializer

- [JMSSerializerBundle - JMSSerializerBundle Documentation - Version: master](http://jmsyst.com/bundles/JMSSerializerBundle)

## FOS Rest Bundle

- [Getting Started With FOSRestBundle (Symfony Bundles Docs)](http://symfony.com/doc/current/bundles/FOSRestBundle/index.html)

## CORS

- [nelmio/NelmioCorsBundle: Adds CORS headers support in your Symfony2 application](https://github.com/nelmio/NelmioCorsBundle)

## Authentication with JWT (JavaScript Web Token)

- [Security (Symfony 3.4 Docs)](https://symfony.com/doc/3.4/security.html)
- [How to Build a Traditional Login Form (Symfony 3.4 Docs)](https://symfony.com/doc/3.4/security/form_login_setup.html)
- [How to Load Security Users from the Database (the Entity Provider) (Symfony 3.4 Docs)](https://symfony.com/doc/3.4/security/entity_provider.html)
- [JSON Web Tokens - jwt.io](https://jwt.io/)
- [lexik/LexikJWTAuthenticationBundle: JWT authentication for your Symfony REST API](https://github.com/lexik/LexikJWTAuthenticationBundle)
- [gfreeau/GfreeauGetJWTBundle](https://github.com/gfreeau/GfreeauGetJWTBundle)

## HTTP authentication with custom user provider (`YamlFileUserProvider`)

- [How to Create a custom User Provider (Symfony 3.4 Docs)](https://symfony.com/doc/3.4/security/custom_provider.html)

  - `~/cours/epsi-arras-php-symfony-2017-2018/my_project/app/config/security.yml`
  - `~/cours/epsi-arras-php-symfony-2017-2018/my_project/app/config/parameters.yml`
  - `~/cours/epsi-arras-php-symfony-2017-2018/my_project/src/AppBundle/Security/User/YamlFileUserProvider.php`

## Trouble shooting

### L'erreur `Specified key was too long` (la clé spécifiée est trop longue)

Cette erreur arrive quand on essaye de créer un index ou une contrainte (avec MySQL) sur un champ texte, et qu'on utilise un codage de caractères `utf8mb4` avec le moteur de stockage `InnoDB`.
Les index et les contraintes sont limités à une longueur de `767` octets.
Le champ en question ne fait que `255` caractères de long, mais avec `utf8mb4`, `1 caractère == 4 octets`.
Avec un champ de `255` caractères, nous avons donc un index ou une contrainte qui fait `4 * 255 caractères == 1020 octets`.

    Schema-Tool failed with Error 'An exception occurred while execu
      ting 'CREATE TABLE promotion (id INT AUTO_INCREMENT NOT NULL, na
      me VARCHAR(255) NOT NULL, start_date DATE DEFAULT NULL, end_date
      DATE DEFAULT NULL, UNIQUE INDEX UNIQ_C11D7DD15E237E06 (name), P
      RIMARY KEY(id)) DEFAULT CHARACTER SET utf8mb4 COLLATE utf8mb4_un
      icode_ci ENGINE = InnoDB':

      SQLSTATE[42000]: Syntax error or access violation: 1071 Specifie
      d key was too long; max key length is 767 bytes' while executing
      DDL: CREATE TABLE promotion (id INT AUTO_INCREMENT NOT NULL, na
      me VARCHAR(255) NOT NULL, start_date DATE DEFAULT NULL, end_date
      DATE DEFAULT NULL, UNIQUE INDEX UNIQ_C11D7DD15E237E06 (name), P
      RIMARY KEY(id)) DEFAULT CHARACTER SET utf8mb4 COLLATE utf8mb4_un
      icode_ci ENGINE = InnoDB

Pour corriger ce problème, la solution la plus simple est de réduire la taille du champ (`length`).
La longueur maximum est `767 / 4 == 191.75`, c-à-d, à peu près `190` caractères.

    /**
     * @var string
     *
     * @ORM\Column(name="name", type="string", length=190, nullable=false, unique=true)
     */
    private $name;

- [mysql - #1071 - Specified key was too long; max key length is 767 bytes - Stack Overflow](https://stackoverflow.com/questions/1814532/1071-specified-key-was-too-long-max-key-length-is-767-bytes)

