<p><sup><a href="../readme.md">retour</a></sup></p>

# Exercices sur la [POO](../POO_concepts.md) :

## exercice 01

Écrire une classe `GainEffect` avec les bonnes variables et fonctions membres, qui permette de faire compiler le code suivant et faire sortir les bonnes valeurs en console.

```cpp
#include <iostream>
#include <string>

//----
class GainEffect
{
    // [...]
};
//----

int main()
{
    GainEffect fx;

    fx.setVolume(0.5);
    std::cout << "fx volume : " << fx.getVolume() << std::endl;
    // fx volume : 0.5

    fx.setVolume(-10);
    std::cout << "fx volume : " << fx.getVolume() << std::endl;
    // fx volume : 0.

    fx.mute();
    std::cout << "fx muted : " << std::boolalpha << fx.muted() << std::endl;
    // fx muted : true

    fx.unmute();
    std::cout << "fx muted : " << std::boolalpha << fx.muted() << std::endl;
    // fx muted : false

    std::string name = "gain";
    fx.setName(name);
    std::cout << "fx name : " << fx.getName() << std::endl;
    // fx name : gain
}
```

## exercice 02

Le code suivant se sert de la classe `GainEffect` créée à l'[exercice 01](#exercice-01).  
Vous devrez cette fois-ci rajouter à cette classe une fonction membre nommée `process` qui devra multiplier chaque sample d'un vecteur donné en entrée par le volume courant, seulement si l'effet n'est pas muté.  
Le code suivant devra compiler et donner les bons résultats en sortie.

> Note: vous pouvez relire la partie sur les [containers](../containers.md) pour savoir comment manipuler un `std::vector`

```cpp
#include <iostream>
#include <string>
#include <vector>

//----
class GainEffect
{
    // [...]
};
//----

int main()
{
    GainEffect fx;
    fx.setVolume(0.5);

    // initialisation d'un vecteur de samples à traiter :
    std::vector<double> buffer {0.1, 0.2, 0.3, 0.4, 0.5, 0.6, 0.7, 0.8, 0.9, 1.};

    // Print buffer
    std::cout << "buffer : ";
    std::copy(buffer.begin(), buffer.end(), std::ostream_iterator<double>(std::cout, " "));
    std::cout << std::endl;
    // print: "buffer : 0.1 0.2 0.3 0.4 0.5 0.6 0.7 0.8 0.9 1"

    fx.process(buffer);

    // Print buffer
    std::cout << "buffer : ";
    std::copy(buffer.begin(), buffer.end(), std::ostream_iterator<double>(std::cout, " "));
    std::cout << std::endl;
    // print: "buffer : 0.05 0.1 0.15 0.2 0.25 0.3 0.35 0.4 0.45 0.5"

    fx.mute();
    fx.process(buffer);

    // Print buffer
    std::cout << "buffer : ";
    std::copy(buffer.begin(), buffer.end(), std::ostream_iterator<double>(std::cout, " "));
    std::cout << std::endl;
    // print: "buffer : 0.05 0.1 0.15 0.2 0.25 0.3 0.35 0.4 0.45 0.5"

    return 0;
}
```
