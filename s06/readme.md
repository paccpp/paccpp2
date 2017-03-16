# Séance 06

<p><sup><a href="../s05">précédente</a> | <a href="../s07">suivante</a></sup></p>

## Programmation d'objets Max/Pd en `C++` (01)

Vous pouvez utiliser les répertoires suivants :
 - [MaxStarterKit](https://github.com/paccpp/MaxStarterKit)
 - [PdStarterKit](https://github.com/paccpp/PdStarterKit)

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
