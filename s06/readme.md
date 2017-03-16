# Séance 06

<p><sup><a href="../s05">précédente</a> | <a href="../s07">suivante</a></sup></p>

## Programmation d'objets Max/Pd en `C++` (01)

Vous pouvez utiliser les répertoires suivants :
 - [MaxStarterKit](https://github.com/paccpp/MaxStarterKit)
 - [PdStarterKit](https://github.com/paccpp/PdStarterKit)

RDV sur le [wiki de paccpp](https://github.com/paccpp/paccpp/wiki) pour un rappel sur la structure des objets Max et Pd.

Pour écrire un nouvel objet pour Max ou Pd en language `C` ou `C++`, suivez les instructions du fichier `readme.md` de l'un des répertoires ci-dessus.  

Une fois que vous avez [cloné](https://help.github.com/articles/cloning-a-repository/) le répertoire, le plus simple pour ajouter un nouvel objet est de dupliquer un projet existant et de changer le nom de l'objet, puis de relancer la commande de génération de projet pour que les modifications puissent être prises en compte.

## Spécificités de la programmation d'objets Max/Pd en `C++`

Pour les **objets Pd** vous devrez envelopper la déclaration de la fonction setup/main au sein d'un `extern "C"` pour que votre objet puisse être instantié par PureData correctement, ex:

```cpp
// Note in c++ you need to wrap the setup method in an extern "C" statement.
extern "C"
{
    extern void setup_pa0x2eobjectpp_tilde(void)
    {
        t_class* c = class_new(gensym("pa.objectpp~"),
                               (t_newmethod)pa_objectpp_tilde_new, (t_method)pa_objectpp_tilde_free,
                               sizeof(t_pa_objectpp_tilde), CLASS_DEFAULT, A_GIMME, 0);
        if(c)
        {
            CLASS_MAINSIGNALIN(c, t_pa_objectpp_tilde, m_f);
            class_addmethod(c, (t_method)pa_objectpp_tilde_dsp, gensym("dsp"), A_CANT);
        }

        pa_objectpp_tilde_class = c;
    }
}
```

## Se servir d'une classe `C++` dans un objet Max/Pd

Le plus souvent on écrira nos différentes classes `C++` au sein d'un fichier spécifique.  
Supposons que l'on veuille créer un objet `Phasor`, on écrira alors une classe `Phasor` qui contiendra toute la logique de notre objet dans un fichier nommé `Phasor.hpp`.

Dans le code de l'objet Max/Pd on pourra inclure ce fichier pour utiliser cette classe `Phasor` en tapant `#include "Phasor.hpp"`

Dans la structure de l'objet on stocke un pointeur de Phasor, ex:

```cpp
typedef struct _pa_phasorpp_tilde
{
    t_object    m_obj; // pd object - always placed in first in the object's struct

    Phasor*     m_phasor;

    t_outlet*   m_out;
    t_float     m_f;

} t_pa_phasorpp_tilde;
```

Ce pointeur de `Phasor` peut être initialisé dans la [fonction `setup/main`](https://github.com/paccpp/paccpp/wiki/Anatomie-d'un-objet-PureData#fonction-mainsetup) en faisant appel à l'operateur `new` du C++:

```cpp
static void* pa_phasorpp_tilde_new(t_symbol* s, int argc, t_atom *argv)
{
    t_pa_phasorpp_tilde *x = (t_pa_phasorpp_tilde*)pd_new(pa_phasorpp_tilde_class);
    if(x)
    {
        // on construit un objet de type phasor
        x->m_phasor = new Phasor();

        x->m_phasor->setFrequency(1000);

        x->m_out = outlet_new((t_object *)x, &s_signal);
    }

    return (x);
}
```

Tous les objets créés avec `new` doivent être détruits quand ils ne sont plus utilisés à l'aide du mot-clef `delete`, dans les objets Max/Pd on le fera souvent dans la [fonction `free`](https://github.com/paccpp/paccpp/wiki/Anatomie-d'un-objet-PureData#fonction-free)
```cpp
static void pa_phasorpp_tilde_free(t_pa_phasorpp_tilde* x)
{
    outlet_free(x->m_out);

    // on supprimme la mémoire allouée pour l'objet Phasor
    delete x->m_phasor;
}
```
