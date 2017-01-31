<p><sup><a href="../">retour</a></sup></p>

# fonctions

## Généralités sur les fonctions

Une fonction est un groupe de déclarations qui représente une tâche ou une opération, elles se décomposent comme ceci :

```
type_de_retour nom_de_la_fonction(liste de parametres)
{
   corps de la fonction
}
```

On distinguera la **définition** d'une fonction de sa **déclaration**.

=> [plus d'informations](http://www.cplusplus.com/doc/tutorial/functions/).

## Passage d'arguments

En C++, les arguments des fonctions peuvent être passés aux fonctions par **valeur**, **référence** ou par **pointeur**.  
Par défaut en C++, les arguments sont passés à une fonction **par valeur**,

*Pour illustrer ces différentes façons de passer des arguments, on va créer une fonction `swap` qui aura pour rôle d'échanger la valeur de deux entiers passés en argument, de façon à ce que le code suivant produise le bon résultat :*

```cpp
#include <iostream>
int main()
{
    int first  = 40;
    int second = 20;

    // before swap: first 40, second 20
    std::cout << "before swap: " << "first " << first << ", second " << second << std::endl;

    // => appel de la fonction swap ici

    std::cout << "after swap: " << "first " << first << ", second " << second << std::endl;
    // after swap: first 20, second 40

    return 0;
}
```

### Passage d'arguments par valeur

Le passage d'argument par **valeur** à une fonction fait une copie de la **valeur** actuelle de la variable qu'elle passe à la fonction. La fonction dispose donc d'une nouvelle variable qui n'a pas d'effet sur la valeur passée en argument.

Le passage d'argument par valeur peut être très utile mais dans le cas de notre fonction `swap`...

```cpp
void swap(int x, int y)
{
    // copie du contenu de la variable x dans une variable temporaire.
    int temp = x;
    x = y;          // copie de y dans x
    y = temp;       // copie de x dans y
}
```

si on passe par valeur les deux entiers à cette fonction `swap(first, second)`, cela produira le résultat suivant :

```
before swap: first 40, second 20
after swap: first 40, second 20
```

Ca n'a pas marché puisque les valeurs transmises à la fonction sont des copies des variables `first` et `second` et non-pas les variables elle-mêmes (l'échange de valeur a bien lieu mais seulement pour les variables `x` et `y`, locales à la fonction `swap`).

Analysons maintenant le comportement de la même fonction mais avec un passage d'arguments par **référence** :

### Passage d'arguments par référence

Le passage d'argument par **référence** à une fonction fait une copie de la **référence** de la variable qu'elle passe à la fonction. À l'intérieur de la fonction, la référence est utilisée pour accéder à la variable initiale passée comme argument à l'appel de cette fonction. Ce qui veut dire que les changements effectués sur les paramètres à l'intérieur de la fonction pourront affecter la variable initiale.

```cpp
void swap(int& x, int& y)
{
    // copie du contenu de la variable x dans une variable temporaire.
    int temp = x;
    x = y;          // copie de y dans x
    y = temp;       // copie de x dans y
}
```

On passe des arguments par **référence** de la même façon qu'on les passe par **valeur**.
si on passe par **référence** les deux entiers à cette fonction `swap(first, second)`, cela produira le résultat suivant :

```
before swap: first 40, second 20
after swap: first 20, second 40
```

Cette fois-ci, on obtient bien le bon résultat !  
C'est bien `first` et `second` que l'on a modifié.

On peut aussi faire la même chose avec des **pointeurs** :

### Passage d'arguments par pointeur

Le passage d'argument par **pointeur** à une fonction fait une copie de l'**adresse** de la variable qu'elle passe à la fonction. À l'intérieur de la fonction on pourra donc soit modifier l'**adresse** de la variable soit sa valeur en **déréférençant** le **pointeur**.

Pour passer une variable par

```cpp
void swap(int* x, int* y)
{
    // copie du contenu de la variable x dans une variable temporaire.
    int temp = *x;
    *x = *y;         // copie de y dans x
    *y = temp;       // copie de x dans y
}
```
si on passe par les arguments par **pointeur**, c'est-à-dire l'**adresse** des deux entiers à cette fonction `swap(&first, &second)`, cela produira le résultat suivant :

```
before swap: first 40, second 20
after swap: first 20, second 40
```

On obtient là aussi le résultat escompté !

### Passage d'arguments constants

Il se peut que l'on n'ait pas modifier le paramètre d'une fonction mais seulement le lire. Dans ce cas, ~~on peut~~ on doit indiquer au compilateur et à l'utilisateur qu'il est constant avec le mot-clef `const`.

```cpp
// passage par valeur constante
void func(const int val);

// passage de valeur constante par référence
void func(int const& ref);

// passage de valeur constante par pointeur
// la fonction ne pourra pas modifier la valeur
void func(int const* ptr);

// passage de valeur constante par pointeur constant
// la fonction ne pourra modifier ni la valeur ni son adresse
void func(int const* const ptr);
```

## Valeurs par défaut

En C++, les paramètres des fonctions peuvent prendre des valeurs par défaut, ils deviennent alors optionnels, ex :

```cpp
void print(std::string const& message, bool is_error = false)
{
    if(is_error) { std::cout << "Error: "; }
    std::cout << message << std::endl;
}

int main()
{
    print("tudo bem !");    // = print("tudo bem !", false);
    print("Outch !", true); // will print "Error: Outch !"
    return 0;
}
```

## Surcharge de fonctions

en C si on veut additionner entre eux deux entier ou deux flottants il faut écrire deux fonctions différentes, ex :

```c
// (c)
int   add_entier(int a, int b)        {return a+b;} // ajoute deux entier
float add_flottant(float a, float b); {return a+b;} // ajoute deux flottants
```

En C++, grâce à la *surcharge de fonction* ou *function overloading* en anglais, on peut écrire :

```cpp
// (c++)
int   add(int a, int b)     {return a+b;} // ajoute deux entier
float add(float a, float b) {return a+b;} // ajoute deux flottants
```

> Attention, le type de valeur de retour d'une fonction ne permet pas à elle seule de différencier deux fonctions, ainsi le code suivant ne compilera pas :

```cpp
int   getValue()  {return 12;}
float getValue()  {return 0.0001;} // ! Compile error
```

> Attention aussi aux ambiguïtés générées par les fonctions avec valeurs par défaut, le compilateur ne sait plus quelle fonction appeler, ex :

```cpp
int add(int a, int b)             {return a+b;}
int add(int a, int b, int c = 10) {return a+b+c;}

int main()
{
    int r1 = add(1, 2, 3);  // ok
    int r2 = add(1, 2);     // error: call to 'add' is ambiguous
}
```

## function template

Les templates sont à la base de la programmation générique, ils permettent d'écrire du code qui ne dépendra d'aucun type en particulier.

En programmation audio on est souvent amené à traiter des *samples* qui sont soit des `float` soit des `double`; pour ne pas avoir à écrire un traitement différent en fonction du type de *sample* à traiter ou pourra employer une seule fonction templatée pour écrire un traitement DSP qui marchera indépendamment du type de *sample* employé.

```cpp
#include <string>

template<typename T>
T add(T const& a, T const& b)
{
    return a + b;
}

int main()
{
    int i1 = 39;
    int i2 = 20;
    int ir = add(i1, i2); // => 59

    float f1 = 13.5;
    float f2 = 20.7;
    float fr = add(f1, f2); // => 34.2

    std::string s1 = "Hello";
    std::string s2 = "World";
    std::string sr = add(s1, s2); // "HelloWorld"
    return 0;
}
```

> Attention l'invocation d'une fonction avec des types différents mais un seul paramètre templaté peut être ambigüe. Si on reprend l'exemple précédent :

```cpp
int main()
{
    float f1 = 13.5;
    double d1 = 0.1;
    //double dr = add(d1, f1); // => Compile error (no matching function)

    // pour lever cette ambiguïté on doit :
    // soit caster le float en double:
    double dr = add(d1, (double)f1);

    // soit définir explicitement le type du template:
    // double dr = add<double>(d1, f1);

    return 0;
}
```
