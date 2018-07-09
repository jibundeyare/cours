# Symfony 3.4 REST API

## Tester l'API

- POSTMAN : possède une interface graphique
- httpie : fonctionne dans le terminal
- curl : fonctionne dans le terminal (mais moins user friendly que httpie)

## HTTPie

### Installation

Avec une distrib Debian, dans le terminal :

    sudo apt install httpie

## FOS Rest Bundle

### Configuration

Pour définir `json` comme format de données par défaut, dans `app/config/config.yml`, ajouter la configuration suivante :

    # friendsofsymony/fos-rest
    fos_rest:
        format_listener:
            rules:
                - { path: '^/api/', priorities: ['json'], fallback_format: json, prefer_extension: false }

Pour ajouter des contrôleurs, dans `app/config/routing.yml`, ajouter la configuration suivante :

    promotions:
        type:     rest
        resource: AppBundle\Controller\ApiPromotionController

    students:
        type:     rest
        resource: AppBundle\Controller\ApiStudentController


### Routes

Pour vérifier les routes disponibles, dans le terminal, lancer la commande suivante :

    php bin/console debug:router

La commande doit afficher quelque chose de semblable à :

    -------------------------- -------- -------- ------ -----------------------------------
     Name                       Method   Scheme   Host   Path                               
    -------------------------- -------- -------- ------ -----------------------------------
     ...
     get_promotions             GET      ANY      ANY    /api/promotions.{_format}          
     get_promotion              GET      ANY      ANY    /api/promotions/{id}.{_format}     
     post_promotion             POST     ANY      ANY    /api/promotions.{_format}          
     put_promotion              PUT      ANY      ANY    /api/promotions/{id}.{_format}     
     delete_promotion           DELETE   ANY      ANY    /api/promotions/{id}.{_format}     
     get_students               GET      ANY      ANY    /api/students.{_format}            
     get_student                GET      ANY      ANY    /api/students/{id}.{_format}       
     post_student               POST     ANY      ANY    /api/students.{_format}            
     put_student                PUT      ANY      ANY    /api/students/{id}.{_format}       
     delete_student             DELETE   ANY      ANY    /api/students/{id}.{_format}       
     ...
    -------------------------- -------- -------- ------ -----------------------------------

## Tester l'API avec httpie

### Lire des données

Récupérer la liste des promotions :

    http GET http://localhost:8000/api/promotions

Récupérer une promotion :

    http GET http://localhost:8000/api/promotions/1

Récupérer la liste des students :

    http GET http://localhost:8000/api/students

Récupérer un student :

    http GET http://localhost:8000/api/students/1

### Écrire des données

Avec le form suivant, dans `src/AppBundle/Form/PromotionType.php` :

    $builder
        ->add('name')
        ->add('startDate', DateType::class, [
            'widget' => 'single_text',
        ])
        ->add('endDate', DateType::class, [
            'widget' => 'single_text',
        ])
    ;

Créer une promotion :

    http --form POST http://localhost:8000/api/promotions appbundle_promotion[name]="Foo" appbundle_promotion[startDate]="2018-01-01" appbundle_promotion[endDate]="2018-06-30"

Modifier une promotion :

    http --form PUT http://localhost:8000/api/promotions/10 appbundle_promotion[name]="Foo" appbundle_promotion[startDate]="2019-01-01" appbundle_promotion[endDate]="2019-06-30"

Supprimer une promotion :

    http DELETE http://localhost:8000/api/promotions/10

Avec le form suivant dans `src/AppBundle/Form/StudentType.php` :

    $builder
        ->add('firstname')
        ->add('lastname')
        ->add('sex')
        ->add('birthdate', DateType::class, [
            'widget' => 'single_text',
        ])
        ->add('promotion', EntityType::class, [
            'class' => 'AppBundle:Promotion',
            'choice_label' => 'name',
        ])
    ;

Créer un student :

    http --form POST http://localhost:8000/api/students appbundle_student[firstname]="Foo" appbundle_student[lastname]="Bar" appbundle_student[sex]="0" appbundle_student[birthdate]="2000-01-01" appbundle_student[promotion]="1"

Modifier un student :

    http --form PUT http://localhost:8000/api/students/10 appbundle_student[firstname]="Foo" appbundle_student[lastname]="Bar" appbundle_student[sex]="" appbundle_student[birthdate]="2000-01-01" appbundle_student[promotion]="1"

Supprimer un student :

    http --form DELETE http://localhost:8000/api/students/10

Un student avec sa promotion qui contient des students :

    {
        "firstname": "Foo",
        "id": 1,
        "lastname": "Bar",
        "promotion": {
            "id": 1,
            "name": "Promo 1",
            "students": [
                {
                    "firstname": "Lorem",
                    "id": 2,
                    "lastname": "Ipsum"
                }
            ]
        }
    }

Activation de l'option `max_depth_checks` dans `app/config/config.yml` :

    # jms/serializer-bundle
    jms_serializer:
        default_context:
            serialization:
                enable_max_depth_checks: true
            deserialization:
                enable_max_depth_checks: true

Configuration de l'annotation `MaxDepth` dans `src/AppBundle/Entity/Promotion.php` :

    use JMS\Serializer\Annotation\MaxDepth;

    // ...

    /**
     * @var \Doctrine\Common\Collections\Collection
     *
     * @MaxDepth(2)
     * @ORM\OneToMany(targetEntity="AppBundle\Entity\Student", mappedBy="promotion")
     */
    private $students;

Configuration de l'annotation `MaxDepth` dans `src/AppBundle/Entity/Student.php` :

    use JMS\Serializer\Annotation\MaxDepth;

    // ...

    /**
     * @var \AppBundle\Entity\Promotion
     *
     * @MaxDepth(1)
     * @ORM\ManyToOne(targetEntity="AppBundle\Entity\Promotion", inversedBy="students")
     * @ORM\JoinColumns({
     *   @ORM\JoinColumn(name="promotion_id", referencedColumnName="id")
     * })
     */
    private $promotion;

Le même student avec l'annotation `MaxDepth` :

    {
        "firstname": "Foo",
        "id": 1,
        "lastname": "Bar",
        "promotion": {
            "id": 1,
            "name": "Promo 1"
        }
    }

## HTTP authentication with custom user provider (`YamlFileUserProvider`)

- [How to Create a custom User Provider (Symfony 3.4 Docs)](https://symfony.com/doc/3.4/security/custom_provider.html)
- `~/cours/epsi-arras-php-symfony-2017-2018/my_project/app/config/security.yml`
- `~/cours/epsi-arras-php-symfony-2017-2018/my_project/app/config/parameters.yml`
- `~/cours/epsi-arras-php-symfony-2017-2018/my_project/src/AppBundle/Security/User/YamlFileUserProvider.php`

## Docs

### Forms

- [Forms (Symfony 3.4 Docs)](https://symfony.com/doc/3.4/forms.html)
- [Form Types Reference (Symfony 3.4 Docs)](http://symfony.com/doc/3.4/reference/forms/types.html)
- [DateType Field (Symfony 3.4 Docs)](http://symfony.com/doc/3.4/reference/forms/types/date.html)

### Validation

- [Validation Constraints Reference (Symfony 3.4 Docs)](http://symfony.com/doc/3.4/reference/constraints.html)
- [NotBlank (Symfony 3.4 Docs)](http://symfony.com/doc/3.4/reference/constraints/NotBlank.html)

### Doctrine

- [Databases and the Doctrine ORM (Symfony 3.4 Docs)](http://symfony.com/doc/3.4/doctrine.html#persisting-objects-to-the-database)

### HTTP Foundation (Request, Response)

- [Symfony\Component\HttpFoundation\Response | Symfony API](http://api.symfony.com/3.4/Symfony/Component/HttpFoundation/Response.html)

### JMS Serializer Bundle

- [JMSSerializerBundle - JMSSerializerBundle Documentation - Version: 2.x](http://jmsyst.com/bundles/JMSSerializerBundle/2.x)
- [Serializer - serializer Documentation (master)](http://jmsyst.com/libs/serializer)

### FOS Rest Bundle

- [Getting Started With FOSRestBundle (Symfony Bundles Docs)](http://symfony.com/doc/current/bundles/FOSRestBundle/index.html)

### CORS

- [nelmio/NelmioCorsBundle: Adds CORS headers support in your Symfony2 application](https://github.com/nelmio/NelmioCorsBundle)

### Authentication with JWT (JavaScript Web Token)

- [Security (Symfony 3.4 Docs)](https://symfony.com/doc/3.4/security.html)
- [How to Build a Traditional Login Form (Symfony 3.4 Docs)](https://symfony.com/doc/3.4/security/form_login_setup.html)
- [How to Load Security Users from the Database (the Entity Provider) (Symfony 3.4 Docs)](https://symfony.com/doc/3.4/security/entity_provider.html)
- [JSON Web Tokens - jwt.io](https://jwt.io/)
- [lexik/LexikJWTAuthenticationBundle: JWT authentication for your Symfony REST API](https://github.com/lexik/LexikJWTAuthenticationBundle)
- [gfreeau/GfreeauGetJWTBundle](https://github.com/gfreeau/GfreeauGetJWTBundle)

### Tutoriels

- [FOSRestBundle et Symfony à la rescousse - Créez une API REST avec Symfony 3 • Tutoriels • Zeste de Savoir](https://zestedesavoir.com/tutoriels/1280/creez-une-api-rest-avec-symfony-3/developpement-de-lapi-rest/fosrestbundle-et-symfony-a-la-rescousse/)
- [Symfony 3 with ReactJS and Angular](https://codereviewvideos.com/course/symfony-3-with-reactjs-and-angular)
