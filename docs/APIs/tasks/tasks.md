# Managing tasks

The task management APIs handle creation and destruction of threads in mbed OS 5, as well as mechanisms for safe inter-thread communication. Threads are a core component of mbed OS 5 (even your `main` function starts in a thread of its own), so understanding how to work with them is a very important part of developing application for mbed OS 5. Pay particular attention to the [interrupt service routines](rtos.md#interrupt-service-routines) section, which contains important information about how the task management APIs can be used from an interrupt handler. 

* [RTOS](rtos.md): The CMSIS-RTOS core.
* [Event Loop](events.md): The queue to store events, extract&excute them later on.
* [Ticker](Ticker.md): Set up a recurring interrupt.
* [Time](Time.md): The C date and time functions.
* [Timeout](TimeOut.md): Raise interrupt after certain time.
* [Timer](Timer.md): Measuring small times.
* [Wait](TimeOut.md): Simple NOP-type wait capabilities.
