# Ticker

The Ticker interface is used to set up a recurring interrupt; it calls a function repeatedly and at a specified rate.

Any number of Ticker objects can be created, allowing multiple outstanding interrupts at the same time. The function can be a static function, or a member function of a particular object.

## Hello World!

A simple program to set up a Ticker to repeatedly invert an LED:

[![View code](https://www.mbed.com/embed/?url=https://developer.mbed.org/users/mbed_official/code/Ticker_HelloWorld/)](https://developer.mbed.org/users/mbed_official/code/Ticker_HelloWorld/file/5014bf742e9b/main.cpp) 

## API

API summary

[![View code](https://www.mbed.com/embed/?type=library)](https://docs.mbed.com/docs/mbed-os-api/en/mbed-os-5.2/api/classmbed_1_1Ticker.html) 

### Warnings and notes

* Timers are based on 32-bit int microsecond counters, so can only time up to a maximum of 2^31-1 microseconds (30 minutes). They are designed for times between microseconds and seconds. For longer times, you should consider the time() real time clock. 

* No blocking code in ISR: avoid any call to wait, infinite while loop, or blocking calls in general. 

* No printf, malloc, or new in ISR: avoid any call to bulky library functions. In particular, certain library functions (like printf, malloc and new) are non re-entrant and their behavior could be corrupted when called from an ISR. 

* RTOS Timer: Consider using the [mbed RTOS Timer](Timer.md) instead of a Ticker. In this way your periodic function will not be executed in an ISR, giving you more freedom and safety in your code. </span>

## Examples

Attaching a member function to a ticker: 

[![View code](https://www.mbed.com/embed/?url=https://developer.mbed.org/users/mbed_official/code/Ticker_Example/)](https://developer.mbed.org/users/mbed_official/code/Ticker_Example/file/14eb5da7a9a3/main.cpp) 
