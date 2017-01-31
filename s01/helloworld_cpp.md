<p><sup><a href="../">retour</a></sup></p>

# Helloworld

```cpp
#include <iostream> // std::cout
int main()
{
    std::cout << "Hello World !!" << std::endl;
    return 0;
}
```

Ce programme **C++** est très simple, c'est un programme de console qui permet d'afficher la phrase "*Hello World !!*".  
Le premier programme que nous avons réalisé au premier cours du 1er semestre en langage C produisait le même résultat (si vous ne vous en souvenez pas voici [un lien](https://github.com/paccpp/StarterKit/blob/master/source/projects/helloworld_c/main.c) vers celui-ci).

Même si ces deux programmes sont très similaires, on distingue néanmoins des différences entre ces deux languages.

## Extension de fichiers c++

En C le fichier compilé s'appelait `main.c`, en C++ il s'appelle `main.cpp`.  
L'extension la plus courante pour des fichiers compilés en C++ est `.cpp` (mais on peut aussi trouver .cxx ou autres).  
Pour les fichiers d'inclusion appelés aussi *headers*, l'extension la plus courante est .hpp (mais on pourra aussi trouver des `.h` ou autres).

## Les espaces de noms (ou namespaces) et l'operateur de portée `::`

Les [namespaces](https://www.tutorialspoint.com/cplusplus/cpp_namespaces.htm) sont une nouveauté du language C++. Ils permettent d'isoler du code (fonctions, variables, types...) dans un espace restreint pour ne pas polluer l'espace de nom global.  
(En C, toutes les fonctions d'un programme doivent avoir un nom différent pour ne pas provoquer de conflits).

Les namespaces se déclarent comme ça :

```cpp
namespace foo
{
  // fonction play à l'intérieur du namespace foo.
  void play();
}

namespace bar
{
  // une fonction du même nom, mais à l'intérieur du namespace bar.
  void play();
}

// une fonction du même nom, mais au sein du namespace global.
void play();
```

pour appeler l'une de ces fonctions en particulier on va pouvoir lever l'ambiguïté en utilisant l'opérateur `::`, nommé opérateur de portée :

```cpp
int main()
{
  play();       // appel de la fonction globale.
  foo::play();  // appel de la fonction play du namespace foo.
  bar::play();  // appel de la fonction play du namespace bar.
}
```

Dans le programme helloworld on utilise l'opérateur `::` pour faire appel à `cout` et `endl`. Une autre syntaxe permet d'exposer tout ou partie d'un autre namespace au sein du namespace courrant :

```c++
...
int main()
{
  using std::cout;  // on se sert de cout au sein du namespace std
  cout << "blabla" << std::endl; // endl n'est pas exposé

  // si on écrit :
  using namespace std;  // dans ce cas on utilise tout le namespace std et on peut donc écrire
  cout << "blabla" << endl;
}
```

## `std::cout`, `std::endl` et operateur de stream

Pour afficher du texte dans la console de sortie on utilise `std::cout` (console output). `std::endl` (end of the line) sert à effectuer un retour à la ligne.

l'opérateur `<<`, est l'operateur *stream* de sortie.

il existe l'opérateur inverse `>>` qui permet de lire une stream et l'inverse de `std::cout`, `std::cin` (console input) qui permet de recevoir une entrée clavier de l'utilisateur.

l'operateur de *stream* est compatible avec beaucoup de types, on peut par exemple écrire:

```cpp
int entier = 4;
float flottant = 3.14;
std::string str = "du texte";

std::cout << "mon entier contient " << entier << ", mon flottant " << flottant << " et mon texte : " << str << std::end;

// ce qui produit la sortie suivante :
// mon entier contient 4, mon flottant 3.140000 et mon texte : du texte
```

=> [en savoir plus](https://www.tutorialspoint.com/cplusplus/cpp_basic_input_output.htm)
