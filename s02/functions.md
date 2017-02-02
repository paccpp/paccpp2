<p><sup><a href="readme.md">retour</a></sup></p>

# fonctions (02) => ([01](../s01/functions))

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
