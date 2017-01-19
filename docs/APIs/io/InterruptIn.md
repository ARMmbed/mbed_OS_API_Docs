# InterruptIn

Use the InterruptIn interface to trigger an event when a [digital input pin](DigitalIn.md) changes.

## API

[![View code](https://www.mbed.com/embed/?type=library)](https://docs.mbed.com/docs/mbed-os-api/en/mbed-os-5.3/api/InterruptIn_8h_source.html) 

**Warnings:**

* No blocking code in ISR: avoid any call to wait, infinite while loop or blocking calls in general.

* No printf, malloc or new in ISR: avoid any call to bulky library functions. In particular, certain library functions (such as printf, malloc and new) are non re-entrant, and their behavior could be corrupted when called from an ISR.

## Hello World!

[![View code](https://developer.mbed.org/teams/mbed_example/code/InterruptIn_HelloWorld/)](https://developer.mbed.org/teams/mbed_example/code/InterruptIn_HelloWorld/file/f729f0421740/main.cpp) 

## Example

Try the following example to count rising edges on a pin.

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
