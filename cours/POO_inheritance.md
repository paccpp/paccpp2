<p><sup><a href="readme.md">retour</a></sup></p>

# Héritage

L'héritage est un concept fondamental en POO qui permet de définir une nouvelle classe en héritant d'une autre classe.    
Quand on crée une classe, à la place de réécrire des variables et fonctions membres plusieurs fois, on peut simplement faire hériter la classe d'une autre classe et ainsi hériter de ses fonctions et variables membres. La nouvelle classe s'appelle une **classe dérivée** et la classe dont elle hérite se nomme la **classe de base**.

L'héritage permet d'exprimer une relation de type "EST UN". Par exemple, on peut dire qu'un *Flanger* EST-UN *Effet*.

Pour exprimer cette relation en C++ on écrira:

```cpp
#include <iostream>

class Effect
{
public:
    Effect() : m_muted(false)
    {
        std::cout << "Effect created" << std::endl;
    };

    bool isMuted() const { return m_muted; }

private:
    bool m_muted;
};

// La classe Flanger hérite de la class Effect
class Flanger : public Effect
{
public:
    Flanger()
    {
        std::cout << "Flanger effect created" << std::endl;
    };
};

int main()
{
    Effect base_fx;         // => "Effect created"
    Flanger flanger_fx;     // => "Flanger effect created"

    // appel de la fonction getMute() sur la classe de base
    std::cout << "base_fx muted: " << base_fx.isMuted() << std::endl;

    // appel de la fonction getMute() de la classe de base à partir de la classe dérivée
    std::cout << "flanger_fx muted: " << flanger_fx.isMuted() << std::endl;
    return 0;
}
```

La classe dérivée `Flanger` hérite de la fonction `getMute()` de la classe de base `Effect`.

## Spécificateur d'accès

Une classe dérivée peut acceder à toute les variables et fonctions **non-privées** de la classe de base dont elle hérite.

Tableau des droits d'accès aux variables et fonctions membres :

| Accès                  |      `public`      |     `protected`    |      `private`     |
|------------------------|:------------------:|:------------------:|:------------------:|
| La même classe         | :white_check_mark: | :white_check_mark: | :white_check_mark: |
| Classe dérivée         | :white_check_mark: | :white_check_mark: |     :no_entry:     |
| Extérieur de la classe | :white_check_mark: |     :no_entry:     |     :no_entry:     |

## Héritage multiple

Une classe peut hériter de plusieurs classes de bases, c'est ce que l'on appelle l'héritage multiple.  
La classe dérivée hérite alors des méthodes et variables membres de chacune des classes de bases.

> Note: Si une fonction avec la même signature existe dans plusieurs classes de bases, la classe dérivée peut utiliser l'opérateur de portée `::` pour appeler explicitement l'une d'entre elle.

ex:

```cpp
#include <iostream>

class A
{
public:
    A() {}

    void foo()
    {
        std::cout << "foo !" << std::endl;
    }

    void sayHello()
    {
        std::cout << "Hello, i'm the A class !" << std::endl;
    }
};

class B
{
public:

    B() {}

    void bar()
    {
        std::cout << "bar !" << std::endl;
    }

    void sayHello()
    {
        std::cout << "Hello, i'm the B class !" << std::endl;
    }
};

// La classe C hérite de la classe A et de la classe B
class C : public A, public B
{
public:

    void sayHello()
    {
        // appel de la fonction sayHello de la classe de base A
        // grâce à l'opérateur de portée ::
        A::sayHello();

        // appel de la fonction sayHello de la classe de base B
        B::sayHello();

        std::cout << "Hello, i'm the C class !" << std::endl;
    }
};

int main()
{
    C c;
    c.foo();
    c.bar();
    c.sayHello();

    return 0;
}
```
