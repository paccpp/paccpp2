<p><sup><a href="readme.md">retour</a></sup></p>

# Containers

## Introduction aux conteneurs de la std.

La bibliothèque standard du C++ contient des conteneurs qui peuvent être utilisés pour stocker et gérer des éléments en mémoire ([`std::array`](std::array), [`std::vector`](std::vector), `std::list`, `std::map`, `std::set` ...).  

Ces conteneurs peuvent avoir des méthodes en commun mais ont souvent des propriétés différentes et ne gèrent pas tous de la même façon les données stockées en mémoire; Certains conteneurs peuvent être parcouru de manière séquentielle (*Sequence containers*); d'autres stockent les données sous une forme *clef-valeur* (*associative-containers*)...

=> [plus d'information](http://en.cppreference.com/w/cpp/container).

### std::array

Même s'il est encore possible d'initialiser un tableau statique fixe de la même façon qu'en C,  
on préférera en C++ passer par le container `std::array`.

Le `std::array` est classé dans la catégorie des *conteneurs séquentiels*, il sert à créer un tableau d'élément de même type dont la taille est connue à l'avance.

Un `std::array` prend deux paramètres obligatoires en template, le type de donnée à stocker, et le nombre d'éléments (la taille du tableau):  
=> déclaration d'un tableau de 5 entiers : `std::array<int, 5> tab;`.

Utilisation du `std::array` :

```cpp
#include <iostream>
#include <array>
#include <iterator>
#include <algorithm>

int main()
{
    // La construction utilise l'aggrégat d'initialisation
    std::array<int, 3> freqs { {440, 220, 110} };   // Double accolades requises
    //std::array<int, 3> freqs = {3, 2, 1};         // sauf après un =

    // on peut accéder à une case du tableau en particulier avec la méthode 'at'
    std::cout << "freqs.at(1): " << freqs.at(1) << std::endl;
    // ou de la même façon avec l'opérateur '[]' pour lire ou écrire dans une case
    freqs[1] = 6;
    std::cout << "freqs[1]: " << freqs[1] << std::endl;

    // --------------------------------------------------------------------

    std::cout << "freqs : [";
    for(int i = 0; i < freqs.size(); ++i)
    {
        std::cout << freqs[i] << ", ";
    }
    std::cout << "]" << std::endl;

    // Les opérations sur les conteneurs sont supportées
    std::sort(freqs.begin(), freqs.end());

    std::cout << "sorted freqs : [";
    for(int i = 0; i < freqs.size(); ++i)
    {
        std::cout << freqs[i] << ", ";
    }
    std::cout << "]" << std::endl;

    // --------------------------------------------------------------------

    // un std::array n'a pas forcément à être initialisé à la construction
    std::array<float, 10> buffer;

    // rempli toutes les cases du tableau avec une même valeur
    buffer.fill(0.5);

    for(float f : buffer)
        std::cout << f << ' ';
    std::cout << std::endl;
    return 0;
}
```

### std::vector

Le `std::vector` (tout comme le `std::array`) est classé dans la catégorie des *conteneurs séquentiels*, il sert à stocker un nombre d'éléments variable en mémoire.  
Le `std::vector` fourni une interface qui **encapsule** un tableau de taille dynamique, elle permet d'ajouter ou supprimer des éléments, d'itérer sur les valeurs contenues de manière séquentielle, d'effectuer des opérations dessus...

Un `std::vector` prend un seul paramètre obligatoire en template: le type de donnée à stocker :  
=> déclaration d'un vecteur d'entiers : `std::vector<int> vec;`.

Utilisation du `std::vector` :

```cpp
#include <iostream>
#include <vector>

int main()
{
    // creation d'un vecteur d'entier
    std::vector<int> vec;

    // affiche la taille du vecteur
    std::cout << "vector size = " << vec.size() << std::endl;

    // ajoute 5 valeurs dans le vecteur
    for(int i = 0; i < 5; i++)
    {
        vec.push_back(i);
    }

    std::cout << "extended vector size = " << vec.size() << std::endl;

    // accède au 3ème case du tableau et modifie sa valeur
    vec[3] = 42;

    // affiche les 5 premières valeurs du vecteur
    for(int i = 0; i < 5; i++)
    {
        std::cout << "value of vec [" << i << "] = " << vec[i] << std::endl;
    }

    // supprime le 1er element (va modifier sa taille)
    vec.erase(vec.begin());
    std::cout << "new vector size = " << vec.size() << std::endl;

    // affiche les valeurs du vecteur avec un boucle de type ranged-for (c++11)
    for(int v : vec)
    {
        std::cout << "value of v = " << v << std::endl;
    }
    return 0;
}
```

### Itérerateurs

Les iterateurs permettent... d'itérer sur des conteneurs, la plupart des conteneurs de la `std` fournissent des itérateurs grâce à des méthodes communes.

```cpp
std::vector<int> vec {0, 1, 2, 3, 4, 5, 6, 7, 8, 9};
std::vector<int>::iterator begin_it = vec.begin();  // renvoie un itérateur sur le premier élément du vecteur
std::vector<int>::iterator end_it = vec.end();      // renvoie un itérateur sur le dernier élément du vecteur
```

Les **iterateurs** sont des objets qui se comportent comme des **pointeurs**, ils ont la même arithmétique, on peut donc par exemple s'en servir pour itérer sur un conteneur ou obtenir la valeur à laquelle il fait référence, on déréférence un iterateur comme un pointeur pour accéder à la variable pointée.

```cpp
std::vector<int> vec {0, 1, 2, 3, 4, 5, 6, 7, 8, 9};
for(std::vector<int>::iterator it = vec.begin(); it != vec.end(); ++it)
{
  int value = *it; // obtention de la valeur par déréférencement de l'iterateur.
  std::cout << "value: " << value << std::endl;
}
```

Les *itérateurs* sont aussi utilisés dans les algorithmes de la `std` (`std::find()`, `std::sort()`...).

```cpp
#include <iostream>
#include <vector>
#include <algorithm> // std::find...

int main()
{
    std::vector<std::string> notes {"Do#", "Mib", "Fa#", "Sol"};

    // déclaration d'un iterateur
    std::vector<std::string>::iterator it;

    // la fonction std::find renvoie un iterateur vers un élément du vecteur
    // si cet élément a été trouvé ou un iterateur vers la fin du vecteur si il n'existe pas.
    it = std::find(notes.begin(), notes.end(), "Fa#");

    // on vérifie que l'élément a bien été trouvé.
    if(it != notes.end())
    {
        // on supprime cet élément du vecteur.
        notes.erase(it);
    }

    // l'élément a bien été supprimé
    for(std::string const& s : notes)
    {
        std::cout << s << std::endl;
    }

    return 0;
}
```
