<p><sup><a href="readme.md">retour</a></sup></p>

# Constructeur(s) et destructeur

Le **constructeur** et le **destructeur** d'une classe (ou d'une structure) sont des fonctions **membres** particulières qui sont appelées respectivement à la constructionet à la destruction d'un objet de cette classe.
Le constructeur et le destructeur d'un objet ne peuvent et ne sont appelés qu'une seule fois chacun au cours de la durée de vie d'un objet.

Le **constructeur** d'une classe est appelée à la construction d'un objet de cette classe, il sert à initialiser un objet de cette classe.  
Un **constructeur** a le même nom que la classe, ne retourne aucune valeur et peut prendre des paramètres en entrée.

Le **destructeur** d'une classe est appelé à la destruction d'un objet de cette classe (implicitement à la sortie d'un contexte d'execution ou explicitement détruit avec le mot-clef `delete` appliqué à un **pointeur** d'un objet de cette classe).  
Le **destructeur** sert à libérer la mémoire utilisée par un objet ou encore si on veut mettre fin à une opération en cours quand l'objet est supprimé.  
Le **destructeur** a le même nom que la classe et doit être préfixé du charactère **~**, il ne retourne jamais de valeur et ne prend aucun paramètres.

```cpp
#include <iostream>

class Clip
{
public:

    // Déclaration du constructeur
    Clip();

    // Déclaration du destructeur
    ~Clip();
};

// Définition du Constructeur
Clip::Clip()
{
    std::cout << "Clip object constructed" << std::endl;
}

// Définition du Destructeur
Clip::~Clip()
{
    std::cout << "Clip object destructed" << std::endl;
}

int main()
{
    Clip clip;
    return 0;
}
```

## Initialisation de variables au constructeur

Un constructeur sert à initialiser les variables membres d'une classe à la construction.
On peut initialiser ces variable dans le corps du constructeur mais on préfèrera, chaque fois que possible, le faire sous la forme d'une *liste d'initialisation* :

```cpp
#include <iostream>

class Clip
{
public:

    Clip();

private:

    double m_volume;
};

// on initialise le volume à 0. à la construction de l'objet
Clip::Clip() : m_volume(0.)
{
    std::cout << "m_volume = " << m_volume << std::endl;
}

int main()
{
    Clip clip; // print "m_volume = 0"
    return 0;
}
```

## Constructeur avec paramètres

Un constructeur peut prendre zéro, un ou plusieurs paramètres d'entrée, on va donc pouvoir définir des constructeurs qui vont nous permettre d'initialiser différemment deux objets d'une même classe à la construction.

```cpp
#include <iostream>

class Clip
{
public:

    // Déclaration d'un constructeur prenant un double en paramètre
    Clip(double volume);

private:

    double m_volume;
};

// Définition du constructeur et initialisation de la variable volume
Clip::Clip(double volume) : m_volume(volume)
{
    std::cout << "m_volume = " << m_volume << std::endl;
}

int main()
{
    // on peut maintenant créer un objet Clip initialisé avec un volume particulier
    Clip clip_1(1.); // print "m_volume = 1"
    Clip clip_2(0.); // print "m_volume = 0"

    return 0;
}
```

Attention, si on défini un constructeur qui prend des paramètres on doit fournir les arguments nécéssaires à la création, dans le code au-dessus on ne peut par exemple plus créer d'objet Clip en écrivant simplement `Clip clip;`.  
Si on veut continuer à pouvoir instancier un nouvel objet clip en tapant `Clip clip;` on doit soit rendre optionnels les paramètres du constructeur, en définissant le constructeur comme ça `Clip(double volume = 0.);`, soit **surcharger** le constructeur.

## Surcharge de constructeur

Le constructeur d'une classe, tout comme les autres fonctions membres, peut être **surchargé** pour faire appel à une methode de construction différente suivant les arguments passés à l'objet construit.

```cpp
#include <iostream>

class Clip
{
public:

    // Déclaration d'un constructeur par défaut sans paramètre
    Clip();

    // Déclaration d'un constructeur prenant un double en paramètre
    Clip(double volume);

private:

    double m_volume;
};

// Définition du constructeur par défaut
Clip::Clip() : m_volume(0.)
{
    std::cout << "default volume = " << m_volume << std::endl;
}

// Définition du constructeur et initialisation de la variable volume
Clip::Clip(double volume) : m_volume(volume)
{
    std::cout << "m_volume = " << m_volume << std::endl;
}

int main()
{
    // on peut maintenant soit créer un objet Clip avec un volume par défaut
    Clip clip_1;      // print "default volume = 0"

    // soit créer un objet Clip avec un volume particulier
    Clip clip_2(1.);  // print "m_volume = 1"

    return 0;
}
```

## Constructeur par copie

Le **constructeur** d'une classe peut aussi être surchargé pour permettre d'initialiser un objet avec une autre instance d'objet, on dit quand ce constructeur est appelé que l'objet est créé par copie. Généralement le constructeur par copie d'une classe permet de faire un *clone* d'un autre objet, le nouvel objet est alors dans le même *état* que l'objet avec lequel il a été créé.

```cpp
#include <iostream>

class Clip
{
public:

    // Déclaration d'un constructeur prenant un double en paramètre
    Clip(double volume);

    // Déclaration d'un constructeur par copie
    Clip(Clip const& other);

private:

    double m_volume;
};

// Définition du constructeur et initialisation de la variable volume
Clip::Clip(double volume) : m_volume(volume)
{
    std::cout << "m_volume = " << m_volume << std::endl;
}

// Définition du constructeur par copie
Clip::Clip(Clip const& other) : m_volume(other.m_volume)
{
    std::cout << "copied volume = " << m_volume << std::endl;
}

int main()
{
    // Création d'un objet Clip avec un volume de 1.
    Clip clip_1(1.);        // print "m_volume = 1"

    // création d'un objet Clip par copie d'un autre objet Clip
    Clip clip_2(clip_1);    // print "copied volume = 1"

    return 0;
}
```
