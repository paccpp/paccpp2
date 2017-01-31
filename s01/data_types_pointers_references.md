<p><sup><a href="readme.md">retour</a></sup></p>

# Variables et types

## Types de données *primitifs*

Les types primitifs sont utilisés pour stocker une donnée en mémoire.
Il existe différents types primitifs hérités du langage **C** (`char`, `int`, `float`, `double`, `void`, `wchar_t`). Le langage **C++** ajoute à ceux-là un nouveau type booléen, le `bool`, qui peut prendre uniquement la valeur `true` ou `false`.

```cpp
bool i_think = true;
bool i_am_not = !i_think;
```

Les booleans sont en fait des nombres, ils sont `false` quand ils sont initialisés à 0, et valent 1 soit `true` pour tout autre valeur, ainsi ;)

```cpp
bool cogito = 1;    // (= true)
bool ergo   = 10;   // (= true = 1)
int  sum = cogito + ergo; // (1 + 1)
std::cout << sum << std::endl; // print: 2
```

=> [plus d'informations](https://www.tutorialspoint.com/cplusplus/cpp_data_types.htm)

## Type `string`

En **C** les chaînes de charactères ne sont manipulables qu'à partir d'un `char*` et de fonctions spécifiques. Même s'il est toujours possible de faire la même chose en ``C++ ``, un nouveau type a été introduit dans ce langage pour faciliter leur manipulation, le type `string` contenu dans le namespace `std`.

```cpp
#include <iostream>
#include <string>

using namespace std;

int main ()
{
   string str1 = "Hello";
   string str2 = "World";
   string str3;

   // copie de str1 dans str3
   str3 = str1;
   cout << "str3 : " << str3 << endl;

   // concatenation de str1 et str2
   str3 = str1 + str2;
   cout << "str1 + str2 : " << str3 << endl;

   // taille totale de str3 après concatenation
   int len = str3.size();
   cout << "str3.size() :  " << len << endl;

   return 0;
}
```

Console output:
```
str3 : Hello
str1 + str2 : HelloWorld
str3.size() :  10
```

### Pointeurs

Les **pointeurs** ne sont pas faciles à appréhender, néanmoins certaines opérations sont plus facilement réalisables grâce à eux et certaines nécéssitent leur emploi comme pour l'allocation dynamique de mémoire.

**Un pointeur est une variable dont la valeur est l'adresse d'une autre variable**, on dit qu'un pointeur pointe vers une variable.

Rappel sur l'utilisation des pointeurs :

```cpp
#include <iostream>

using namespace std;

int main ()
{
    int  var = 20;      // declaration d'une variable de type entier.
    int* ptr = nullptr; // déclaration d'une variable de type "pointeur sur int"

    // note : une bonne pratique en c++ est toujours
    // d'assigner nullptr à un pointeur ne pointant vers aucune variable

    ptr = &var;         // stocke l'adresse de var dans le pointeur

    cout << "Value of var: " << var << endl; // => 20

    // affiche l'address stockée dans le pointer ptr
    cout << "Address stored in ptr variable: " << ptr << endl; // => eg: 0x7fff5fbff768

    // Accède à la valeur stockée à l'adresse contenue dans ke pointeur (par déréférencement)
    cout << "Value of *ptr: " << *ptr << endl; // => 20

    return 0;
}
```

Les **pointeurs** ont aussi une arithmétique particulière, comme le fait de pouvoir être incrémentés ou désincrémentés ([en savoir plus](https://www.tutorialspoint.com/cplusplus/cpp_pointer_arithmatic.htm)).

## Références

Une variable de type **référence** doit être vue comme un **alias**, un nouveau nom se référant à une variable déja existante. Quand une **référence** est initialisée avec une variable, le nom de la variable et le nom de la référence de la variable peuvent tous deux être utilisés pour manipuler la même variable.

```cpp
#include <iostream>
int main()
{
    int  i = 10;
    int& ref_i = i; // déclare une référence de i

    std::cout << "i = " << i << std::endl; // print: "i = 10";
    std::cout << "ref_i = " << ref_i << std::endl; // print: "ref_i = 10";

    i = 20; // on assigne la valeur 20 à la variable i

    std::cout << "i = " << i << std::endl; // print: "i = 20";
    std::cout << "ref_i = " << ref_i << std::endl; // print: "ref_i = 20";

    ref_i = 30; // on assigne la valeur 30 à référence de la variable i

    std::cout << "i = " << i << std::endl; // print: "i = 30";
    std::cout << "ref_i = " << ref_i << std::endl; // print: "ref_i = 30";

    return 0;
}
```

3 différences majeures distinguent les **références** des **pointeurs** :

- **Les références ne peuvent jamais être nulles**. On considère qu'une référence est toujours liée à une zone de mémoire valide.

- **Les références sont toujours constantes**. Une fois qu'elle a été liée à une variable, une référence ne peut pas être changer pour se référer à un autre variable. (les pointeurs peuvent quant à eux être réinitialisés à n'importe quel moment pour pointer sur une autre variable).

- **Une référence doit être initialisée à la création**. (les pointeurs peuvent être initialisés ou réassignés à n'importe quel moment).
