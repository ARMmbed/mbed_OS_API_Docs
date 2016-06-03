# BusOut

You use the BusOut interface to write a single value to a number of [DigitalOut](DigitalOut.md) pins, creating an artificial bus. You can use any Digitalin pins, and they do not have to have sequential hardware addressing. 

<span class="tips">**Tip:** When initializing a BusOut object, you initialize pins in their bit order from right to left. Be careful with this, as the bit order is the reverse of the object initializing order:
</br>
```
// init object
BusOut thingy(pin0,pin1,...,pin15)
 
// use object
thingy = (val15<<15 | ... | val1 <<1 | val0 <<0) ; // where valx corresponds to the value to be put onto pinx
```
</span>


## Hello World!

This example program counts up from 0 to 15 or binary 0000 to 1111. This turns LEDs 1 to 4 on or off, according to the number going across the bus.  If the board has a single, tricolor LED, then the LED will change colors instead of going on and off.

[![View code](https://www.mbed.com/embed/?url=https://developer.mbed.org/teams/mbed_example/code/BusOut_HelloWorld/)](https://developer.mbed.org/teams/mbed_example/code/BusOut_HelloWorld/file/f979089a5ca0/main.cpp) 

The values are:

| Integer | Binary | LED on(1) / off(0)          |
|---------|--------|-----------------------------|
| 0       | 0000   | LED4=0 LED3=0 LED2=0 LED1=0 |
| 1       | 0001   | LED4=0 LED3=0 LED2=0 LED1=1 |
 
 And so on until 15.

## API

[![View code](https://www.mbed.com/embed/?type=library)](https://developer.mbed.org/users/mbed_official/code/mbed/docs/tip/classmbed_1_1BusOut.html) 

## Interface

You can use any pin with a blue label, and the on-board LEDs (LED1-LED4).

<span class="images">![](../Images/pin_out.png)</span> 

You can use the interface to both set the state of the output pin and read back the current output state. 

Set the BusOut to 0 to turn it off, or 1 to turn it on.
