---
layout: stagiaire
title: Programmation orientée objet
author: Guilbert Vincent
---

# Programmation Orientée Objet

La programmation orientée objet (POO, ou OOP en anglais) est un paradigme de programmation informatique, une façon d'organiser ses progammes, qui consiste à créer des briques logicielles, appelées objets, et les faire intéragir entre elles.  

## Les Objets

Un objet en POO peut être comparé à un objet du monde réel : prenons l'exemple d'un téléphone, il possède des propriétés ( attributs ) comme sa couleur, sa taille, son poids... et il peut faire des actions ( appelées méthodes ) comme passer des appels, prendre des photos, etc.  
Cet objet intéragit avec d'autres objets : avec l'utilisateur grâce au clavier, avec d'autres téléphones, avec différents réseaux...  

Ce type de programmation intègre un concept important : l'encapsulation.  
L'encapsulation est un mécanisme consistant à rassembler les données et les méthodes au sein de l'objet en cachant l'implémentation de celui-ci, c'est-à-dire en empêchant l'accès aux données par d'autres moyens que ceux proposés. L'encapsulation permet donc de garantir l'intégrité des données contenues dans l'objet.  

## Les Classes

La création d'un objet se fait par ce qu'on appelle une classe. La classe permet de définir l’objet, ses attributs et ses méthodes. C’est un peu « le moule » qu’on utilisera pour instancier un objet.  

Exemple en PHP :  

Définition de la classe :

``` php
class Téléphone
{
    // Attributs
    private $couleur;
    private $taille;
    private $poids;

    // Méthodes
    public function passerAppel()
    {
        ...
    }

    public function prendrePhoto()
    {
        ...
    }

}
```
Instanciation ( création ) d'un objet Téléphone :  

``` php
$téléphone = new Téléphone;
```

Tous les objets crées posséderont les attributs définis dans la classe ainsi que l'accès à ses méthodes, pour appeler une méthode de l'objet, on le fait suivre de l'opérateur "->" puis on appelle la méthode souhaité :

``` php
$téléphone->passerAppel();
```

## Notion de visibilité

Dans notre classe Téléphone nous pouvons voir deux mots-clés : public et private. Ils font référence à la visibilité / accessibilité des attributs et des méthodes. Le mot-clé public sert à indiquer que notre méthode/attribut peut être accessible depuis d’autres classes alors que le mot-clé private permet d’avoir une méthode/attribut qui n’est accessible que depuis la classe dans laquelle elle est définie.  
En générale, il est recommandé de déclarer les attributs private et les méthodes public.  

Pour accéder aux attributs private en dehors de la classe on doit donc créer des méthodes public qui vont nous permettre de les récuperer ou les modifier :

``` php
// Récuperer
public function getCouleur()
{
    return $this->couleur;
}

// Modifier
public function setCouleur($couleur)
{
    $this->couleur = $couleur;
    return $this;
}
```

## L'héritage

L'héritage est un principe propre à la programmation orientée objet, permettant de créer une nouvelle classe à partir d'une classe existante. Prenons par exemple une classe B qui hériterait de la classe A. La classe A est donc considérée comme la classe mère et la classe B est considérée comme la classe fille.  
La classe B héritera de tout les attributs et les méthodes de la classe A.  
L'intérêt majeur de l'héritage est de pouvoir définir de nouveaux attributs et de nouvelles méthodes pour la classe dérivée, qui viennent s'ajouter à ceux et celles héritées.
Cela a comme avantage de ne pas avoir à repartir de zéro lorsque l'on veut spécialiser une classe existante.  



Pour faire hériter une classe on utilise le mot-clé 'extends', exemple : 

``` php
class Vehicule
{
    ...
}

class Voiture extends Vehicule
{
    ...
}

class Moto extends Vehicule
{
    ...
}
```
On peut aller aussi loin que l'on veut en héritant des classes elles mêmes héritées : 

``` php
class Citadine extends Voiture
{
    ...
}
```

Avec l'héritage on a accés à un nouveau type de visiblité : protected.  
Le type de visibilité protected fonctionne un peu comme le type private à l'exception que toute classe fille aura accès aux éléments protégés.

## Mise en place de contraintes

### Abstraction : 

- Classes Abstraites  

Grâce au mot-clé abstract placé avant class on peut empêcher l'instanciation d'une classe ( la création d'objets ). On ne pourra pas se servir directement de la classe. La seule façon d'exploiter ses méthodes est de créer une classe héritant de la classe abstraite.  

Par exemple on ne veut pas directement créer d'objets de la classe Véhicule car ça n'aurais pas de sens, il faut d'abord créer une classe par exemple Voiture qui hérite de cette classe abstraite : 

``` php
abstract class Vehicule
{
    ...
}

class Voiture extends Vehicule
{
    ...
}
```

- Méthodes Abstraites

On peut également définir des méthodes abstraites en plaçant le mot-clé abstract avant la visibilité de la méthode. Cela aura pour effet de forcer toutes les classes filles à réecrire cette méthode. Puisque chaque classe fille écrira cette méthode, on ne doit spécifier aucune instructions dans cette méthode.
Attention, pour définir une méthode comme étant abstraite, il faut que la classe elle-même soit abstraite.

``` php
abstract class Vehicule
{
    abstract public function freiner();
}

class Voiture extends Vehicule
{
    public function freiner()
    {
        ...
    }
}
```

### Finalisation : 

- Classes finales

Le concept des classes et méthodes finales est exactement l'inverse du concept d'abstraction. Si une classe est finale, vous ne pourrez pas créer de classe fille héritant de cette classe.  
Pour déclarer une classe finale, vous devez placer le mot-clé final juste avant le mot-clé class.

``` php
// On ne pourra plus créér de classe héritant de Voiture
final class Voiture extends Vehicule
{
    ...
}
```

- Méthodes finales

Si vous déclarez une méthode finale, toute classe fille de la classe comportant cette méthode finale héritera de cette méthode mais ne pourra la redéfinir. Si vous déclarez la méthode freiner en tant que méthode finale, vous ne pourrez la redéfinir.

``` php
abstract class Vehicule 
{
    final public function freiner()
    {
        ...
    }
}

class Voiture extends Vehicule
{
    // Erreur car cette méthode est finale dans la classe parent
    public function freiner()
    {
        ...
    }

}
```










