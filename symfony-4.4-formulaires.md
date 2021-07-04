# Symfony 4.4 - Les formulaires

@WIP

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

Voici comment adapter le formulaire `src/Form/StudentType.php` :

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
+                 // On précise que ce champ permet de gérer la relation avec l'entité SchoolYear
+                 'class' => SchoolYear::class,
+                 // Le label qui est affiché utilisera le nom de la schoolYear
+                 'choice_label' => function(SchoolYear $schoolYear) {
+                     return "{$schoolYear->getName()}";
+                 },
+                 // Les schoolYears doivent être triés par ordre croissant (c-à-d alphabétique) du champ name
+                 'query_builder' => function (EntityRepository $er) {
+                     return $er->createQueryBuilder('s')
+                         ->orderBy('s.name', 'ASC')
+                     ;
+                 },
+             ]);
  // ...
```

Voici comment adapter le formulaire `src/Form/SchoolYearType.php` :

```php
  use App\Entity\SchoolYear;
+ use App\Entity\Student;
+ use Doctrine\ORM\EntityRepository;
+ use Symfony\Bridge\Doctrine\Form\Type\EntityType;
  use Symfony\Component\Form\AbstractType;
  use Symfony\Component\Form\FormBuilderInterface;
  use Symfony\Component\OptionsResolver\OptionsResolver;

  // ...
-             $builder->add('students')
+             $builder->add('students', EntityType::class, [
+                 'class' => Student::class,
+                 'choice_label' => function(User $student) {
+                     return "{$student->getFirstname()} {$student->getLastname()}";
+                 },
+                 // Le champ est à choix multiple
+                 'multiple' => true,
+                 // nécessaire du côté inverse sinon la relation n'est pas enregitrée après mise à jour
+                 'by_reference' => false,
+             ]);
  // ...
```

## Les options des champs

### L'option `class`

Cette option ...

```diff-php
-             $builder->add('foo')
+             $builder->add('foo', EntityType::class, [
+                 'class' => Foo::class,
+             ]);
```

### L'option `choice_label`


### Les options `multiple` et `expanded`


### L'option `query_builder`


### L'option `by_reference`

Cette option permet de dire à Symfony d'enregistrer l'entité dans le BDD en utilisant les données et non pas une référence à l'objet.
Cela fait une grande différence puisque dans le cas d'une entité qui est du côté inverse de la relation, oublier cette option empêche d'enregistrer les données en BDD.

#### L'erreur de l'option `by_referece`

Si par mégarde vous ajouter l'option `'by_reference' => false,` sur un champ côté possédant, vous risquez de voir l'erreur suivante :

```error
Entity of type "Proxies\__CG__\App\Entity\SchoolYear" passed to the choice field must be managed. Maybe you forget to persist it in the entity manager?
```

C'est parce que cette option ne peut être utilisée que du côté inverse de la relation.

## Référénces

- [php - How to avoid "Entities passed to the choice field must be managed. Maybe persist them in the entity manager?" - Stack Overflow](https://stackoverflow.com/questions/35450827/how-to-avoid-entities-passed-to-the-choice-field-must-be-managed-maybe-persist)

