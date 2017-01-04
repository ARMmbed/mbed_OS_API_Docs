# Managing tasks

The task management APIs handle creation and destruction of threads in mbed OS 5, as well as mechanisms for safe interthread communication. Threads are a core component of mbed OS 5 (even your `main` function starts in a thread of its own), so understanding how to work with them is an important part of developing applications for mbed OS 5. Pay particular attention to the [interrupt service routines](rtos.md#interrupt-service-routines) section, which contains important information about how the task management APIs can be used from an interrupt handler. 

* [RTOS](https://docs.mbed.com/docs/mbed-os-api-reference/en/5.3/APIs/tasks/rtos/): The CMSIS-RTOS core.
* [Event Loop](https://docs.mbed.com/docs/mbed-os-api-reference/en/5.3/APIs/tasks/events/): The queue to store events, extract them and excute them later.
* [Ticker](https://docs.mbed.com/docs/mbed-os-api-reference/en/5.3/APIs/tasks/Ticker/): Set up a recurring interrupt.
* [Time](https://docs.mbed.com/docs/mbed-os-api-reference/en/5.3/APIs/tasks/Time/): The C date and time functions.
* [Timeout](https://docs.mbed.com/docs/mbed-os-api-reference/en/5.3/APIs/tasks/TimeOut/): Raise interrupt after certain time.
* [Timer](https://docs.mbed.com/docs/mbed-os-api-reference/en/5.3/APIs/tasks/Timer/): Measuring small times.
* [Wait](https://docs.mbed.com/docs/mbed-os-api-reference/en/5.3/APIs/tasks/wait/): NOP-type wait capabilities.
