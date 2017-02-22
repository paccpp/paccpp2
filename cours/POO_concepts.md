<p><sup><a href="readme.md">retour</a></sup></p>

# Introduction à la POO

Le concept de base de la programmation orientée objet est de créer des **objets** qui ont des **propriétés** et des **méthodes**.

### Objet

Un **objet** possède une structure interne et un comportement, c'est l'unité de base de la POO qui embarque à la fois des données et des fonctions qui opèrent sur ces données. Un **objet** est l'**instance** d'une **classe**.

=> [en savoir plus](POO_classes_and_objects.md).

### Classe

Une classe défini le *moule* d'un objet, son nom, ce qu'un objet de cette classe va faire, les données qu'il va stocker et les opérations qui vont être permises sur cet objet.

=> [en savoir plus](POO_classes_and_objects.md).

### Abstraction

Le concept d'**abstraction** fait référence au fait de rendre accéssibles uniquement les informations et données nécessaires à l'extérieur d'une classe en cachant et en gardant le détail et la complexité de l'implémentation à l'intérieur de la classe.

### Encapsulation

L'encapsulation est le fait de regrouper les données et les fonctions qui opèrent sur ces données au même endroit, au sein d'une même classe par exemple.

### Héritage

L'héritage permet la création d'une nouvelle classe en héritant d'une ou plusieurs autres classes, on appelle **classe dérivée** ou **classe enfant** la nouvelle classe et **classe de base** ou **classe mère** la classe dont la nouvelle classe hérite.
L'héritage permet de réutiliser du code en faisant hériter la nouvelle classe des propriétés et méthodes de la classe de base.

=> [en savoir plus](POO_inheritance.md).

### Polymorphisme

On appelle polymorphisme le fait de fournir une même interface pour des entités de types différents, la possibilité d'utiliser un opérateur ou une fonction différement suivant le contexte ou le type données sur lesquels ils opèrent.

#### Polymorphisme ad-hoc

Le polymorphisme ad-hoc est aussi nommé [surcharge](#surcharge).

#### Polymorphisme paramétrique

C'est quand un code une partie de code est écrit sans spécifier de type particulier et qui peut donc être utilisé avec différents types. c'est la base de la **programmation générique** et de la programmation avec des templates.

```cpp
template<class T>
T add(T a, T b)
{
  return a + b;
}

// la même fonction add peut être appelée de la même façon avec des nombres entiers ou des flottants
int i = add(10, 20);
float f = add(1.5f, 3.7f);
```

#### Polymorphisme par sous-typage

Aussi nommé **polymorphisme par héritage**, est la possibilité d'appeler la méthode d'un objet sans se soucier de son type intrinsèque. Quand une classe hérite d'une autre elle peut spécialiser une méthode en la redéfinissant de manière à ce que l'on puisse manipuler avec la même interface plusieurs objets d'une classe de base faisant référence à des objets de classes concrètes ou dérivées différentes.

```cpp
#include <iostream>

class Processor
{
public:

    // destructeur virtuel
    virtual ~Processor() = default;

    // méthode process déclaré virtuelle pour pouvoir la spécialiser.
    virtual void process()
    {
        std::cout << "Processor: default process !" << std::endl;
    }
};

// La classe Flanger hérite de la class Processor
class Flanger : public Processor
{
public:

    // spécialisation de la méthode process
    void process() override
    {
        std::cout << "Flanger: yeeeaah !" << std::endl;
    }
};

class Phasor : public Processor
{
public:

    void process() override
    {
        std::cout << "Phasor: bzzzzzz !" << std::endl;
    }
};

// appel de la fonction process par passage par référence de la classe de base
void process(Processor& proc)
{
    proc.process();
}


int main()
{
    Processor processor;
    Flanger flanger;
    Phasor phasor;

    process(processor); // Processor: default process !
    process(flanger);   // Flanger: yeeeaah !
    process(phasor);    // Phasor: bzzzzzz !

    return 0;
}
```

### Surcharge

Le concept de la **surcharge** est lié au **polymorphisme ad-hoc**. On appelle surcharge le fait qu'un opérateur ou une fonction puisse opérer sur un nouveau type de donnée.

```cpp
int add(int a, int b);
int add(float a, float b); // surcharge de la fonction add avec des flottants.
```
