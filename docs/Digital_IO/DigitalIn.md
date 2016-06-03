#DigitalIn

You use the DigitalIn interface to read the value of a digital input pin such as a button (possible values: 0 and 1). You can use any of the numbered mbed pins for this. 

## Hello World!

### Generic Hello World

Press a button to turn the LED on.

[![View code](https://www.mbed.com/embed/?url=https://developer.mbed.org/teams/mbed_example/code/DigitalIn_HelloWorld/)](https://developer.mbed.org/teams/mbed_example/code/DigitalIn_HelloWorld/file/ef3558374977/main.cpp) 

### Hello World for the FRDM-KL25Z board

Press a button to turn the LED on.

[![View code](https://www.mbed.com/embed/?url=https://developer.mbed.org/teams/mbed/code/DigitalIn_HelloWorld_FRDM-KL25Z/)](https://developer.mbed.org/teams/mbed/code/DigitalIn_HelloWorld_FRDM-KL25Z/file/950f06e55c90/main.cpp) 

## Examples

Examples of logical functions

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

## API

[![View code](https://www.mbed.com/embed/?type=library)](https://developer.mbed.org/users/mbed_official/code/mbed/docs/tip/classmbed_1_1DigitalIn.html) 

## Interface

The DigitalIn Interface can be used on any pin with a blue label.

The pin input is logic '0' for any voltage on the pin below 0.8v, and '1' for any voltage above 2.0v. By default, the DigitalIn is set up with an internal pull-down resistor.

<span class="images">![](../Images/pin_out.png)<span>Any pin with a blue label can work as a DigitalIn</span></span>

## Related

To handle an interrupt, see [InterruptIn](InterruptIn.md).


