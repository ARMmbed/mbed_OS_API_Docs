The InterruptIn interface is used to trigger an event when a [digital input pin](DigitalIn) changes.

## Hello World!

[![View code](https://www.mbed.com/embed/?url=https://developer.mbed.org/users/mbed_official/code/InterruptIn_HelloWorld/)](https://developer.mbed.org/users/mbed_official/code/InterruptIn_HelloWorld/file/7a20a6aa1f5e/main.cpp) 

## API

API summary

[![View code](https://www.mbed.com/embed/?type=library)](https://developer.mbed.org/users/mbed_official/code/mbed/docs/tip/classmbed_1_1InterruptIn.html) 

## Interface

<div class="alert-box warning" title="Certain pins cannot be used for InterruptIn">

  * mbed NXP LPC1768: Any of the numbered mbed pins can be used as an InterruptIn, except p19 and p20.
  * mbed FRDM KL25Z: Only the pins of port A and D can be used. (PTA[0-31] and PTD[0-31]). </div>
[![/media/uploads/chris/pinout-thumbnails.jpg](https://developer.mbed.org/media/uploads/chris/pinout-thumbnails.jpg)](https://developer.mbed.org/handbook/Pinouts)  
---  
[See the Pinout page for more details](https://developer.mbed.org/handbook/Pinouts)  
  
The pin input will be logic '0' for any voltage on the pin below 0.8v, and '1' for any voltage above 2.0v. By default, the InterruptIn is setup with an internal pull-down resistor.

<div class="alert-box warning" title="No blocking code in ISR"> In ISR you should avoid any call to wait, infinitive while loop, or blocking calls in general. </div> <div class="alert-box warning" title="No printf, malloc, or new in ISR"> In ISR you should avoid any call to bulky library functions. In particular, certain library functions (like printf, malloc and new) are non re-entrant and their behaviour could be corrupted when called from an ISR. </div>

## Examples

```

#include "mbed.h"

class Counter {
public:
    Counter(PinName pin) : _interrupt(pin) {        // create the InterruptIn on the pin specified to Counter
        _interrupt.rise(this, &Counter;::increment); // attach increment function of this counter instance
    }

    void increment() {
        _count++;
    }

    int read() {
        return _count;
    }

private:
    InterruptIn _interrupt;
    volatile int _count;
};

Counter counter(p5);

int main() {
    while(1) {
        printf("Count so far: %d\n", counter.read());
        wait(2);
    }
}
```

## Related

To read an input, see [DigitalIn](DigitalIn)

For timer-based interrupts, see [Ticker](Ticker) (repeating interrupt) and [Timeout](Timeout) (one-time interrupt)
