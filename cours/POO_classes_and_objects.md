<p><sup><a href="readme.md">retour</a></sup></p>

# Classes et objets

La principale motivation à la création du langage **C++** était de rajouter une dimension *orientée objet* au langage **C**.  
La notion de **classe** est centrale en **C++**, elle permet de créer de nouveaux types de données définis par l'utilisateur.  
Quand on défini une classe on modélise un objet c-à-d que l'on défini ses propriétés et ses méthodes.  
Les données et les fonctions d'une classe sont dîtes **membres**.

## C-struct

En langage **C**, une structure permet de définir un nouveau type de donnée fait de données hétérogènes toutes accessibles publiquement, en revanche on ne peut pas définir de fonctions au sein de cette structure pour manipuler les données qu'elle contient:

```c
// (c)
struct Effect
{
  int     m_muted;
  int     m_bypassed;
  double  m_volume;
  double  m_pan;
  // ...
};

void mute_effect(Effect* fx)
{
  fx->m_muted = 1;
}

double get_volume(Effect* effect)
{
  return fx->m_volume;
}

int main()
{
  Effect someEffect = {0, 0, 1., 0.5};
  mute_effect(&someEffect);
  double volume = get_volume(&someEffect);
  return 0;
}
```

## C++ `struct`

En **C++** on peut définir une structure de la même façon, mais cette fois on va pouvoir ajouter des méthodes directement à l'intérieur de la structure.

```cpp
// (c++)
struct Effect
{
  bool    m_muted;
  bool    m_bypassed;
  double  m_volume;
  double  m_pan;

  void mute()
  {
    m_muted = true;
  }

  double getVolume()
  {
    return m_volume;
  }
};

int main()
{
  Effect someEffect = {0, 0, 1., 0.5};
  someEffect.mute();
  double volume = someEffect.getVolume();
  return 0;
}
```

## C++ `struct` VS C++ `class`

On peut faire exactement la même chose avec une `struct` ou une `class` en **C++**, la seule seule chose qui diffère est le comportement d'accès aux fonction et données **membres** par défaut.
Par défaut, les données et fonctions **membres** d'une structure sont publiques, celles d'une classe sont par défaut privées.

```cpp
struct EffectStruct
{
  // public par défaut
  void mute() { m_muted = true; }
  double getVolume() { return m_volume; }

  bool    m_muted;
  double  m_volume;
};

class EffectClass
{
  // private par défaut
  void mute() { m_muted = true; }
  double getVolume() { return m_volume; }

  bool    m_muted;
  double  m_volume;
};

int main()
{
  EffectStruct fx_struct;   // declare variable fx_struct of type EffectStruct
  fx_struct.mute();         // public method is accessible
  fx_struct.m_volume = 10;  // public data member is accessible

  EffectClass fx_class;
  fx_class.mute();          // Error: mute method is private
  fx_class.m_volume = 10;   // Error: m_volume data member is private
}
```

## Définition explicite des modifieurs d'accès aux données et fonctions membres

Ce comportement par défaut peut être modifié en redéfinnissant explicitement l'accès aux variables et fonctions membres grâce aux mots-clefs `public`, `protected` et `private`.

```cpp
class Effect
{
public: // ce qui suit est maintenant publiquement accéssible

  void mute() { m_muted = true; }
  double getVolume() { return m_volume; }

private: // ce qui suit est maintenant privé (interne à la classe)

  bool    m_muted;
  double  m_volume;
};

int main()
{
  Effect fx;          // declare variable fx of type Effect
  fx.mute();          // mute method is public
  fx.m_volume = 10;   // Error: m_volume data member is still private
}
```

Les mots-clefs `public` et `private` servent donc à définir ce qui va pouvoir être accéssible ou non depuis l'extérieur de la classe, c'est la base du concept d'[abstraction](POO_concepts.md#abstraction) en POO.
Le mot-clef `protected` sert quant à lui à rendre accéssibles des variables ou fonctions membres uniquement au sous-classes (classes qui héritent d'une classe de base) mais pas à l'extérieur de la classe.


## Définition des fonctions membres à l'exterieur de la classe

Les fonctions membres d'une classe doivent être **déclarées** à l'intérieur de la classe, on peut par contre écrire leur **définition** soit directement dans la classe soit à l'extérieur en utilisant l'opérateur de résolution de portée `::`.

```cpp
class Effect
{
public:

  // simple déclaration de la fonction mute
  void mute();

  // déclaration ET définition de la fonction getVolume
  double getVolume()
  {
    return m_volume;
  }

private:

  bool    m_muted;
  double  m_volume;
};

// définition de la fonction mute déclarée dans la classe Effect
void Effect::mute()
{
  // la fonction mute fait partie de la classe effect, on a donc accès aux variables privées.
  m_muted = true;
}

int main()
{
  Effect fx;          // declare variable fx of type Effect
  fx.mute();
  double volume = fx.getVolume();
}
```

## Variables et fonctions membres statiques

Les variables dîtes statiques sont des variables qui ne dépendent pas d'une instance de classe particulière mais qui sont globales à toutes les instances d'une classe donnée. Une variable statique se déclare en faisant précéder le type de variable du mot-clef `static`, ex `static size_t id;`.

Les fonctions membres statiques peuvent être appelées sans instance d'objet. Une fonction est statique quand elle est précédée du mot-clef `static`, ex `static size_t getId();`.

```cpp
#include <iostream>

class Clip
{
public:

    // Constructeur par defaut
    Clip()
    {
        m_count++;
    }

    // Destructor
    ~Clip()
    {
        m_count--;
        std::cout << "clip dtor called" << std::endl;
        std::cout << m_count << " clip(s) left" << std::endl;
    }

    // methode statique
    static size_t getNumberOfInstance()
    {
        return m_count;
    }

    // variable statique
    static size_t m_count;
};

// les variables membres statiques doivent être initialisées en dehors de la classe
size_t Clip::m_count = 0ul;

int main()
{
    std::cout << "Number of clip : " << Clip::getNumberOfInstance() << std::endl;

    Clip clip_1;
    std::cout << "Number of clip : " << Clip::getNumberOfInstance() << std::endl;

    {
        Clip clip_2;
        std::cout << "Number of clip : " << Clip::getNumberOfInstance() << std::endl;
    }

    std::cout << "Number of clip : " << Clip::getNumberOfInstance() << std::endl;

    Clip clip_3;
    std::cout << "Number of clip : " << Clip::getNumberOfInstance() << std::endl;

    return 0;
}
```

## Encapsulation getter/setter

```cpp
#include <iostream>

class StereoEffect
{
public:

    // Constructor
    StereoEffect(double volume = 1., double panning = 0.5) : m_volume(volume), m_pan(panning)
    {
        ;
    }

    // la fonction modifie la variable m_pan et ne peut donc pas être constante
    void setPan(const double pan)
    {
        // on s'assure que la valeur de panning reste toujours entre 0. et 1.
        m_pan = (pan < 0.) ? 0. : (pan > 1.) ? 1. : pan;
    }

    // cette fonction ne modifie aucune variable, elle peut donc être marquée const
    double getPan() const
    {
        return m_pan;
    }

    // la fonction modifie la variable volume et ne peut donc pas être constante
    void setVolume(const double volume)
    {
        // on s'assure que le volume ne peut pas être négatif.
        m_volume = (volume > 0.) ? volume : 0.;
    }

    // Abstraction du calcul du volume gauche dans la fonction
    double getLeftVolume() const
    {
        return m_volume * (1. - m_pan);
    }

    // Abstraction du calcul du volume gauche dans la fonction
    double getRightVolume() const
    {
        return m_volume * m_pan;
    }

private:

    double  m_volume;
    double  m_pan;
};

int main()
{
    StereoEffect fx;

    std::cout << "volume Left = " << fx.getLeftVolume() << std::endl;
    std::cout << "volume Right = " << fx.getRightVolume() << std::endl;

    std::cout << "set panning to 0.25" << std::endl;
    fx.setPan(0.25);

    std::cout << "volume Left = " << fx.getLeftVolume() << std::endl;
    std::cout << "volume Right = " << fx.getRightVolume() << std::endl;

    std::cout << "set volume to 2." << std::endl;
    fx.setVolume(2.);

    std::cout << "volume Left = " << fx.getLeftVolume() << std::endl;
    std::cout << "volume Right = " << fx.getRightVolume() << std::endl;

    return 0;
}
```
