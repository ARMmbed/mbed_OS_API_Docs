<span class="warnings">**Out of date**: This is not the most recent version of this page. Please see [the most recent version](https://os.mbed.com/docs/latest/reference/timer.html)</span>
# Timer

Use the Timer interface to create, start, stop and read a timer for measuring small times (between microseconds and seconds).

You can independently create, start and stop any number of Timer objects.

## Hello World!

[![View code](https://www.mbed.com/embed/?url=https://developer.mbed.org/users/mbed_official/code/Timer_HelloWorld/)](https://developer.mbed.org/users/mbed_official/code/Timer_HelloWorld/file/27e1de20d3cb/main.cpp) 

## API

API summary

[![View code](https://www.mbed.com/embed/?type=library)](https://docs.mbed.com/docs/mbed-os-api/en/mbed-os-5.1.0/api/classmbed_1_1Timer.html) 

<span class="warnings">**Warning:** Timers are based on 32-bit int microsecond counters, so can only time up to a maximum of 2^31-1 microseconds (30 minutes). They are designed for times between microseconds and seconds. For longer times, you should consider the `time()` real time clock. </span> 
