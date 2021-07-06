# Symfony 4.4 - Les formulaires

## La création d'un formulaire

Vous pouvez créer un formulaire :

- totalement « à la main »
- en utilisant la commande `php bin/console make:form`
- ou en utilisant la commande `php bin/console make:crud`

### Création d'un forumulaire avec `make:crud`

Admettons que avez une entité `Student` et une entité `SchoolYear` et que l'entité `Student` a une relation (`ManyToOne`, `ManyToMany`, etc, peu importe) avec l'entité `SchoolYear`.

Vous pouvez générer un CRUD complet avec les commandes suivantes :

```bash
php bin/console make:crud Student
php bin/console make:crud SchoolYear
```

Si votre base de données contient des données school year, un rapide coup d'œil à la page [http://127.0.0.1:8000/student/new](http://127.0.0.1:8000/student/new) devrait afficher l'erreur :

```error
Object of class App\Entity\SchoolYear could not be converted to string
```

Ne vous inquiétez pas, c'est normal.
C'est juste que Symfony a besoin d'informations supplémentaires pour pouvoir afficher correctement le champ `schoolYear` dans le formulaire.

## Les champs de type relations

Pour qu'un champ qui permet de définir une relation entre deux entités s'affiche correctement, il faut préciser qu'il s'agit d'un champ `EntityType`.
Il faut ensuite déclarer les attributs de l'entité qui seront utilisés comme label et si c'est un champ à choix unique ou choix multiple.
Enfin, pour que la relation soit correctement enregistrée même quand on modifie le côté inverse, il faudra préciser dans le formulaire du côté inverse que l'enregistrement ne doit pas se faire par référence.
Si le champ est à choix multiple, on pourra aussi préciser le tri (croissant, décroissant, etc) des labels.

Dans notre exmeple, le côté possédant est l'entité `Student` et l'entité `SchoolYear` est le côté inverse de la relation.

Voici comment adapter le formulaire `src/Form/StudentType.php`, côté possédant de la relation :

```diff-php
  namespace App\Form;

+ use App\Entity\SchoolYear;
  use App\Entity\Student;
+ use Doctrine\ORM\EntityRepository;
+ use Symfony\Bridge\Doctrine\Form\Type\EntityType;
  use Symfony\Component\Form\AbstractType;
  use Symfony\Component\Form\FormBuilderInterface;
  use Symfony\Component\OptionsResolver\OptionsResolver;

  // ...
-             ->add('schoolYear')
+             // Déclaration d'un champ EntityType
+             ->add('schoolYear', EntityType::class, [
+                 // On précise que ce champ permet de gérer la relation avec une entité SchoolYear
+                 'class' => SchoolYear::class,
+                 // Le label qui est affiché utilisera le nom de la school year
+                 'choice_label' => function(SchoolYear $schoolYear) {
+                     return "{$schoolYear->getName()}";
+                 },
+                 // Les school years sont triés par ordre croissant (c-à-d alphabétique) du champ name
+                 'query_builder' => function (EntityRepository $er) {
+                     return $er->createQueryBuilder('s')
+                         ->orderBy('s.name', 'ASC')
+                     ;
+                 },
+             ])
  // ...
```

Voici comment adapter le formulaire `src/Form/SchoolYearType.php`, côté inverse de la relation :

```php
  use App\Entity\SchoolYear;
+ use App\Entity\Student;
+ use Doctrine\ORM\EntityRepository;
+ use Symfony\Bridge\Doctrine\Form\Type\EntityType;
  use Symfony\Component\Form\AbstractType;
  use Symfony\Component\Form\FormBuilderInterface;
  use Symfony\Component\OptionsResolver\OptionsResolver;

  // ...
-             ->add('students')
+             // Déclaration d'un champ EntityType
+             ->add('students', EntityType::class, [
+                 // On précise que ce champ permet de gérer la relation avec des entités Student
+                 'class' => Student::class,
+                 // Le label qui est affiché utilisera le prénom et le nom du student
+                 'choice_label' => function(User $student) {
+                     return "{$student->getFirstname()} {$student->getLastname()}";
+                 },
+                 // Nécessaire du côté inverse sinon la relation n'est pas enregitrée après mise à jour.
+                 'by_reference' => false,
+                 // Les students sont triés par ordre croissant (c-à-d alphabétique) des champs firstname puis lastname.
+                 // Si vous voulez trier par lastname puis firstname, inversez les deux lignes orderBy().
+                 'query_builder' => function (EntityRepository $er) {
+                     return $er->createQueryBuilder('s')
+                         ->orderBy('s.firstname', 'ASC')
+                         ->orderBy('s.lastname', 'ASC')
+                     ;
+                 },
+                 // Le champ est à choix multiple
+                 'multiple' => true,
+             ])
  // ...
```

## La configuration du champ

Je récapitule ici les différentes options qu'on peut ou doit utiliser.

### Le type de champ

Les champs qui permettent de définir une relation avec une ou d'autres entités, sont des champs `EntityType`.

```diff-php
+ use Symfony\Bridge\Doctrine\Form\Type\EntityType;
  use Symfony\Component\Form\AbstractType;
  use Symfony\Component\Form\FormBuilderInterface;
  use Symfony\Component\OptionsResolver\OptionsResolver;

  // ...
          $builder
              // ...
-             ->add('foo')
+             ->add('foo', EntityType::class, [
+             ])
              // ...
          ;
```

### L'option `class`

Cette option définit quel type d'entité le champ doit afficher.

Dans cet exemple, le champ permet de choisir une relation avec une ou des entités de type `Foo` :

```diff-php
+ use App\Entity\Foo;
  use Symfony\Bridge\Doctrine\Form\Type\EntityType;
  use Symfony\Component\Form\AbstractType;
  use Symfony\Component\Form\FormBuilderInterface;
  use Symfony\Component\OptionsResolver\OptionsResolver;

  // ...
              ->add('foo', EntityType::class, [
+                 'class' => Foo::class,
              ])
          ;
```

### L'option `choice_label`

Quand le champ s'affiche, il est représenté par :

- un menu déroulant à choix unique ou des boutons radio
- ou par un menu déroulant à choix multiple ou des cases à cocher

Du texte doit permettre la sélection d'un item.

Justement, cette option permet de choisir quel attribut (ou variable) de l'entité sera utilisé lors de l'affichage du champs.

```diff-php
              ->add('foo', EntityType::class, [
                  'class' => Foo::class,
+                 'choice_label' => function(Foo $foo) {
+                     return "{$foo->getName()} {$foo->getLevel()}";
+                 },
              ])
          ;
```

Il y a une syntaxe simplifiée mais elle ne permet d'utiliser qu'un seul attribut à la fois :

```diff-php
              ->add('foo', EntityType::class, [
                  'class' => Foo::class,
+                 'choice_label' => 'name',
              ])
          ;
```

### L'option `multiple`

Cette option permet de préciser si le champ est à choix multiple.

Par défaut cette option est à `false`.
C'est un problème car la valeur de l'option dépend du type de relation.

Voici les différents cas :

- relation `OneToOne` : on doit avoir `'multiple' => false,`
- relation `ManyToOne` : on doit avoir `'multiple' => false,`
- relation `OneToMany` : on doit avoir `'multiple' => true,`
- relation `ManyToMany` : on doit avoir `'multiple' => true,`

Donc pas la peine de rajouter de `'multiple' => ...` quand on a une relation de type `OneToOne` ou `ManyToOne`.

Exemple avec une relation `ManyToOne` ou `ManyToMany` :

```diff-php
              ->add('foo', EntityType::class, [
                  'class' => Foo::class,
                  'choice_label' => function(Foo $foo) {
                      return "{$foo->getName()} {$foo->getLevel()}";
                  },
+                 'multiple' => true,
              ])
```

### L'option `by_reference`

**Attention : cette option est hyper importante !**

Cette option, qui ne peut être utilisée que du côté inverse de la relation, permet de demander à Symfony d'enregistrer l'entité dans le BDD en utilisant les données et non pas une référence à l'objet.
Cela fait une grande différence puisque dans le cas d'une entité qui est du côté inverse de la relation, oublier cette option empêche d'enregistrer les données en BDD.

Si l'entité `Foo` est l'entité possédante (et que mon formulaire est celui de l'entité du côté inverse de la relation), il faut utilier l'option `by_referece` pour que les données soient correctement enregistrées :

```diff-php
              ->add('foo', EntityType::class, [
                  'class' => Foo::class,
                  'choice_label' => function(Foo $foo) {
                      return "{$foo->getName()} {$foo->getLevel()}";
                  },
                  'multiple' => true,
+                 'by_referece' => false,
              ])
```

#### L'erreur de l'option `by_referece`

Si par mégarde vous ajouter l'option `'by_reference' => false,` sur un champ côté possédant, vous risquez de voir l'erreur suivante :

```error
Entity of type "Proxies\__CG__\App\Entity\SchoolYear" passed to the choice field must be managed. Maybe you forget to persist it in the entity manager?
```

C'est parce que cette option ne peut être utilisée que du côté inverse de la relation.

### L'option `query_builder`

Cette option permet de trier, filtrer ou limiter le jeu de résultat qui servira à alimenter le champ.

L'usage le plus est de trier les éléments par ordre alphabétique :

```diff-php
  use App\Entity\Foo;
+ use Doctrine\ORM\EntityRepository;
  use Symfony\Bridge\Doctrine\Form\Type\EntityType;
  use Symfony\Component\Form\AbstractType;
  use Symfony\Component\Form\FormBuilderInterface;
  use Symfony\Component\OptionsResolver\OptionsResolver;

  // ...
              ->add('foo', EntityType::class, [
                  'class' => Foo::class,
                  'choice_label' => function(Foo $foo) {
                      return "{$foo->getName()} {$foo->getLevel()}";
                  },
                  'multiple' => true,
                  'by_referece' => false,
+                 'query_builder' => function (EntityRepository $er) {
+                     return $er->createQueryBuilder('f')
+                         ->orderBy('s.name', 'ASC')
+                     ;
+                 },
              ])
```

### L'option `expanded`

Cette option contrôle le mode d'affichage :

- `'expanded' => false` affiche un menu déroulant
- `'expanded' => true` affiche des boutons raidio si le champ est à choix unique et des cases à cocher si le champ est à choix multiple

Affichage de cases à cocher :

```diff-php
              ->add('foo', EntityType::class, [
                  'class' => Foo::class,
                  'choice_label' => function(Foo $foo) {
                      return "{$foo->getName()} {$foo->getLevel()}";
                  },
                  'multiple' => true,
                  'by_referece' => false,
                  'query_builder' => function (EntityRepository $er) {
                      return $er->createQueryBuilder('f')
                          ->orderBy('s.name', 'ASC')
                      ;
                  },
+                 'expanded' => true,
              ])
```

À mon avis cette option doit être utilisée avec l'option `attr` qui permet de rajouter des attributs à la balise HTML qui va contenir le champ.
Cela permet d'attribuer une classe à la balise et de limiter la hauteur du champ et d'ajouter un scroll automatique en CSS.

### Les options `attr` et `label_attr`

L'option `attr` permet d'ajouter des attributs à la balise HTML qui va contenir le champ.

Vous pouvez l'utiliser pour rajouter un id ou une classe custom de votre feuille de style :

```diff-php
              ->add('foo', EntityType::class, [
                  'class' => Foo::class,
                  'choice_label' => function(Foo $foo) {
                      return "{$foo->getName()} {$foo->getLevel()}";
                  },
                  'multiple' => true,
                  'by_referece' => false,
                  'query_builder' => function (EntityRepository $er) {
                      return $er->createQueryBuilder('f')
                          ->orderBy('s.name', 'ASC')
                      ;
                  },
                  'expanded' => true,
+                 'attr' => [
+                     'class' => 'checkboxes-with-scroll',
+                 ],
              ])
```

L'option `label_attr` permet d'ajouter des attributs à la balise HTML qui va contenir le label (le nom) du champ.

Je trouve cette option pratique pour masquer avec une classe bootstrap (`d-none`) le titre d'un formulaire imbriqué :

```php
            ->add('bar', BarType::class, [
                'label_attr' => [
                    'class' => 'd-none',
                ],
            ])
```

*Dans tous les cas évitez de rajouter du style CSS inline, c'est cracra.*

## Les formulaires imbriqués

Si vous avez un objet composite, c-à-d un objet composé d'autres objets, vous devez d'abord créer les sous-objets avant de créer l'objet principal.
Les formulaires imbriqués vous permettent de faire ces différentes opérations en une seule en proposant un formulaire qui contient d'autres formulaires. Le cas typique d'usage de cette fonctionnalité est la création simultanée d'un compte user et d'un profil.

Imaginons que nous voulons créer un nouveau student.
Ce student doit avoir un compte utilisateur et un profil student.

Pour éviter de devoir créer d'abord le user puis créer le student et choisir le compte user auquel il se rattache, on peut utiliser un formulaire imbriqué.

Exemple de formulaire de création d'un nouveau compte user dans le fichier `src/Form/UserCreationType.php` :

```php
<?php

namespace App\Form;

use App\Entity\User;
use Symfony\Component\Form\AbstractType;
use Symfony\Component\Form\Extension\Core\Type\PasswordType;
use Symfony\Component\Form\Extension\Core\Type\RepeatedType;
use Symfony\Component\Form\FormBuilderInterface;
use Symfony\Component\OptionsResolver\OptionsResolver;
use Symfony\Component\Validator\Constraints\Length;
use Symfony\Component\Validator\Constraints\NotBlank;

class UserCreationType extends AbstractType
{
    public function buildForm(FormBuilderInterface $builder, array $options)
    {
        $builder
            ->add('email')
            ->add('roles', ChoiceType::class, [
                'choices'  => [
                    'admin' => 'ROLE_ADMIN',
                    'student' => 'ROLE_STUDENT',
                ],
                'multiple' => true,
                'expanded' => true,
            ])
            ->add('plainPassword', RepeatedType::class, [
                'mapped' => false,
                'type' => PasswordType::class,
                'options' => ['attr' => [
                    'class' => 'password-field',
                    'autocomplete' => 'new-password'
                ]],
                'first_options'  => ['label' => 'Mot de passe'],
                'second_options' => ['label' => 'Répétez le mot de passe'],
                'required' => true,
                'constraints' => [
                    new NotBlank([
                        'message' => 'Entrez un mot de passe',
                    ]),
                    new Length([
                        'min' => 6,
                        'minMessage' => 'Le mot de passe doit faire au moins {{ limit }} caractères',
                        // taille maximum autorisée par Symfony pour des raison de sécurité
                        'max' => 4096,
                    ]),
                ],
            ])
        ;
    }

    public function configureOptions(OptionsResolver $resolver)
    {
        $resolver->setDefaults([
            'data_class' => User::class,
        ]);
    }
}
```

Et maintenant voici un exemple de formulaire imbriqué dans `src/Form/StudentType.php` :

```diff-php
  use App\Entity\Student;
+ use App\Entity\User;
+ use App\Form\UserCreationType;
  use Doctrine\ORM\EntityRepository;
  use Symfony\Bridge\Doctrine\Form\Type\EntityType;
  use Symfony\Component\Form\AbstractType;
  use Symfony\Component\Form\FormBuilderInterface;
  use Symfony\Component\OptionsResolver\OptionsResolver;

  // ...
          $builder
+             ->add('user', UserCreationType::class, [
+                 'label_attr' => [
+                     'class' => 'd-none',
+                 ]
+             ])
              ->add('firstname')
              ->add('lastname')
              // ...

```

À l'affichage, vous verrez le formulaire `UserCreationType` et `StudentType` s'afficher dans un seul formulaire.
À l'usage vous n'aurez qu'à remplir un seul formulaire et les entités user et student seront créées en même temps avec la relation.

## Référénces

- [php - How to avoid "Entities passed to the choice field must be managed. Maybe persist them in the entity manager?" - Stack Overflow](https://stackoverflow.com/questions/35450827/how-to-avoid-entities-passed-to-the-choice-field-must-be-managed-maybe-persist)

