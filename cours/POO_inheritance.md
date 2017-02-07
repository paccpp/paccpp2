<p><sup><a href="readme.md">retour</a></sup></p>

# Héritage

L'héritage est un concept fondamental en POO qui permet de définir une nouvelle classe en héritant d'une autre classe.    
Quand on crée une classe, à la place de réécrire des variables et fonctions membres plusieurs fois, on peut simplement faire hériter la classe d'une autre classe et ainsi hériter de ses fonctions et variables membres. La nouvelle classe s'appelle une **classe dérivée** et la classe dont elle hérite se nomme la **classe de base**.

L'héritage permet d'exprimer une relation de type "EST UN". Par exemple, on peut dire qu'un *Flanger* EST-UN *Effet*.

Pour exprimer cette relations en C++ on écrira:

```cpp
#include <iostream>

class Effect
{
public:
    Effect() : m_muted(false)
    {
        std::cout << "Effect created" << std::endl;
    };

    bool isMuted() { return m_muted; }

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
