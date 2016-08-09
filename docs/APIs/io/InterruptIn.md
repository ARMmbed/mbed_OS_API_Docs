# InterruptIn

The InterruptIn interface is used to trigger an event when a [digital input pin](DigitalIn.md) changes.

## Hello World!

[![View code](https://www.mbed.com/embed/?url=https://developer.mbed.org/users/mbed_official/code/InterruptIn_HelloWorld/)](https://developer.mbed.org/users/mbed_official/code/InterruptIn_HelloWorld/file/7a20a6aa1f5e/main.cpp) 

## API

API summary

[![View code](https://www.mbed.com/embed/?type=library)](https://docs.mbed.com/docs/mbed-os-api/en/mbed-os-5.1.0/api/InterruptIn_8h_source.html) 

**Warnings:**

* No blocking code in ISR: avoid any call to wait, infinite while loop, or blocking calls in general.

* No printf, malloc, or new in ISR: avoid any call to bulky library functions. In particular, certain library functions (like printf, malloc and new) are non re-entrant and their behavior could be corrupted when called from an ISR.

## Example

An example class for counting rising edges on a pin

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

To read an input, see [DigitalIn](DigitalIn.md).

For timer-based interrupts, see [Ticker](../tasks/Ticker.md) (repeating interrupt) and [Timeout](../tasks/TimeOut.md) (one-time interrupt).
