# TimeOut

Use the Timeout interface to set up an interrupt to call a function after a specified delay.

Any number of Timeout objects can be created, allowing multiple outstanding interrupts at the same time.

## Hello World!

Set up a Timeout to invert an LED after a given timeout:

[![View code](https://www.mbed.com/embed/?url=https://developer.mbed.org/users/mbed_official/code/Timeout_HelloWorld/)](https://developer.mbed.org/users/mbed_official/code/Timeout_HelloWorld/file/8a555873b7d3/main.cpp) 

## API

API summary

[![View code](https://www.mbed.com/embed/?type=library)](https://docs.mbed.com/docs/mbed-os-api/en/mbed-os-5.2/api/classmbed_1_1Timeout.html) 

## Warnings and notes

* Timers are based on 32-bit int microsecond counters, so can only time up to a maximum of 2^31-1 microseconds (30 minutes). They are designed for times between microseconds and seconds. For longer times, you should consider the time() real time clock. 

* No blocking code in ISR: avoid any call to wait, infinite while loop, or blocking calls in general. 

* No printf, malloc, or new in ISR: avoid any call to bulky library functions. In particular, certain library functions (like printf, malloc and new) are non re-entrant and their behavior could be corrupted when called from an ISR. 

* RTOS Timer: consider using the [mbed RTOS Timer](https://developer.mbed.org/handbook/RTOS) instead of a Timeout. Your callback function will not be executed in an ISR, giving you more freedom and safety in your code.

## Examples

Attaching a member function:

[![View code](https://www.mbed.com/embed/?url=https://developer.mbed.org/users/mbed_official/code/Timeout_Example/)](https://developer.mbed.org/users/mbed_official/code/Timeout_Example/file/f24282a2495f/main.cpp) 
