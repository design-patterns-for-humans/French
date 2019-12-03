![Design Patterns For Humans](https://cloud.githubusercontent.com/assets/11269635/23065273/1b7e5938-f515-11e6-8dd3-d0d58de6bb9a.png)

***
<p align="center">
🎉 Introduction ultra-simplifiée aux design patterns! 🎉
</p>
<p align="center">
Un sujet qui peut facilment faire peur. Ici je vais essayé de vous les faire intéger ( moi aussi par la même occasion ) en les expliquant de la manière la plus <i>simple</i> possible.
</p>
***

🚀 Introduction
=================

Les design patterns (patrons de conception en Français) sont des solutions à des problèmes récurrents; **des directives pour surmonter certains problèmes**. Ce ne sont pas des classes, packages ou libraries que vous pouvez connnecter à votre application et attendre que la magie fasse effet.Ce sont, plutôt, des directives pour résoudre certains problèmes dans certaines situations.

> Les design patterns sont des solutions récurrentes à des problèmes récurrents; des directives pour résourdre certains problèmes.

Wikipedia les décrit ainsi :

> En informatique, et plus particulièrement en développement logiciel, un patron de conception (plus souvent appelé design pattern) est un arrangement caractéristique de modules, reconnu comme bonne pratique en réponse à un problème de conception d'un logiciel. Il décrit une solution standard, utilisable dans la conception de différents logiciels.

⚠️ Faites Attention
-------------------
- Les design patterns ne sont pas des solutions-miracles à tous vos problèmes.
- N'essayez pas de les forcer; de mauvaises choses vont se produire si vous le faites. Gardez à l'esprit que les design patterns sont des solutions **à** des problèmes et non pas des solutions pour **trouver** des problèmes; alors n'y pensez pas trop.
- S'ils sont utilisés à un endroit correct et de manière correcte, ils peuvent s'avérer salvateurs; dans le cas contraire ils peuvent entraîner un désordre horrible dans un code.

> Notez également que les exemples de code ci-dessous sont en PHP-7, cependant cela ne devrait pas vous arrêter parce que les concepts sont identiques. De plus la **rédaction pour d'autres langages est en cours**.

Types de design patterns
-----------------------

* [Création](#creational-design-patterns)
* [Structurel](#structural-design-patterns)
* [Comportemental](#behavioral-design-patterns)

Design patterns de création
===========================

En clair
> Les patterns de créations sont centrés sur la façon d'instancier un objet ou un groupe d'objets liés.

Wikipedia dit
> Dans le génie logiciel, les design patterns de créations sont des patterns de conception qui traitent des mécanismes de création d'objet en essayant de créer les objets d'une façon appropriée à la situation. La forme de base de création d'objet pourrait entraîner des problèmes de conception ou ajouter de la complexité à la conception. Les design patterns de création résolvent ce problème en contrôlant la création d'objets d'une certaine façon.

 * [Simple Factory](#-simple-factory)
 * [Factory Method](#-factory-method)
 * [Abstract Factory](#-abstract-factory)
 * [Builder](#-builder)
 * [Prototype](#-prototype)
 * [Singleton](#-singleton)

🏠 Simple Factory
--------------
Un exemple dans le monde réel
> Considérez que vous construisez une maison et vous avez besoin de portes. Ce serait un gâchis si chaque fois que vous avez besoin d'une porte, vous mettez vos vêtements de menuisier (charpentier) et commencez à construire une porte dans votre maison. Au lieu de cela vous le faites dans une usine.

En clair
> Simple factory génère simplement une instance pour le client sans exposer toute la logique d'instanciation au client.

D'après Wikipédia
> Dans la programmation orientée objet (POO), un factory est un objet qui créer d'autres objets – formellement un factory est une fonction ou méthode qui retourne des objets d'un prototype ou d'une classe variable à partir d'un appel de méthode, qui est supposé être "new".

**Exemple de programme**

Tout d'abord, nous avons une interface de porte (Door) et l'implémentation
```php
interface Door 
{
    public function getWidth(): float;
    public function getHeight(): float;
}

class WoodenDoor implements Door
{
    protected $width;
    protected $height;

    public function __construct(float $width, float $height)
    {
        $this->width = $width;
        $this->height = $height;
    }

    public function getWidth(): float
    {
        return $this->width;
    }

    public function getHeight(): float
    {
        return $this->height;
    }
}
```
Alors nous avons notre factory de porte (DoorFactory) qui construit la porte et la renvoie
```php
class DoorFactory
{
    public static function makeDoor($width, $height): Door
    {
        return new WoodenDoor($width, $height);
    }
}
```
Et il peut être utilisé comme
```php
$door = DoorFactory::makeDoor(100, 200);
echo 'Width: ' . $door->getWidth();
echo 'Height: ' . $door->getHeight();
```

**Quand l'utiliser?**

Quand la création d'objet n'est pas seulement une affectation et implique une certaine logique, cela a du sens de le mettre dans un factory au lieu de répéter le même code partout.

🏭 Factory Method
--------------

Un exemple dans le monde réel
> Considérez un directeur d'embauche. C'est impossible pour une personne d'interviewer pour chacun des postes. Basée sur l'ouverture du travail, elle doit décider et déléguer les étapes de l'entretien (entrevue) à différentes personnes.

En clair
> Il fournit une façon de déléguer la logique d'instanciation aux classes enfant.

D'après Wikipédia
> Dans la programmation à base de classe, le pattern factory method est un pattern créationnel qui utilise des patterns factory pour résoudre le problème de création d'objets sans devoir spécifier la classe exacte de l'objet qui sera créé. Pour ce faire, on créé des objets par l’appel d’une méthode factory - soit spécifiée dans une interface et implémentée dans une classe enfant, soit l'implémentée dans une classe de base et éventuellement surcharger (redéfinir) par des classes dérivées plutôt que d'appeler un constructeur.

 **Exemple de programme**

Prenons l'exemple de notre directeur d'embauche. Tout d'abord, nous disposons d'une interface d'intervieweur et de certaines implémentations

```php
interface Interviewer
{
    public function askQuestions();
}

class Developer implements Interviewer
{
    public function askQuestions()
    {
        echo 'Asking about design patterns!';
    }
}

class CommunityExecutive implements Interviewer
{
    public function askQuestions()
    {
        echo 'Asking about community building';
    }
}
```

Maintenant, créons notre `HiringManager`

```php
abstract class HiringManager
{

    // Factory method
    abstract public function makeInterviewer(): Interviewer;

    public function takeInterview()
    {
        $interviewer = $this->makeInterviewer();
        $interviewer->askQuestions();
    }
}

```
A présent, n'importe quel enfant peut étendre et fournir l'intervieweur requis
```php
class DevelopmentManager extends HiringManager
{
    public function makeInterviewer(): Interviewer
    {
        return new Developer();
    }
}

class MarketingManager extends HiringManager
{
    public function makeInterviewer(): Interviewer
    {
        return new CommunityExecutive();
    }
}
```
et ensuite il peut être utilisé comme

```php
$devManager = new DevelopmentManager();
$devManager->takeInterview(); // Output: Asking about design patterns

$marketingManager = new MarketingManager();
$marketingManager->takeInterview(); // Output: Asking about community building.
```

**Quand l'utiliser?**

Utile quand il y a du traitement générique dans une classe, mais la sous-classe requise est déterminée dynamiquement lors de l'exécution. Ou, en d'autre termes, lorsque le client ne sait pas de quelle sous-classe exacte il pourrait avoir besoin. 

🔨 Abstract Factory
----------------

Un exemple dans le monde réel
> Étendons notre exemple de porte sur la base de Simple Factory. En fonction de vos besoins, vous pouvez obtenir une porte en bois depuis un magasin de portes en bois, d'une porte en fer depuis un magasin de porte en fer ou d'une porte en PVC d'un magasin approprié. De plus vous pourriez avoir besoin d'un type avec de différentes sortes de spécialités pour placer (fixer) la porte, par exemple un charpentier pour la porte en bois, un soudeur pour la porte en fer, etc. Comme vous pouvez le voir, les portes sont maintenant dépendantes, la porte en bois à besoins d'un charpentier, une porte en fer à besoins d'un soudeur etc.

En clair
> Une usine de Factory (Une usine d'usines); Une usine (Factory) qui regroupe les Factory individuelles mais liées ensemble sans spécifier leurs classes concrètes. En d’autres mots, une Factory qui regroupe plusieurs Factory individuelles mais partageant des liens entre elles.

D'après Wikipédia
> Le pattern Abstract Factory offre un moyen d'encapsuler un groupe de Factory individuelles qui ont un thème commun sans spécifier leurs classes concrètes.

**Exemple de programme**

Traduction de l'exemple de la porte (Door) ci-dessus. Tout d'abord nous avons notre interface `Door` et quelques implémentations pour elle.

```php
interface Door
{
    public function getDescription();
}

class WoodenDoor implements Door
{
    public function getDescription()
    {
        echo 'I am a wooden door';
    }
}

class IronDoor implements Door
{
    public function getDescription()
    {
        echo 'I am an iron door';
    }
}
```
Ensuite, nous avons quelques experts appropriés pour chaque type de porte

```php
interface DoorFittingExpert
{
    public function getDescription();
}

class Welder implements DoorFittingExpert
{
    public function getDescription()
    {
        echo 'I can only fit iron doors';
    }
}

class Carpenter implements DoorFittingExpert
{
    public function getDescription()
    {
        echo 'I can only fit wooden doors';
    }
}
```

Maintenant, nous avons notre Abrastract Factory qui nous permettrait de créer une famille d'objets liés, c'est-à-dire que le Factory de la porte (`WoodenDoorFactory`) en bois créerait une porte en bois et un spécialiste de porte en bois (un Charpentier) et le Factory de la porte en fer (`IronDoorFactory`) créerait une porte en fer et un spécialiste de porte en fer (un Soudeur).
```php
interface DoorFactory
{
    public function makeDoor(): Door;
    public function makeFittingExpert(): DoorFittingExpert;
}

// Factory de la porte en bois qui retourne un Charpentier et une porte en bois
class WoodenDoorFactory implements DoorFactory
{
    public function makeDoor(): Door
    {
        return new WoodenDoor();
    }

    public function makeFittingExpert(): DoorFittingExpert
    {
        return new Carpenter();
    }
}

// Factory de la porte en fer qui retourne une porte en fer et un Soudeur
class IronDoorFactory implements DoorFactory
{
    public function makeDoor(): Door
    {
        return new IronDoor();
    }

    public function makeFittingExpert(): DoorFittingExpert
    {
        return new Welder();
    }
}
```
Et ensuite il peut être utilisé comme 
```php
$woodenFactory = new WoodenDoorFactory();

$door = $woodenFactory->makeDoor();
$expert = $woodenFactory->makeFittingExpert();

$door->getDescription();  // Output: I am a wooden door
$expert->getDescription(); // Output: I can only fit wooden doors

// Idem pour le Factory de porte en fer
$ironFactory = new IronDoorFactory();

$door = $ironFactory->makeDoor();
$expert = $ironFactory->makeFittingExpert();

$door->getDescription();  // Output: I am an iron door
$expert->getDescription(); // Output: I can only fit iron doors
```

Comme vous pouvez le voir, le Factory de la porte (`WoodenDoorFactory`) a encapsulé le charpentier (`carpenter`) et la porte en bois (`wooden door`), aussi le Factory de la porte en fer (`IronDoorFactory`) a encapsulé la porte en fer (`iron door`) et le soudeur (`welder`). Et donc, cela nous a aidés à nous  assurer que pour chacune des portes créées, nous n'obtenions pas un mauvais expert.  

**Quand l'utiliser?**

Quand il y a des dépendances interdépendantes avec une logique de création pas si simples.

👷 Builder
--------------------------------------------
Un exemple dans le monde réel
> Imaginez que vous êtes chez Hardee's (une chaîne de restauration) et que vous passez une commande spécifique, Disons, un "Big Hardee" et ils le remettent sans poser *de questions*; c'est l'exemple d'une simple Factory. Mais il y a des cas où la logique de création pourrait impliquer plus d'étapes. Par exemple, vous voulez un Subway personnalisé, vous avez plusieurs options dans la façon de confectionner votre sandwich, par exemple, quel pain voulez-vous ? quels types de sauces aimeriez-vous ? quel fromage voudriez-vous ? etc. Dans de telles situations, le pattern Builder vient à la rescousse.

En clair
> Permets de créer des versions différentes d’un objet tout en évitant la pollution des constructeurs. Utile quand il pourrait y avoir plusieurs versions différentes d’un objet. Ou lorsqu’il y a beaucoup d’étapes dans la création d’un objet.

D'après Wikipédia
> Le pattern Builder est un pattern de conception de logiciels de création  d’objet avec l’intention de trouver une solution à l’anti-pattern de constructeur télescopique (ou  condenser).
> Télescopique : dont les éléments s'emboîtent les uns dans les autres.

Ayant dit cela, laissez-moi ajouter un anti-pattern de constructeur télescopique. À un moment donné ou à un autre, nous avons vu un constructeur semblable à celui du constructeur ci-dessous:

```php
public function __construct($size, $cheese = true, $pepperoni = true, $tomato = false, $lettuce = true)
{
}
```

Comme vous pouvez le voir; le nombre de paramètres du constructeur peut rapidement devenir incontrôlable et il pourrait être difficile de comprendre la disposition des paramètres. De plus, cette liste de paramètres pourrait continuer à augmenter si vous souhaitez ajouter d'autres options à l'avenir. C'est ce qu'on appelle un constructeur anti-pattern télescopique.

**Exemple de programme**

L'alternative raisonnable est d'utiliser le pattern Builder. Tout d'abord, nous avons notre hamburger que nous voulons créer.

```php
class Burger
{
    protected $size;

    protected $cheese = false;
    protected $pepperoni = false;
    protected $lettuce = false;
    protected $tomato = false;

    public function __construct(BurgerBuilder $builder)
    {
        $this->size = $builder->size;
        $this->cheese = $builder->cheese;
        $this->pepperoni = $builder->pepperoni;
        $this->lettuce = $builder->lettuce;
        $this->tomato = $builder->tomato;
    }
}
```

Et ensuite nous avons le constructeur

```php
class BurgerBuilder
{
    public $size;

    public $cheese = false;
    public $pepperoni = false;
    public $lettuce = false;
    public $tomato = false;

    public function __construct(int $size)
    {
        $this->size = $size;
    }

    public function addPepperoni()
    {
        $this->pepperoni = true;
        return $this;
    }

    public function addLettuce()
    {
        $this->lettuce = true;
        return $this;
    }

    public function addCheese()
    {
        $this->cheese = true;
        return $this;
    }

    public function addTomato()
    {
        $this->tomato = true;
        return $this;
    }

    public function build(): Burger
    {
        return new Burger($this);
    }
}
```
Et alors, il peut être utilisé comme :

```php
$burger = (new BurgerBuilder(14))
                    ->addPepperoni()
                    ->addLettuce()
                    ->addTomato()
                    ->build();
```

**Quand l'utiliser ?**

Quand il peut avoir plusieurs versions d'un objet et pour éviter le constructeur télescopique. La principale différence par rapport au pattern Factory est là celle-ci; le pattern Factory doit être utilisé lorsque la création est un processus en une seule étape tandis que le pattern Builder doit être utilisé lorsque la création est un processus de plusieurs étapes.

🐑 Prototype
------------
Un exemple dans le monde réel
> Vous vous souvenez de Dolly ? Le mouton cloné ! N'entrons pas dans les détails mais nous allons parler de clonage.

En clair
> Crée un objet basé sur un objet existant comme du clonage.

D'après Wikipédia
> Le patron de conception prototype est utilisé lorsque la création d'une instance est complexe ou consommatrice en temps. Plutôt que créer plusieurs instances de la classe, on copie la première instance et on modifie la copie de façon appropriée.

En gros, il permet de créer une copie d'un objet existant en le modifiant à nos besoins, au lieu de devoir passer dans les travers de recréer un nouvel objet en partant de zéro.

**Exemple de programme**

C'est faisable très facilement en PHP en utilisant `clone`

```php
class Sheep
{
    protected $name;
    protected $category;

    public function __construct(string $name, string $category = 'Mountain Sheep')
    {
        $this->name = $name;
        $this->category = $category;
    }

    public function setName(string $name)
    {
        $this->name = $name;
    }

    public function getName()
    {
        return $this->name;
    }

    public function setCategory(string $category)
    {
        $this->category = $category;
    }

    public function getCategory()
    {
        return $this->category;
    }
}
```
Ensuite on peut le cloner comme ci-dessous
```php
$original = new Sheep('Jolly');
echo $original->getName(); // Jolly
echo $original->getCategory(); // Mountain Sheep

// Clone and modify what is required
$cloned = clone $original;
$cloned->setName('Dolly');
echo $cloned->getName(); // Dolly
echo $cloned->getCategory(); // Mountain sheep
```

On peut également utiliser la méthode magique `__clone` pour modifier le comportement du clonage.

**Quand l'utiliser?**

Quand on a besoin d'un objet similaire à un objet existant ou quand la création d'un tout nouvel objet serait trop coûteuse par rapport à un clonage.

💍 Singleton
------------
Un exemple dans le monde réel
> Il ne peut y avoir qu'un seul président d'un pays à la fois. A chaque fois que le devoir l'appelle, c'est le même président qui agit. Le président est ici un singleton.

En clair
> S'assure qu'un seul et unique objet d'une classe ne peut être instancié.

D'après Wikipédia
> En génie logiciel, le singleton est un patron de conception (design pattern) dont l'objectif est de restreindre l'instanciation d'une classe à un seul objet (ou bien à quelques objets seulement). Il est utilisé lorsqu'on a besoin exactement d'un objet pour coordonner des opérations dans un système.

Le pattern Singleton est en fait considéré comme un anti-pattern et il faut éviter d'en abuser. Il n'est pas nécessairement mauvais et a quelques cas d'utilisation valides mais il doit être utilisé avec précaution car il introduit un état global dans l'application. Des changements à l'intérieur du Singleton pourraient avoir des effets secondaires à d'autres endroits et le debug peut s'avérer difficile. Une autre mauvaise conséquence de son utilisation est qu'il rend le code fortement couplé et les tests unitaires s'en retrouvent d'autant plus compliqués.

**Exemple de programme**

Pour créer un Singleton, il faut rendre le constructeur privé, empêcher le clonage, empêcher l'héritage et créer une variable statique qui va accueillir l'instance globale
```php
final class President
{
    private static $instance;

    private function __construct()
    {
        // Hide the constructor
    }

    public static function getInstance(): President
    {
        if (!self::$instance) {
            self::$instance = new self();
        }

        return self::$instance;
    }

    private function __clone()
    {
        // Disable cloning
    }

    private function __wakeup()
    {
        // Disable unserialize
    }
}
```
Il peut ensuite être utilisé comme
```php
$president1 = President::getInstance();
$president2 = President::getInstance();

var_dump($president1 === $president2); // true
```

Design Patterns Structurels
==========================
En clair
> Les Patterns Structurels s'occupent principalement de la composition entre objets ou de la manière dont les objets interagissent entre eux. Une autre explication serait qu'ils répondent à la question : "Comment construire un composoant logiciel ?"

D'après Wikipédia
> En informatique, un patron d'architecture est un patron de conception, c'est-à-dire un modèle de référence qui sert de source d'inspiration lors de la conception de l'architecture d'un système ou d'un logiciel informatique en sous-éléments plus simples.

 * [Adapter](#-adapter)
 * [Bridge](#-bridge)
 * [Composite](#-composite)
 * [Decorator](#-decorator)
 * [Facade](#-facade)
 * [Flyweight](#-flyweight)
 * [Proxy](#-proxy)

🔌 Adapter
-------
Un exemple dans le monde réel
> Imaginons que vous souhaitiez transférer des images d'une carte mémoire à votre ordinateur. Pour les transférer, vous avez besoin d'un adaptateur compatible avec les ports de votre ordinateur de manière à y insérer votre carte mémoire. Dans ce cas, le lecteur de carte est un adaptateur.
> Un autre exemple est le fameux adapteur secteur; un câble électrique doté d'une fiche à trois branches ne peut pas entrer dans une prise électrique à deux trous. On a besoin d'un adaptateur qui rend la fiche compatible avec la prise.
> Encore un autre exemple serait le traducteur qui traduit les mots prononcés par une personne qui s'adresse à une autre

En clair
> Le Pattern Adapter permet d'encapsuler un objet (incompatible en l'état) pour le rendre compatible avec une autre classe.

D'après Wikipédia
> En génie logiciel, adaptateur (ou wrapper) est un patron de conception (design pattern) de type structure (structural). Il permet de convertir l'interface d'une classe en une autre interface que le client attend.

**Exemple de programme**

Soit un jeu dans lequel se trouve un chasseur chassant des lions.

Nous avons d'abord une interface `Lion` que tous les lions doivent implémenter.

```php
interface Lion
{
    public function roar();
}

class AfricanLion implements Lion
{
    public function roar()
    {
    }
}

class AsianLion implements Lion
{
    public function roar()
    {
    }
}
```
Le chasseur chasse n'importe quelle implémentation de l'interface `Lion`.
```php
class Hunter
{
    public function hunt(Lion $lion)
    {
    }
}
```

Maintenant, disons que nous voulions ajouter un `WildDog` dans notre jeu que le chasseur pourrait aussi chasser. Malheureusement nous ne pouvons pas l'ajouter directement car ce chien a une interface différente. Pour le rendre compatible avec notre chasseur, nous devons créer un Adapter compatible.

```php
// This needs to be added to the game
class WildDog
{
    public function bark()
    {
    }
}

// Adapter around wild dog to make it compatible with our game
class WildDogAdapter implements Lion
{
    protected $dog;

    public function __construct(WildDog $dog)
    {
        $this->dog = $dog;
    }

    public function roar()
    {
        $this->dog->bark();
    }
}
```
A présent le `WildDog` peut être utilisé dans notre jeu en utilisant `WildDogAdapter`.

```php
$wildDog = new WildDog();
$wildDogAdapter = new WildDogAdapter($wildDog);

$hunter = new Hunter();
$hunter->hunt($wildDogAdapter);
```

🚡 Bridge
------
Un exemple dans le monde réel
> Considérons que vous ayez un site internet constitué de différentes pages et que vous êtes censé permettre à l'utilisateur de changer le thème du site. Comment feriez-vous ?Créer une copie de chaque page pour chaque thème ou juste créer des thèmes chargés en fonction des préférences de l'utilisateur ? Le Pattern Bridge vous permet de réaliser la seconde solution c-à-d :

![With and without the bridge pattern](https://cloud.githubusercontent.com/assets/11269635/23065293/33b7aea0-f515-11e6-983f-98823c9845ee.png)

En clair
> Le Pattern Bridge consiste à privilégier la composition par rapport à l'héritage. Les détails d'implémentation sont déplacés d'une hiérarchie à un autre objet doté d'une hiérarchie séparée.

D'après Wikipédia
> Le pont est un patron de conception de la famille structuration, qui permet de découpler l'interface d'une classe et son implémentation. La partie concrète (implémentation réelle) peut alors varier, indépendamment de celle abstraite (définition virtuelle), tant qu'elle respecte le contrat de réécriture associé qui les lie (obligation de se conformer aux signatures des fonctions/méthodes, et de leurs fournir un corps physique d'implémentation).

**Exemple de programme**

Traduisons notre exemple de pages Web sous forme d'une hiérarchie de `WebPage` :

```php
interface WebPage
{
    public function __construct(Theme $theme);
    public function getContent();
}

class About implements WebPage
{
    protected $theme;

    public function __construct(Theme $theme)
    {
        $this->theme = $theme;
    }

    public function getContent()
    {
        return "About page in " . $this->theme->getColor();
    }
}

class Careers implements WebPage
{
    protected $theme;

    public function __construct(Theme $theme)
    {
        $this->theme = $theme;
    }

    public function getContent()
    {
        return "Careers page in " . $this->theme->getColor();
    }
}
```
Voici la hiérarchie des thèmes :
```php

interface Theme
{
    public function getColor();
}

class DarkTheme implements Theme
{
    public function getColor()
    {
        return 'Dark Black';
    }
}
class LightTheme implements Theme
{
    public function getColor()
    {
        return 'Off white';
    }
}
class AquaTheme implements Theme
{
    public function getColor()
    {
        return 'Light blue';
    }
}
```
Une fois réunies, on obtient :
```php
$darkTheme = new DarkTheme();

$about = new About($darkTheme);
$careers = new Careers($darkTheme);

echo $about->getContent(); // "About page in Dark Black";
echo $careers->getContent(); // "Careers page in Dark Black";
```

🌿 Composite
-----------------

Un exemple dans le monde réel
> Toutes les entreprises sont composées d'employés. Chaque employé possède les mêmes caractéristiques c-à-d un salaire, des responsabilités, un supérieur hiérarchique, des subordonnés, etc.

En clair
> Le Pattern Composite permet au client de traiter chaque objet de manière uniforme.

D'après Wikipédia
> En génie logiciel, un objet composite est un patron de conception (design pattern) structurel. Ce patron permet de concevoir une structure d'arbre, par exemple un arbre binaire en limitant à deux le nombre de sous-éléments.

**Exemple de programme**

En reprenant l'exemple des employés ci-dessus, nous avons plusieurs types d'employé :

```php
interface Employee
{
    public function __construct(string $name, float $salary);
    public function getName(): string;
    public function setSalary(float $salary);
    public function getSalary(): float;
    public function getRoles(): array;
}

class Developer implements Employee
{
    protected $salary;
    protected $name;

    public function __construct(string $name, float $salary)
    {
        $this->name = $name;
        $this->salary = $salary;
    }

    public function getName(): string
    {
        return $this->name;
    }

    public function setSalary(float $salary)
    {
        $this->salary = $salary;
    }

    public function getSalary(): float
    {
        return $this->salary;
    }

    public function getRoles(): array
    {
        return $this->roles;
    }
}

class Designer implements Employee
{
    protected $salary;
    protected $name;

    public function __construct(string $name, float $salary)
    {
        $this->name = $name;
        $this->salary = $salary;
    }

    public function getName(): string
    {
        return $this->name;
    }

    public function setSalary(float $salary)
    {
        $this->salary = $salary;
    }

    public function getSalary(): float
    {
        return $this->salary;
    }

    public function getRoles(): array
    {
        return $this->roles;
    }
}
```

Ensuite nous avons une entreprise qui consiste simplement en plusieurs employés :

```php
class Organization
{
    protected $employees;

    public function addEmployee(Employee $employee)
    {
        $this->employees[] = $employee;
    }

    public function getNetSalaries(): float
    {
        $netSalary = 0;

        foreach ($this->employees as $employee) {
            $netSalary += $employee->getSalary();
        }

        return $netSalary;
    }
}
```

On peut ensuite l'utiliser comme ceci :

```php
// Prepare the employees
$john = new Developer('John Doe', 12000);
$jane = new Designer('Jane', 10000);

// Add them to organization
$organization = new Organization();
$organization->addEmployee($john);
$organization->addEmployee($jane);

echo "Net salaries: " . $organization->getNetSalaries(); // Net Salaries: 22000
```

☕ Decorator
-------------

Un exemple dans le monde réel

> Imaginez que vous gérez un magasin de service automobile offrant de multiples services. Maintenant, comment calculez-vous le montant à facturer? Vous choisissez un service et continuez à y ajouter les prix pour les services fournis jusqu'à ce que vous obteniez le coût final. Ici, chaque type de service est un décorateur.

En clair
> Le pattern Decorator vous permet de modifier dynamiquement le comportement d'un objet au moment de l'exécution en les enveloppant dans un objet d'une classe de décorateur.

D'après Wikipédia
> Dans la programmation orientée objet, le pattern Décorateur est un pattern de conception  de logiciels qui permet d'ajouter un comportement à un objet individuel, de manière statique ou dynamique, sans affecter le comportement d'autres objets de la même classe. Le pattern Décorateur est souvent utile pour adhérer au principe de responsabilité unique, car il permet de répartir les fonctionnalités entre les classes avec des domaines de préoccupation uniques.

**Exemple de programme**

Prenons l’exemple d’un café. Tout d'abord, nous avons un simple café (SimpleCoffee) qui implémente l'interface café

```php
interface Coffee
{
    public function getCost();
    public function getDescription();
}

class SimpleCoffee implements Coffee
{
    public function getCost()
    {
        return 10;
    }

    public function getDescription()
    {
        return 'Simple coffee';
    }
}
```
Nous voulons rendre le code extensible pour permettre aux options de le modifier si nécessaire. Faisons quelques add-ons (décorateurs)
```php
class MilkCoffee implements Coffee
{
    protected $coffee;

    public function __construct(Coffee $coffee)
    {
        $this->coffee = $coffee;
    }

    public function getCost()
    {
        return $this->coffee->getCost() + 2;
    }

    public function getDescription()
    {
        return $this->coffee->getDescription() . ', milk';
    }
}

class WhipCoffee implements Coffee
{
    protected $coffee;

    public function __construct(Coffee $coffee)
    {
        $this->coffee = $coffee;
    }

    public function getCost()
    {
        return $this->coffee->getCost() + 5;
    }

    public function getDescription()
    {
        return $this->coffee->getDescription() . ', whip';
    }
}

class VanillaCoffee implements Coffee
{
    protected $coffee;

    public function __construct(Coffee $coffee)
    {
        $this->coffee = $coffee;
    }

    public function getCost()
    {
        return $this->coffee->getCost() + 3;
    }

    public function getDescription()
    {
        return $this->coffee->getDescription() . ', vanilla';
    }
}
```

Faisons un café maintenant

```php
$someCoffee = new SimpleCoffee();
echo $someCoffee->getCost(); // 10
echo $someCoffee->getDescription(); // Simple Coffee

$someCoffee = new MilkCoffee($someCoffee);
echo $someCoffee->getCost(); // 12
echo $someCoffee->getDescription(); // Simple Coffee, milk

$someCoffee = new WhipCoffee($someCoffee);
echo $someCoffee->getCost(); // 17
echo $someCoffee->getDescription(); // Simple Coffee, milk, whip

$someCoffee = new VanillaCoffee($someCoffee);
echo $someCoffee->getCost(); // 20
echo $someCoffee->getDescription(); // Simple Coffee, milk, whip, vanilla
```

📦 Facade
----------------

Un exemple dans le monde réel
> Comment démarrer l’ordinateur ? "Appuyer sur le bouton d’alimentation" dites-vous! C’est ce que vous croyez parce que vous utilisez une interface simple que l’ordinateur fournit à l’extérieur, intérieurement, il doit faire beaucoup de choses pour que cela se produise. Cette interface simple au sous-système complexe est une façade.

En clair
> Le pattern Façade fournit une interface simplifiée à un sous-système complexe. 

D'après Wikipédia
> Une façade est un objet qui fournit une interface simplifiée à un grand nombre de codes, comme une bibliothèque de classe.

**Exemple de programme**

Prenons l’exemple de l’ordinateur ci-dessus. Nous avons ici la classe ordinateur (Computer)

```php
class Computer
{
    public function getElectricShock()
    {
        echo "Ouch!";
    }

    public function makeSound()
    {
        echo "Beep beep!";
    }

    public function showLoadingScreen()
    {
        echo "Loading..";
    }

    public function bam()
    {
        echo "Ready to be used!";
    }

    public function closeEverything()
    {
        echo "Bup bup bup buzzzz!";
    }

    public function sooth()
    {
        echo "Zzzzz";
    }

    public function pullCurrent()
    {
        echo "Haaah!";
    }
}
```
Nous avons ici la façade
```php
class ComputerFacade
{
    protected $computer;

    public function __construct(Computer $computer)
    {
        $this->computer = $computer;
    }

    public function turnOn()
    {
        $this->computer->getElectricShock();
        $this->computer->makeSound();
        $this->computer->showLoadingScreen();
        $this->computer->bam();
    }

    public function turnOff()
    {
        $this->computer->closeEverything();
        $this->computer->pullCurrent();
        $this->computer->sooth();
    }
}
```
Maintenant, utiliser la façade.
```php
$computer = new ComputerFacade(new Computer());
$computer->turnOn(); // Ouch! Beep beep! Loading.. Ready to be used!
$computer->turnOff(); // Bup bup buzzz! Haah! Zzzzz
```

🍃 Flyweight
---------

Un exemple dans le monde réel
> Did you ever have fresh tea from some stall? They often make more than one cup that you demanded and save the rest for any other customer so to save the resources e.g. gas etc. Flyweight pattern is all about that i.e. sharing.

En clair
> It is used to minimize memory usage or computational expenses by sharing as much as possible with similar objects.

D'après Wikipédia
> In computer programming, flyweight is a software design pattern. A flyweight is an object that minimizes memory use by sharing as much data as possible with other similar objects; it is a way to use objects in large numbers when a simple repeated representation would use an unacceptable amount of memory.

**Exemple de programme**

Translating our tea example from above. First of all we have tea types and tea maker

```php
// Anything that will be cached is flyweight.
// Types of tea here will be flyweights.
class KarakTea
{
}

// Acts as a factory and saves the tea
class TeaMaker
{
    protected $availableTea = [];

    public function make($preference)
    {
        if (empty($this->availableTea[$preference])) {
            $this->availableTea[$preference] = new KarakTea();
        }

        return $this->availableTea[$preference];
    }
}
```

Then we have the `TeaShop` which takes orders and serves them

```php
class TeaShop
{
    protected $orders;
    protected $teaMaker;

    public function __construct(TeaMaker $teaMaker)
    {
        $this->teaMaker = $teaMaker;
    }

    public function takeOrder(string $teaType, int $table)
    {
        $this->orders[$table] = $this->teaMaker->make($teaType);
    }

    public function serve()
    {
        foreach ($this->orders as $table => $tea) {
            echo "Serving tea to table# " . $table;
        }
    }
}
```
And it can be used as below

```php
$teaMaker = new TeaMaker();
$shop = new TeaShop($teaMaker);

$shop->takeOrder('less sugar', 1);
$shop->takeOrder('more milk', 2);
$shop->takeOrder('without sugar', 5);

$shop->serve();
// Serving tea to table# 1
// Serving tea to table# 2
// Serving tea to table# 5
```

🎱 Proxy
-------------------
Un exemple dans le monde réel
> Have you ever used an access card to go through a door? There are multiple options to open that door i.e. it can be opened either using access card or by pressing a button that bypasses the security. The door's main functionality is to open but there is a proxy added on top of it to add some functionality. Let me better explain it using the code example below.

En clair
> Using the proxy pattern, a class represents the functionality of another class.

D'après Wikipédia
> A proxy, in its most general form, is a class functioning as an interface to something else. A proxy is a wrapper or agent object that is being called by the client to access the real serving object behind the scenes. Use of the proxy can simply be forwarding to the real object, or can provide additional logic. In the proxy extra functionality can be provided, for example caching when operations on the real object are resource intensive, or checking preconditions before operations on the real object are invoked.

**Exemple de programme**

Taking our security door example from above. Firstly we have the door interface and an implementation of door

```php
interface Door
{
    public function open();
    public function close();
}

class LabDoor implements Door
{
    public function open()
    {
        echo "Opening lab door";
    }

    public function close()
    {
        echo "Closing the lab door";
    }
}
```
Then we have a proxy to secure any doors that we want
```php
class Security
{
    protected $door;

    public function __construct(Door $door)
    {
        $this->door = $door;
    }

    public function open($password)
    {
        if ($this->authenticate($password)) {
            $this->door->open();
        } else {
            echo "Big no! It ain't possible.";
        }
    }

    public function authenticate($password)
    {
        return $password === '$ecr@t';
    }

    public function close()
    {
        $this->door->close();
    }
}
```
And here is how it can be used
```php
$door = new Security(new LabDoor());
$door->open('invalid'); // Big no! It ain't possible.

$door->open('$ecr@t'); // Opening lab door
$door->close(); // Closing lab door
```
Yet another example would be some sort of data-mapper implementation. For example, I recently made an ODM (Object Data Mapper) for MongoDB using this pattern where I wrote a proxy around mongo classes while utilizing the magic method `__call()`. All the method calls were proxied to the original mongo class and result retrieved was returned as it is but in case of `find` or `findOne` data was mapped to the required class objects and the object was returned instead of `Cursor`.

Behavioral Design Patterns
==========================

En clair
> It is concerned with assignment of responsibilities between the objects. What makes them different from structural patterns is they don't just specify the structure but also outline the patterns for message passing/communication between them. Or in other words, they assist in answering "How to run a behavior in software component?"

D'après Wikipédia
> In software engineering, behavioral design patterns are design patterns that identify common communication patterns between objects and realize these patterns. By doing so, these patterns increase flexibility in carrying out this communication.

* [Chain of Responsibility](#-chain-of-responsibility)
* [Command](#-command)
* [Iterator](#-iterator)
* [Mediator](#-mediator)
* [Memento](#-memento)
* [Observer](#-observer)
* [Visitor](#-visitor)
* [Strategy](#-strategy)
* [State](#-state)
* [Template Method](#-template-method)

🔗 Chain of Responsibility
-----------------------

Un exemple dans le monde réel
> For example, you have three payment methods (`A`, `B` and `C`) setup in your account; each having a different amount in it. `A` has 100 USD, `B` has 300 USD and `C` having 1000 USD and the preference for payments is chosen as `A` then `B` then `C`. You try to purchase something that is worth 210 USD. Using Chain of Responsibility, first of all account `A` will be checked if it can make the purchase, if yes purchase will be made and the chain will be broken. If not, request will move forward to account `B` checking for amount if yes chain will be broken otherwise the request will keep forwarding till it finds the suitable handler. Here `A`, `B` and `C` are links of the chain and the whole phenomenon is Chain of Responsibility.

En clair
> It helps building a chain of objects. Request enters from one end and keeps going from object to object till it finds the suitable handler.

D'après Wikipédia
> In object-oriented design, the chain-of-responsibility pattern is a design pattern consisting of a source of command objects and a series of processing objects. Each processing object contains logic that defines the types of command objects that it can handle; the rest are passed to the next processing object in the chain.

**Exemple de programme**

Translating our account example above. First of all we have a base account having the logic for chaining the accounts together and some accounts

```php
abstract class Account
{
    protected $successor;
    protected $balance;

    public function setNext(Account $account)
    {
        $this->successor = $account;
    }

    public function pay(float $amountToPay)
    {
        if ($this->canPay($amountToPay)) {
            echo sprintf('Paid %s using %s' . PHP_EOL, $amountToPay, get_called_class());
        } elseif ($this->successor) {
            echo sprintf('Cannot pay using %s. Proceeding ..' . PHP_EOL, get_called_class());
            $this->successor->pay($amountToPay);
        } else {
            throw new Exception('None of the accounts have enough balance');
        }
    }

    public function canPay($amount): bool
    {
        return $this->balance >= $amount;
    }
}

class Bank extends Account
{
    protected $balance;

    public function __construct(float $balance)
    {
        $this->balance = $balance;
    }
}

class Paypal extends Account
{
    protected $balance;

    public function __construct(float $balance)
    {
        $this->balance = $balance;
    }
}

class Bitcoin extends Account
{
    protected $balance;

    public function __construct(float $balance)
    {
        $this->balance = $balance;
    }
}
```

Now let's prepare the chain using the links defined above (i.e. Bank, Paypal, Bitcoin)

```php
// Let's prepare a chain like below
//      $bank->$paypal->$bitcoin
//
// First priority bank
//      If bank can't pay then paypal
//      If paypal can't pay then bit coin

$bank = new Bank(100);          // Bank with balance 100
$paypal = new Paypal(200);      // Paypal with balance 200
$bitcoin = new Bitcoin(300);    // Bitcoin with balance 300

$bank->setNext($paypal);
$paypal->setNext($bitcoin);

// Let's try to pay using the first priority i.e. bank
$bank->pay(259);

// Output will be
// ==============
// Cannot pay using bank. Proceeding ..
// Cannot pay using paypal. Proceeding ..:
// Paid 259 using Bitcoin!
```

👮 Command
-------

Un exemple dans le monde réel
> A generic example would be you ordering a food at restaurant. You (i.e. `Client`) ask the waiter (i.e. `Invoker`) to bring some food (i.e. `Command`) and waiter simply forwards the request to Chef (i.e. `Receiver`) who has the knowledge of what and how to cook.
> Another example would be you (i.e. `Client`) switching on (i.e. `Command`) the television (i.e. `Receiver`) using a remote control (`Invoker`).

En clair
> Allows you to encapsulate actions in objects. The key idea behind this pattern is to provide the means to decouple client from receiver.

D'après Wikipédia
> In object-oriented programming, the command pattern is a behavioral design pattern in which an object is used to encapsulate all information needed to perform an action or trigger an event at a later time. This information includes the method name, the object that owns the method and values for the method parameters.

**Exemple de programme**

First of all we have the receiver that has the implementation of every action that could be performed
```php
// Receiver
class Bulb
{
    public function turnOn()
    {
        echo "Bulb has been lit";
    }

    public function turnOff()
    {
        echo "Darkness!";
    }
}
```
then we have an interface that each of the commands are going to implement and then we have a set of commands
```php
interface Command
{
    public function execute();
    public function undo();
    public function redo();
}

// Command
class TurnOn implements Command
{
    protected $bulb;

    public function __construct(Bulb $bulb)
    {
        $this->bulb = $bulb;
    }

    public function execute()
    {
        $this->bulb->turnOn();
    }

    public function undo()
    {
        $this->bulb->turnOff();
    }

    public function redo()
    {
        $this->execute();
    }
}

class TurnOff implements Command
{
    protected $bulb;

    public function __construct(Bulb $bulb)
    {
        $this->bulb = $bulb;
    }

    public function execute()
    {
        $this->bulb->turnOff();
    }

    public function undo()
    {
        $this->bulb->turnOn();
    }

    public function redo()
    {
        $this->execute();
    }
}
```
Then we have an `Invoker` with whom the client will interact to process any commands
```php
// Invoker
class RemoteControl
{
    public function submit(Command $command)
    {
        $command->execute();
    }
}
```
Finally let's see how we can use it in our client
```php
$bulb = new Bulb();

$turnOn = new TurnOn($bulb);
$turnOff = new TurnOff($bulb);

$remote = new RemoteControl();
$remote->submit($turnOn); // Bulb has been lit!
$remote->submit($turnOff); // Darkness!
```

Command pattern can also be used to implement a transaction based system. Where you keep maintaining the history of commands as soon as you execute them. If the final command is successfully executed, all good otherwise just iterate through the history and keep executing the `undo` on all the executed commands.

➿ Iterator
--------

Un exemple dans le monde réel
> An old radio set will be a good example of iterator, where user could start at some channel and then use next or previous buttons to go through the respective channels. Or take an example of MP3 player or a TV set where you could press the next and previous buttons to go through the consecutive channels or in other words they all provide an interface to iterate through the respective channels, songs or radio stations.  

En clair
> It presents a way to access the elements of an object without exposing the underlying presentation.

D'après Wikipédia
> In object-oriented programming, the iterator pattern is a design pattern in which an iterator is used to traverse a container and access the container's elements. The iterator pattern decouples algorithms from containers; in some cases, algorithms are necessarily container-specific and thus cannot be decoupled.

**Exemple de programme**

In PHP it is quite easy to implement using SPL (Standard PHP Library). Translating our radio stations example from above. First of all we have `RadioStation`

```php
class RadioStation
{
    protected $frequency;

    public function __construct(float $frequency)
    {
        $this->frequency = $frequency;
    }

    public function getFrequency(): float
    {
        return $this->frequency;
    }
}
```
Then we have our iterator

```php
use Countable;
use Iterator;

class StationList implements Countable, Iterator
{
    /** @var RadioStation[] $stations */
    protected $stations = [];

    /** @var int $counter */
    protected $counter;

    public function addStation(RadioStation $station)
    {
        $this->stations[] = $station;
    }

    public function removeStation(RadioStation $toRemove)
    {
        $toRemoveFrequency = $toRemove->getFrequency();
        $this->stations = array_filter($this->stations, function (RadioStation $station) use ($toRemoveFrequency) {
            return $station->getFrequency() !== $toRemoveFrequency;
        });
    }

    public function count(): int
    {
        return count($this->stations);
    }

    public function current(): RadioStation
    {
        return $this->stations[$this->counter];
    }

    public function key()
    {
        return $this->counter;
    }

    public function next()
    {
        $this->counter++;
    }

    public function rewind()
    {
        $this->counter = 0;
    }

    public function valid(): bool
    {
        return isset($this->stations[$this->counter]);
    }
}
```
And then it can be used as
```php
$stationList = new StationList();

$stationList->addStation(new RadioStation(89));
$stationList->addStation(new RadioStation(101));
$stationList->addStation(new RadioStation(102));
$stationList->addStation(new RadioStation(103.2));

foreach($stationList as $station) {
    echo $station->getFrequency() . PHP_EOL;
}

$stationList->removeStation(new RadioStation(89)); // Will remove station 89
```

👽 Mediator
========

Un exemple dans le monde réel
> A general example would be when you talk to someone on your mobile phone, there is a network provider sitting between you and them and your conversation goes through it instead of being directly sent. In this case network provider is mediator.

En clair
> Mediator pattern adds a third party object (called mediator) to control the interaction between two objects (called colleagues). It helps reduce the coupling between the classes communicating with each other. Because now they don't need to have the knowledge of each other's implementation.

D'après Wikipédia
> In software engineering, the mediator pattern defines an object that encapsulates how a set of objects interact. This pattern is considered to be a behavioral pattern due to the way it can alter the program's running behavior.

**Exemple de programme**

Here is the simplest example of a chat room (i.e. mediator) with users (i.e. colleagues) sending messages to each other.

First of all, we have the mediator i.e. the chat room

```php
interface ChatRoomMediator 
{
    public function showMessage(User $user, string $message);
}

// Mediator
class ChatRoom implements ChatRoomMediator
{
    public function showMessage(User $user, string $message)
    {
        $time = date('M d, y H:i');
        $sender = $user->getName();

        echo $time . '[' . $sender . ']:' . $message;
    }
}
```

Then we have our users i.e. colleagues
```php
class User {
    protected $name;
    protected $chatMediator;

    public function __construct(string $name, ChatRoomMediator $chatMediator) {
        $this->name = $name;
        $this->chatMediator = $chatMediator;
    }

    public function getName() {
        return $this->name;
    }

    public function send($message) {
        $this->chatMediator->showMessage($this, $message);
    }
}
```
And the usage
```php
$mediator = new ChatRoom();

$john = new User('John Doe', $mediator);
$jane = new User('Jane Doe', $mediator);

$john->send('Hi there!');
$jane->send('Hey!');

// Output will be
// Feb 14, 10:58 [John]: Hi there!
// Feb 14, 10:58 [Jane]: Hey!
```

💾 Memento
-------
Un exemple dans le monde réel
> Take the example of calculator (i.e. originator), where whenever you perform some calculation the last calculation is saved in memory (i.e. memento) so that you can get back to it and maybe get it restored using some action buttons (i.e. caretaker).

En clair
> Memento pattern is about capturing and storing the current state of an object in a manner that it can be restored later on in a smooth manner.

D'après Wikipédia
> The memento pattern is a software design pattern that provides the ability to restore an object to its previous state (undo via rollback).

Usually useful when you need to provide some sort of undo functionality.

**Exemple de programme**

Lets take an example of text editor which keeps saving the state from time to time and that you can restore if you want.

First of all we have our memento object that will be able to hold the editor state

```php
class EditorMemento
{
    protected $content;

    public function __construct(string $content)
    {
        $this->content = $content;
    }

    public function getContent()
    {
        return $this->content;
    }
}
```

Then we have our editor i.e. originator that is going to use memento object

```php
class Editor
{
    protected $content = '';

    public function type(string $words)
    {
        $this->content = $this->content . ' ' . $words;
    }

    public function getContent()
    {
        return $this->content;
    }

    public function save()
    {
        return new EditorMemento($this->content);
    }

    public function restore(EditorMemento $memento)
    {
        $this->content = $memento->getContent();
    }
}
```

And then it can be used as

```php
$editor = new Editor();

// Type some stuff
$editor->type('This is the first sentence.');
$editor->type('This is second.');

// Save the state to restore to : This is the first sentence. This is second.
$saved = $editor->save();

// Type some more
$editor->type('And this is third.');

// Output: Content before Saving
echo $editor->getContent(); // This is the first sentence. This is second. And this is third.

// Restoring to last saved state
$editor->restore($saved);

$editor->getContent(); // This is the first sentence. This is second.
```

😎 Observer
--------
Un exemple dans le monde réel
> A good example would be the job seekers where they subscribe to some job posting site and they are notified whenever there is a matching job opportunity.   

En clair
> Defines a dependency between objects so that whenever an object changes its state, all its dependents are notified.

D'après Wikipédia
> The observer pattern is a software design pattern in which an object, called the subject, maintains a list of its dependents, called observers, and notifies them automatically of any state changes, usually by calling one of their methods.

**Exemple de programme**

Translating our example from above. First of all we have job seekers that need to be notified for a job posting
```php
class JobPost
{
    protected $title;

    public function __construct(string $title)
    {
        $this->title = $title;
    }

    public function getTitle()
    {
        return $this->title;
    }
}

class JobSeeker implements Observer
{
    protected $name;

    public function __construct(string $name)
    {
        $this->name = $name;
    }

    public function onJobPosted(JobPost $job)
    {
        // Do something with the job posting
        echo 'Hi ' . $this->name . '! New job posted: '. $job->getTitle();
    }
}
```
Then we have our job postings to which the job seekers will subscribe
```php
class JobPostings implements Observable
{
    protected $observers = [];

    protected function notify(JobPost $jobPosting)
    {
        foreach ($this->observers as $observer) {
            $observer->onJobPosted($jobPosting);
        }
    }

    public function attach(Observer $observer)
    {
        $this->observers[] = $observer;
    }

    public function addJob(JobPost $jobPosting)
    {
        $this->notify($jobPosting);
    }
}
```
Then it can be used as
```php
// Create subscribers
$johnDoe = new JobSeeker('John Doe');
$janeDoe = new JobSeeker('Jane Doe');

// Create publisher and attach subscribers
$jobPostings = new JobPostings();
$jobPostings->attach($johnDoe);
$jobPostings->attach($janeDoe);

// Add a new job and see if subscribers get notified
$jobPostings->addJob(new JobPost('Software Engineer'));

// Output
// Hi John Doe! New job posted: Software Engineer
// Hi Jane Doe! New job posted: Software Engineer
```

🏃 Visitor
-------
Un exemple dans le monde réel
> Consider someone visiting Dubai. They just need a way (i.e. visa) to enter Dubai. After arrival, they can come and visit any place in Dubai on their own without having to ask for permission or to do some leg work in order to visit any place here; just let them know of a place and they can visit it. Visitor pattern lets you do just that, it helps you add places to visit so that they can visit as much as they can without having to do any legwork.

En clair
> Visitor pattern lets you add further operations to objects without having to modify them.

D'après Wikipédia
> In object-oriented programming and software engineering, the visitor design pattern is a way of separating an algorithm from an object structure on which it operates. A practical result of this separation is the ability to add new operations to existing object structures without modifying those structures. It is one way to follow the open/closed principle.

**Exemple de programme**

Let's take an example of a zoo simulation where we have several different kinds of animals and we have to make them Sound. Let's translate this using visitor pattern

```php
// Visitee
interface Animal
{
    public function accept(AnimalOperation $operation);
}

// Visitor
interface AnimalOperation
{
    public function visitMonkey(Monkey $monkey);
    public function visitLion(Lion $lion);
    public function visitDolphin(Dolphin $dolphin);
}
```
Then we have our implementations for the animals
```php
class Monkey implements Animal
{
    public function shout()
    {
        echo 'Ooh oo aa aa!';
    }

    public function accept(AnimalOperation $operation)
    {
        $operation->visitMonkey($this);
    }
}

class Lion implements Animal
{
    public function roar()
    {
        echo 'Roaaar!';
    }

    public function accept(AnimalOperation $operation)
    {
        $operation->visitLion($this);
    }
}

class Dolphin implements Animal
{
    public function speak()
    {
        echo 'Tuut tuttu tuutt!';
    }

    public function accept(AnimalOperation $operation)
    {
        $operation->visitDolphin($this);
    }
}
```
Let's implement our visitor
```php
class Speak implements AnimalOperation
{
    public function visitMonkey(Monkey $monkey)
    {
        $monkey->shout();
    }

    public function visitLion(Lion $lion)
    {
        $lion->roar();
    }

    public function visitDolphin(Dolphin $dolphin)
    {
        $dolphin->speak();
    }
}
```

And then it can be used as
```php
$monkey = new Monkey();
$lion = new Lion();
$dolphin = new Dolphin();

$speak = new Speak();

$monkey->accept($speak);    // Ooh oo aa aa!    
$lion->accept($speak);      // Roaaar!
$dolphin->accept($speak);   // Tuut tutt tuutt!
```
We could have done this simply by having a inheritance hierarchy for the animals but then we would have to modify the animals whenever we would have to add new actions to animals. But now we will not have to change them. For example, let's say we are asked to add the jump behavior to the animals, we can simply add that by creating a new visitor i.e.

```php
class Jump implements AnimalOperation
{
    public function visitMonkey(Monkey $monkey)
    {
        echo 'Jumped 20 feet high! on to the tree!';
    }

    public function visitLion(Lion $lion)
    {
        echo 'Jumped 7 feet! Back on the ground!';
    }

    public function visitDolphin(Dolphin $dolphin)
    {
        echo 'Walked on water a little and disappeared';
    }
}
```
And for the usage
```php
$jump = new Jump();

$monkey->accept($speak);   // Ooh oo aa aa!
$monkey->accept($jump);    // Jumped 20 feet high! on to the tree!

$lion->accept($speak);     // Roaaar!
$lion->accept($jump);      // Jumped 7 feet! Back on the ground!

$dolphin->accept($speak);  // Tuut tutt tuutt!
$dolphin->accept($jump);   // Walked on water a little and disappeared
```

💡 Strategy
--------

Un exemple dans le monde réel
> Consider the example of sorting, we implemented bubble sort but the data started to grow and bubble sort started getting very slow. In order to tackle this we implemented Quick sort. But now although the quick sort algorithm was doing better for large datasets, it was very slow for smaller datasets. In order to handle this we implemented a strategy where for small datasets, bubble sort will be used and for larger, quick sort.

En clair
> Strategy pattern allows you to switch the algorithm or strategy based upon the situation.

D'après Wikipédia
> In computer programming, the strategy pattern (also known as the policy pattern) is a behavioural software design pattern that enables an algorithm's behavior to be selected at runtime.

**Exemple de programme**

Translating our example from above. First of all we have our strategy interface and different strategy implementations

```php
interface SortStrategy
{
    public function sort(array $dataset): array;
}

class BubbleSortStrategy implements SortStrategy
{
    public function sort(array $dataset): array
    {
        echo "Sorting using bubble sort";

        // Do sorting
        return $dataset;
    }
}

class QuickSortStrategy implements SortStrategy
{
    public function sort(array $dataset): array
    {
        echo "Sorting using quick sort";

        // Do sorting
        return $dataset;
    }
}
```

And then we have our client that is going to use any strategy
```php
class Sorter
{
    protected $sorter;

    public function __construct(SortStrategy $sorter)
    {
        $this->sorter = $sorter;
    }

    public function sort(array $dataset): array
    {
        return $this->sorter->sort($dataset);
    }
}
```
And it can be used as
```php
$dataset = [1, 5, 4, 3, 2, 8];

$sorter = new Sorter(new BubbleSortStrategy());
$sorter->sort($dataset); // Output : Sorting using bubble sort

$sorter = new Sorter(new QuickSortStrategy());
$sorter->sort($dataset); // Output : Sorting using quick sort
```

💢 State
-----
Un exemple dans le monde réel
> Imagine you are using some drawing application, you choose the paint brush to draw. Now the brush changes its behavior based on the selected color i.e. if you have chosen red color it will draw in red, if blue then it will be in blue etc.  

En clair
> It lets you change the behavior of a class when the state changes.

D'après Wikipédia
> The state pattern is a behavioral software design pattern that implements a state machine in an object-oriented way. With the state pattern, a state machine is implemented by implementing each individual state as a derived class of the state pattern interface, and implementing state transitions by invoking methods defined by the pattern's superclass.
> The state pattern can be interpreted as a strategy pattern which is able to switch the current strategy through invocations of methods defined in the pattern's interface.

**Exemple de programme**

Let's take an example of text editor, it lets you change the state of text that is typed i.e. if you have selected bold, it starts writing in bold, if italic then in italics etc.

First of all we have our state interface and some state implementations

```php
interface WritingState
{
    public function write(string $words);
}

class UpperCase implements WritingState
{
    public function write(string $words)
    {
        echo strtoupper($words);
    }
}

class LowerCase implements WritingState
{
    public function write(string $words)
    {
        echo strtolower($words);
    }
}

class Default implements WritingState
{
    public function write(string $words)
    {
        echo $words;
    }
}
```
Then we have our editor
```php
class TextEditor
{
    protected $state;

    public function __construct(WritingState $state)
    {
        $this->state = $state;
    }

    public function setState(WritingState $state)
    {
        $this->state = $state;
    }

    public function type(string $words)
    {
        $this->state->write($words);
    }
}
```
And then it can be used as
```php
$editor = new TextEditor(new Default());

$editor->type('First line');

$editor->setState(new UpperCase());

$editor->type('Second line');
$editor->type('Third line');

$editor->setState(new LowerCase());

$editor->type('Fourth line');
$editor->type('Fifth line');

// Output:
// First line
// SECOND LINE
// THIRD LINE
// fourth line
// fifth line
```

📒 Template Method
---------------

Un exemple dans le monde réel
> Suppose we are getting some house built. The steps for building might look like
> - Prepare the base of house
> - Build the walls
> - Add roof
> - Add other floors

> The order of these steps could never be changed i.e. you can't build the roof before building the walls etc but each of the steps could be modified for example walls can be made of wood or polyester or stone.

En clair
> Template method defines the skeleton of how a certain algorithm could be performed, but defers the implementation of those steps to the children classes.

D'après Wikipédia
> In software engineering, the template method pattern is a behavioral design pattern that defines the program skeleton of an algorithm in an operation, deferring some steps to subclasses. It lets one redefine certain steps of an algorithm without changing the algorithm's structure.

**Exemple de programme**

Imagine we have a build tool that helps us test, lint, build, generate build reports (i.e. code coverage reports, linting report etc) and deploy our app on the test server.

First of all we have our base class that specifies the skeleton for the build algorithm
```php
abstract class Builder
{

    // Template method
    final public function build()
    {
        $this->test();
        $this->lint();
        $this->assemble();
        $this->deploy();
    }

    abstract public function test();
    abstract public function lint();
    abstract public function assemble();
    abstract public function deploy();
}
```

Then we can have our implementations

```php
class AndroidBuilder extends Builder
{
    public function test()
    {
        echo 'Running android tests';
    }

    public function lint()
    {
        echo 'Linting the android code';
    }

    public function assemble()
    {
        echo 'Assembling the android build';
    }

    public function deploy()
    {
        echo 'Deploying android build to server';
    }
}

class IosBuilder extends Builder
{
    public function test()
    {
        echo 'Running ios tests';
    }

    public function lint()
    {
        echo 'Linting the ios code';
    }

    public function assemble()
    {
        echo 'Assembling the ios build';
    }

    public function deploy()
    {
        echo 'Deploying ios build to server';
    }
}
```
And then it can be used as

```php
$androidBuilder = new AndroidBuilder();
$androidBuilder->build();

// Output:
// Running android tests
// Linting the android code
// Assembling the android build
// Deploying android build to server

$iosBuilder = new IosBuilder();
$iosBuilder->build();

// Output:
// Running ios tests
// Linting the ios code
// Assembling the ios build
// Deploying ios build to server
```

## 🚦 Wrap Up Folks

And that about wraps it up. I will continue to improve this, so you might want to watch/star this repository to revisit. Also, I have plans on writing the same about the architectural patterns, stay tuned for it.

## 👬 Contribution

- Report issues
- Open pull request with improvements
- Spread the word
- Reach out to me directly at kamranahmed.se@gmail.com or on twitter [@kamranahmedse](http://twitter.com/kamranahmedse)

## License
MIT © [Kamran Ahmed](http://kamranahmed.info)
