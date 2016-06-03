#BusIn

You use the BusIn interface to read a single value from several DigitalIn pins. You can use any of the numbered pins on your board for this.

BusIn is an abstraction that takes any pins and makes them appear as though they are linearly memory mapped for ease of use.  You can then check multiple inputs in a single pass. In general, this abstraction makes your code less cluttered, clearer,  and faster to write.

<span class="tips">**Tip:** Please pay attention to the ordering of pins in the initialization. The order pins are initialized in are the reverse order that bits are OR'd together.</span>


## Hello World!

Combine four pins into a single BusIn.

[![View code](https://www.mbed.com/embed/?url=https://developer.mbed.org/teams/mbed_example/code/BusIn_HelloWorld/)](https://developer.mbed.org/teams/mbed_example/code/BusIn_HelloWorld/file/e1f4151bb580/main.cpp) 

## API

[![View code](https://www.mbed.com/embed/?type=library)](https://developer.mbed.org/users/mbed_official/code/mbed/docs/tip/classmbed_1_1BusIn.html) 

## Interface

The BusIn Interface can use any pin with a blue label, as shown in these pinouts:

<span class="images">![](../Images/pin_out.png)<span>Pinouts showing the numbered pins with blue labels</span></span>


