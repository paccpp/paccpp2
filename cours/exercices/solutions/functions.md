<p><sup><a href="../functions.md">exercices</a></sup></p>

# Solution des exercices sur les [fonctions](../functions.md) :

## exercice 01

```cpp
#include <iostream>

template <typename T>
void swap(T& a, T& b)
{
    T temp = a;
    a = b;
    b = temp;
}

int main()
{
    int i1 = 39;
    int i2 = 20;
    swap(i1, i2);
    std::cout << "i1: " << i1 << " et i2: " << i2 << std::endl; // => "i1: 20 et i2: 39"

    float f1 = 13.5;
    float f2 = 20.7;
    swap(f1, f2);
    std::cout << "f1: " << f1 << " et f2: " << f2 << std::endl; // => "f1: 20.7 et f2: 13.5"

    std::string s1 = "Hello";
    std::string s2 = "World";
    swap(s1, s2);
    std::cout << "s1: " << s1 << " et s2: " << s2 << std::endl; // => "s1: World et s2: Hello"
    return 0;
}
```

## exercice 02

```cpp
#include <iostream>

template <typename T>
T clip(T value, T min = -1, T max = 1)
{
    return (value < min) ? min : (value > max) ? max : value;
}

int main()
{
    std::cout << "clip(10, 0, 1): " << clip(10, 0, 1) << std::endl; // => "clip(10, 0, 1): 1"
    std::cout << "clip(-10, 0, 1): " << clip(-10, 0, 1) << std::endl; // => "clip(-10, 0, 1): 0"
    std::cout << "clip(10, 0): " << clip(10, 0) << std::endl; // => "clip(10, 0): 1"

    std::cout << "clip(0.5): " << clip(0.5) << std::endl; // => "clip(0.5): 0.5"
    std::cout << "clip(1.7): " << clip(1.7) << std::endl; // => "clip(1.7): 1."
    std::cout << "clip(1.7, 1., 2.): " << clip(1.7, 1., 2.) << std::endl; // => "clip(1.7, 1., 2.): 1.7"

    // -----

    int low = -10;
    int high = 10;

    // puis décommenter et modifier les appels aux fonctions suivantes afin que le code compile:

    int   i_result = clip<int>(2.8f, low, high);
    std::cout << "i_result: " << i_result << std::endl; // => "i_result: 2"

    float f_result = clip<float>(2.8f, low, high);
    std::cout << "f_result: " << f_result << std::endl; // => "f_result: 2.8"
}
```
