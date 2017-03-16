<p><sup><a href="../readme.md">retour</a></sup></p>

# Exercices sur la [POO](../POO_concepts.md) :

## Exercice 01

Compléter le code suivant afin qu'il compile correctement et que les instructions de la fonction `main` produisent les bons résultats en sortie.  

Vous devrez créer une classe nommée `Phasor` qui [hérite](../POO_inheritance.md) de la classe `Processor`.
La classe `Phasor` devra avoir :
 - Un constructeur qui initialise la fréquence initiale du phasor ainsi que le sampling rate.
 - Une méthode publique qui permette de changer la phase nommée `setPhase` (faites en sorte dans cette méthode que la phase soit maintenue entre 0. et 1., une phase à 1.25 doit par exemple être equivalente à une phase à 0.25).
 - Une méthode publique pour demander la phase courante nommée `getPhase`.
 - Une méthode publique pour changer la fréquence du Phasor et une autre pour demander la fréquence courante nommées respectivement `setFrequency` et `getFrequency`.
 - Une méthode privée qui permette de calculer l'incrément de phase nommée `computeIncrement`.
 - Une méthode publique nommée `process` qui incrémente la phase du phasor et renvoit un `double` correspondant à la phase courante du Phasor.

Dans un second temps vous [surchargerez le constructeur](../POO_object_construction_and_destruction.md#surcharge-de-constructeur) de la classe `Phasor` en ajoutant un [constructeur par copie](../POO_object_construction_and_destruction.md#constructeur-par-copie) afin que l'on puisse construire un objet phasor avec un autre objet phasor (le nouvel objet phasor devra avoir la même phase, la même frequence et le même *sampling rate* que le phasor avec lequel il a été construit).


```cpp
#include <iostream>

class Processor
{

public: // methods

    //! @brief Constructor
    Processor(double samplerate) : m_samplerate(samplerate) {}

    //! @brief Copy constructor
    Processor(Processor const& other) : m_samplerate(other.m_samplerate) {}

    //! @brief Default destructor
    ~Processor() = default;

    void setSampleRate(double samplerate) { m_samplerate = samplerate; }

    double getSampleRate() const { return m_samplerate; }

private: // variables

    double m_samplerate;
};

// --------------------------------

// implémenter une classe Phasor ici

// --------------------------------

int main()
{
    // 1) ------------------------------------------------------- //

    // create a Phasor object with a default frequency and samplerate
    Phasor phasor(1000., 44100.);

    std::cout << "phasor: " << phasor.process() << std::endl;
    // print "phasor: 0."

    std::cout << "phasor: " << phasor.process() << std::endl;
    // print "phasor: 0.0226757"

    std::cout << "phasor: " << phasor.process() << std::endl;
    // print "phasor: 0.0453515"

    std::cout << "phasor: " << phasor.process() << std::endl;
    // print "phasor: 0.0680272"

    phasor.setFrequency(440.);
    phasor.setPhase(-0.25);

    std::cout << "phasor: " << phasor.getPhase() << std::endl;
    // print "phasor: 0.75"

    for(int i = 0; i < 256; ++i)
    {
        phasor.process();
    }

    std::cout << "phasor: " << phasor.getPhase() << std::endl;
    // print "phasor: 0.304195"

    // 2) ------------------------------------------------------- //

    // Create a new Phasor by copying another phasor (via a copy constructor)
    Phasor phasor2(phasor);

    std::cout << "phasor2: " << phasor2.process() << std::endl;
    // print "phasor2: 0.304195"

    std::cout << "phasor2: " << phasor2.process() << std::endl;
    // print "phasor2: 0.314172"

    return 0;
}
```

---

:eyes: => [solution](solutions/POO_02.md#exercice-01)

---

## Exercice 02

Dans ce second exercice on va utiliser le concept de [polymorphisme](../POO_polymorphism.md) pour mettre à jour et adapter nos classes `Processor` et `Phasor` écrites lors de l'[exercice 01](#exercice-01).  
La classe `Processor` permet de changer la fréquence d'échantillonnage. La classe `Phasor` quant à elle dépend de cette fréquence d'échantillonnage pour calculer son incrément de phase, or elle n'est pas en mesure de se mettre automatiquement à jour quand la valeur de la fréquence d'échantillonnage change.

Pour remédier à cela il vous faudra:

 - Implémenter une **méthode virtuelle** au sein de la classe `Processor` nommée `sampleRateChanged`.
 - Vous ferez appel à cette méthode au sein de la fonction `setSampleRate` de la classe `Processor`.
 - Vous devrez ensuite spécialiser la méthode `sampleRateChanged` dans la classe `Phasor` pour mettre à jour la valeur d'incrément de phase.
 - Profitez-en enfin pour faire en sorte que la méthode `process` devienne une **methode virtuelle pure** de la classe `Processor`.

 Gardez les classes `Processor` et `Phasor` de l'exercice au-dessus, apportez-leurs les modifications nécéssaires, puis remplacez la fonction `main` de l'exercice 01 par la fonction `main` suivante afin de vérifier que vos modifications permettent de faire compiler le programme et d'obtenir les bonnes valeurs à l'exécution:

 ```cpp
 int main()
{
    // create a Phasor object with a default frequency and samplerate
    Phasor phasor(440., 96000);

    // modify sample rate here :
    phasor.setSampleRate(44100.);

    // To show that we can call process on the processor base class
    Processor& processor = phasor;

    for(int i = 0; i < 256; ++i)
    {
        processor.process();
    }

    std::cout << "phasor: " << phasor.getPhase() << std::endl;
    // print "phasor: 0.554195"

    return 0;
}
 ```

---

:eyes: => [solution](solutions/POO_02.md#exercice-02)
