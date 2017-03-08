<p><sup><a href="../P00_02.md">retour</a></sup></p>

# Solution des exercices sur la [POO](../POO_02.md) :

## exercice 01

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

class Phasor : public Processor
{
public:

    Phasor(double freq, double samplerate) :
    Processor(samplerate),
    m_phase(0.),
    m_freq(freq)
    {
        computeIncrement();
    }

    Phasor(Phasor const& other) :
    Processor(other.getSampleRate()),
    m_phase(other.m_phase),
    m_freq(other.m_freq),
    m_phase_inc(other.m_phase_inc)
    {

    }

    void setPhase(double phase)
    {
        while (phase >= 1.) phase -= 1.;
        while (phase < 0.) phase += 1.;
        m_phase = phase;
    }

    double getPhase() const
    {
        return m_phase;
    }

    void setFrequency(double freq)
    {
        m_freq = freq;
        computeIncrement();
    }

    double getFrequency() const
    {
        return m_freq;
    }

    double process()
    {
        double out = m_phase;

        // wrap phase between 0. and 1.
        if((m_phase += m_phase_inc) >= 1.)
        {
            m_phase -= 1.;
        }

        return out;
    }

private:

    void computeIncrement()
    {
        m_phase_inc = m_freq / getSampleRate();
    }

    double m_phase;
    double m_freq;
    double m_phase_inc;
};

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
