<div side style="float: right"> <div class="alert-box info" title="Memory Model">

  1. [mbed Memory Model](https://developer.mbed.org/handbook/Memory-Model)  

  2. **_RTOS Memory Model_**  
</div> </div>

This is a basic overview of the memory model used by the mbed RTOS.

## Threads

Each thread of execution in the RTOS has its separate stack.

When you use the RTOS, before explicitly initializing any additional thread, you will have 4 separate stacks:

  * The stack of the **##Main Thread##** (executing the ##main## function).
  * The **##Idle Thread##** executed each time all the other threads are waiting for external, or scheduled events. This is particularly useful to implement energy saving strategies. (ie ##sleep##).
  * The **##Timer Thread##** that executes all the time scheduled tasks (periodic and non periodic).
  * The stack of **##OS Scheduler##** itself (also used by the ISRs).
    
    
          +-------------------+   Last Address of RAM
          | Scheduler Stack   |
          +-------------------+
          | Main Thread Stack |
          |         |         |
          |         v         |
          +-------------------+
    RAM   |                   |
          |                   |
          +-------------------+
          |         ^         |
          |         |         |
          |       Heap        |
          +-------------------+
          | ZI                |
          +-------------------+
          | ZI: Idle Stack    |
          +-------------------+
          | ZI: Timer Stack   |
          +-------------------+
          | RW                |  
          +===================+  First Address of RAM
          |                   |
    Flash |                   |
