#DigitalIn

The DigitalIn interface is used to read the value of a digital input pin.

Any of the numbered mbed pins can be used as a DigitalIn. 


## API

API summary

[![View code](https://www.mbed.com/embed/?type=library)](https://docs.mbed.com/docs/mbed-os-api/en/mbed-os-5.3/api/classmbed_1_1DigitalIn.html)

## Hello World!

[![View code](https://www.mbed.com/embed/?url=https://developer.mbed.org/users/mbed_official/code/DigitalIn_HelloWorld_Mbed/)](https://developer.mbed.org/users/mbed_official/code/DigitalIn_HelloWorld_Mbed/file/tip/main.cpp) 

## Related API

To handle an interrupt, see [InterruptIn](InterruptIn.md).

Examples of logical functions - boolean logic NOT, AND, OR, XOR:

```
#include "mbed.h"
 
DigitalIn a(p5);
DigitalIn b(p6);
DigitalOut z_not(LED1);
DigitalOut z_and(LED2);
DigitalOut z_or(LED3);
DigitalOut z_xor(LED4);
 
int main() {
    while(1) {
        z_not = !a;
        z_and = a && b;
        z_or = a || b;
        z_xor = a ^ b;
    }
}
```
