<p><sup><a href="../P00_01.md">retour</a></sup></p>

# Solution des exercices sur la [POO](../POO_01.md) :

## exercice 01

```cpp
#include <iostream>
#include <string>
#include <vector>

class GainEffect
{
public:

    GainEffect() : m_name() {}
    ~GainEffect() = default;

    void setName(std::string const& name)
    {
        m_name = name;
    }

    std::string getName() const
    {
        return m_name;
    }

    void unmute()
    {
        m_muted = false;
    }

    void mute()
    {
        m_muted = true;
    }

    bool muted() const
    {
        return m_muted;
    }

    void setVolume(double volume)
    {
        m_volume = (volume < 0.) ? 0. : volume;
    }

    double getVolume()
    {
        return m_volume;
    }

private:

    std::string m_name {};
    bool        m_muted = false;
    double      m_volume = 0.;
};

int main()
{
    GainEffect fx;

    fx.setVolume(0.5);
    std::cout << "fx volume : " << fx.getVolume() << std::endl;
    // fx volume : 0.5

    fx.setVolume(-10);
    std::cout << "fx volume : " << fx.getVolume() << std::endl;
    // fx volume : 0.

    fx.mute();
    std::cout << "fx muted : " << std::boolalpha << fx.muted() << std::endl;
    // fx muted : true

    fx.unmute();
    std::cout << "fx muted : " << std::boolalpha << fx.muted() << std::endl;
    // fx muted : false

    std::string name = "gain";
    fx.setName(name);
    std::cout << "fx name : " << fx.getName() << std::endl;
    // fx name : gain

    return 0;
}
```

## exercice 02

```cpp
#include <iostream>
#include <string>
#include <vector>

class GainEffect
{
public:

    GainEffect() : m_name() {}
    ~GainEffect() = default;

    void setName(std::string const& name)
    {
        m_name = name;
    }

    std::string getName() const
    {
        return m_name;
    }

    void unmute()
    {
        m_muted = false;
    }

    void mute()
    {
        m_muted = true;
    }

    bool muted() const
    {
        return m_muted;
    }

    void setVolume(double volume)
    {
        m_volume = (volume < 0.) ? 0. : volume;
    }

    double getVolume()
    {
        return m_volume;
    }

    void process(std::vector<double>& samples)
    {
        if(!muted())
        {
            for(auto& sample : samples)
            {
                sample *= m_volume;
            }
        }
    }

private:

    std::string m_name {};
    bool        m_muted = false;
    double      m_volume = 0.;
};

int main()
{
    GainEffect fx;
    fx.setVolume(0.5);

    std::vector<double> buffer {0.1, 0.2, 0.3, 0.4, 0.5, 0.6, 0.7, 0.8, 0.9, 1.};

    // Print buffer
    std::cout << "buffer : ";
    std::copy(buffer.begin(), buffer.end(), std::ostream_iterator<double>(std::cout, " "));
    std::cout << std::endl;
    // print: "buffer : 0.1 0.2 0.3 0.4 0.5 0.6 0.7 0.8 0.9 1"

    fx.process(buffer);

    // Print buffer
    std::cout << "buffer : ";
    std::copy(buffer.begin(), buffer.end(), std::ostream_iterator<double>(std::cout, " "));
    std::cout << std::endl;
    // print: "buffer : 0.05 0.1 0.15 0.2 0.25 0.3 0.35 0.4 0.45 0.5"

    fx.mute();
    fx.process(buffer);

    // Print buffer
    std::cout << "buffer : ";
    std::copy(buffer.begin(), buffer.end(), std::ostream_iterator<double>(std::cout, " "));
    std::cout << std::endl;
    // print: "buffer : 0.05 0.1 0.15 0.2 0.25 0.3 0.35 0.4 0.45 0.5"

    return 0;
}
```
