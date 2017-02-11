<p><sup><a href="readme.md">retour</a></sup></p>

# Introduction

Dans l'exemple suivant la classe `Flanger` et la classe `Phasor` héritent toutes deux d'une même classe `Processor`. Ces 3 classes possèdent une méthode `process` qui produit un résultat différent suivant la classe de l'objet sur laquelle elle est appelée.

```cpp
#include <iostream>

class Processor
{
public:

    void process()
    {
        std::cout << "Processor: default process !" << std::endl;
    }
};

// La classe Flanger hérite de la class Processor
class Flanger : public Processor
{
public:

    void process()
    {
        std::cout << "Flanger: yeeeaah !" << std::endl;
    }
};

class Phasor : public Processor
{
public:

    void process()
    {
        std::cout << "Phasor: bzzzzzz !" << std::endl;
    }
};

int main()
{
    Processor processor;
    Flanger flanger;
    Phasor phasor;

    processor.process();  // Processor: default process !
    flanger.process();    // Flanger: yeeeaah !
    phasor.process();     // Phasor: bzzzzzz !

    return 0;
}
```

# Polymorphisme

Supposons maintenant que nous voulions stocker ces processeurs dans un vecteur pour les traiter de manière groupée :

```cpp
#include <iostream>
#include <vector>

// [...]

int main()
{
    Processor processor;
    Flanger flanger;
    Phasor phasor;

    // on stocke dans un vector des pointeurs de processeurs.
    std::vector<Processor*> processors {&processor, &flanger, &phasor};

    // appel de la fonction process de chaque objets à partir de la classe de base
    for(Processor* p : processors)
    {
        p->process();
    }

    return 0;
}
```

Output:

```
Processor: default process !
Processor: default process !
Processor: default process !
```

Ca n'a pas marché car la résolution de la fonction a été faite de manière statique à la compilation en pointant vers la fonction de la classe de base.  
Pour modifier ce comportement et faire en sorte que la fonction appelée soit différente en fonction du type réel d'objet sur laquelle elle est appelée on doit la rendre virtuelle.

## Fonction *virtuelles*

Une fonction virtuelle est une fonction d'une classe de base précédée dans sa déclaration du mot-clef `virtual`.  
Quand on défini une fonction virtuelle on indique au compilateur que l'appel de cette fonction doit être relative au type d'objet réel à partir duquel elle est appelée. (**static linkage** vs **dynamic linkage**).

En modifiant le programme ci-dessus en rendant la fonction `process` virtuelle et en indiquant dans les classes dérivées qu'elle est spécialisée grâce au mot-clef `override` on obtient bien le bon résultat :

```cpp
#include <iostream>
#include <vector>

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

int main()
{
    Processor processor;
    Flanger flanger;
    Phasor phasor;

    std::vector<Processor*> processors {&processor, &flanger, &phasor};

    // appel de la fonction process à partir de la classe de base
    for(Processor* p : processors)
    {
        p->process();
    }

    return 0;
}
```

Output:

```
Processor: default process !
Flanger: yeeeaah !
Phasor: bzzzzzz !
```

## Fonction *virtuelles pures*

Quand on déclare une fonction virtuelle pure on indique que cette fonction **doit** être spécialisée par une **classe dérivée**.  
Une classe qui comporte au moins une fonction **virtuelle pure** est appelée classe abstraite dans la mesure elle ne peut plus être instanciée directement.

```cpp
#include <iostream>

class Processor
{
public:

    // destructeur virtuel
    virtual ~Processor() = default;

    // méthode process déclaré virtuelle PURE.
    virtual void process() = 0;
};

// La classe Flanger hérite de la class Processor,
// elle DOIT spécialiser la fonction process
class Flanger : public Processor
{
public:

    // spécialisation de la méthode process
    void process() override
    {
        std::cout << "Flanger: yeeeaah !" << std::endl;
    }
};

int main()
{
    //Processor processor; // Compile Error: Variable type 'Processor' is an abstract class
    Flanger flanger;

    return 0;
}
```
