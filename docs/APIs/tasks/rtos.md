<span class="warnings">**Out of date**: This is not the most recent version of this page. Please see [the most recent version](https://os.mbed.com/docs/latest/reference/rtos.html)</span>
# RTOS

## Overview

The mbed RTOS is a C++ wrapper over the Keil RTX code. For more information about Keil RTX, check [the Keil CMSIS-RTOS tutorial](https://www.keil.com/pack/doc/CMSIS/RTX/CMSIS_RTOS_Tutorial.pdf) and [the element14 introduction to Keil RTX](https://www.element14.com/community/docs/DOC-46650/l/arm-keil-rtx-real-time-operating-system-overview). These resources can be used as a general introduction to RTOS principles; it is important to be familiar with the concepts behind an RTOS in order to understand this guide. 

The code of the mbed RTOS can be found in the [mbed-os](https://github.com/ARMmbed/mbed-os) repository, in the [rtos/rtos subdirectory](https://github.com/ARMmbed/mbed-os/tree/master/rtos/rtos).


## Thread

The ``Thread`` class allows defining, creating, and controlling thread functions in the system. 

A ``Thread`` can be in the following states:

* ``Running``: The currently running thread. Only one thread at a time can be in this state.
* ``Ready``: Threads that are ready to run. Once the ``running`` thread has terminated or is ``waiting``, the ``ready`` thread with the highest priority becomes the ``running`` thread.
* ``Waiting``: Threads that are waiting for an event to occur.
* ``Inactive``: Threads that are not created or terminated. These threads typically consume no system resources.

<span class="images">![](Images/Thread/thread_status.png)</span>

### The main() function

The function ``main`` is a special thread function that is started at system initialization and has the initial priority ``osPriorityNormal``; it is the first thread scheduled by the RTOS.

### Thread example

The code below uses two separate threads to blink two LEDs. The first thread is automatically created and executes the `main` function; the second thread is created explicitly inside `main`. 

[![View code](https://www.mbed.com/embed/?url=https://developer.mbed.org/teams/mbed/code/rtos_basic/)](https://developer.mbed.org/teams/mbed/code/rtos_basic/file/209f4db62daf/main.cpp) 


### Thread class reference

[![View code](https://www.mbed.com/embed/?type=library)](https://docs.mbed.com/docs/mbed-os-api/en/mbed-os-5.1.0/api/classrtos_1_1Thread.html) 

<span class="images">![](Images/Thread/thread_priority.png)</span>

## Mutex

A ``Mutex`` is used to synchronize the execution of threads, for example to protect the access to a shared resource.

<span class="warnings"> **Warning:** ISR</br>The ``Mutex`` methods cannot be called from interrupt service routines (ISR). In the current version of mbed OS, if you attempt to use a mutex from within an ISR nothing happens; attempts to lock a mutex succeed immediately, regardless of whether the lock is actually free. In other words, if you acquire a mutex lock in an ISR, you can break the thread safety mechanisms and introduce race-conditions into an otherwise safe piece of code Future versions of mbed OS will provide warnings and ultimately prevent this from happening. </span>

<span class="images">![](Images/Thread/Mutex.png)</span>

### Mutex example

Use Mutex to protect printf().

[![View code](https://www.mbed.com/embed/?url=https://developer.mbed.org/teams/mbed/code/rtos_mutex/)](https://developer.mbed.org/teams/mbed/code/rtos_mutex/file/192fef923dbc/main.cpp) 

<span class="notes">**Note:** C standard library Mutexes</br>The ARM C standard library already has Mutexes in place to protect the access to ``stdio``, so on the LPC1768 the above example is not necessary. On the other hand, the LPC11U24 does not provide default ``stdio`` Mutexes, making the above example a necessity. </span> 
<span class="warnings">**Warning:** ``stdio``, ``malloc`` and ``new`` in ISR</br>Because of the mutexes in the ARM C standard library you cannot use ``stdio`` (``printf``, ``putc``, ``getc`` and so on), ``malloc`` and ``new`` in ISR. </span> 

### Mutex class reference

[![View code](https://www.mbed.com/embed/?type=library)](https://docs.mbed.com/docs/mbed-os-api/en/mbed-os-5.1.0/api/classrtos_1_1Mutex.html) 

## Semaphore

A ``Semaphore`` manages thread access to a pool of shared resources of a certain type.

<span class="images">![](Images/Thread/Semaphore.png)</span>

### Semaphore example

Use Semaphore to protect printf().

[![View code](https://www.mbed.com/embed/?url=https://developer.mbed.org/teams/mbed/code/rtos_semaphore/)](https://developer.mbed.org/teams/mbed/code/rtos_semaphore/file/02e47d35a686/main.cpp) 

### Semaphore class reference

[![View code](https://www.mbed.com/embed/?type=library)](https://docs.mbed.com/docs/mbed-os-api/en/mbed-os-5.1.0/api/classrtos_1_1Semaphore.html) 

## Signals

Each ``Thread`` can wait for signals and be notified of events:

[![View code](https://www.mbed.com/embed/?url=https://developer.mbed.org/teams/mbed/code/rtos_signals/)](https://developer.mbed.org/teams/mbed/code/rtos_signals/file/c133402c77cb/main.cpp) 

## Queue and MemoryPool

### Queue

A ``Queue`` allows you to queue pointers to data from producer threads to consumer threads:

<span class="images">![](Images/Thread/queue.png)</span>

```
Queue queue;

message_t *message;

queue.put(message);

osEvent evt = queue.get();
if (evt.status == osEventMessage) {
    message_t *message = (message_t*)evt.value.p;
``` 

### Queue class reference

[![View code](https://www.mbed.com/embed/?type=library)](https://docs.mbed.com/docs/mbed-os-api/en/mbed-os-5.1.0/api/classrtos_1_1Queue.html) 

### MemoryPool

The MemoryPool class is used to define and manage fixed-size memory pools:

```
MemoryPool mpool;

message_t *message = mpool.alloc();

mpool.free(message);
``` 

### MemoryPool class reference

[![View code](https://www.mbed.com/embed/?type=library)](https://docs.mbed.com/docs/mbed-os-api/en/mbed-os-5.1.0/api/classrtos_1_1MemoryPool.html) 

### Queue and MemoryPool example

Shows ``Queue`` and ``MemoryPool`` (see below) managing measurements. 

[![View code](https://www.mbed.com/embed/?url=https://developer.mbed.org/teams/mbed/code/rtos_queue/)](https://developer.mbed.org/teams/mbed/code/rtos_queue/file/c980920b5e22/main.cpp) 

## Mail

``Mail`` works like a queue, with the added benefit of providing a memory pool for allocating messages (not only pointers).

<span class="images">![](Images/Thread/mail_queue.png)</span>

### Mail example

Using ``mail`` to manage measurement.

[![View code](https://www.mbed.com/embed/?url=https://developer.mbed.org/teams/mbed/code/rtos_mail/)](https://developer.mbed.org/teams/mbed/code/rtos_mail/file/a3428581e64c/main.cpp) 

### Mail class reference

[![View code](https://www.mbed.com/embed/?type=library)](https://docs.mbed.com/docs/mbed-os-api/en/mbed-os-5.1.0/api/classrtos_1_1Mail.html) 

## RtosTimer

You use the ``RtosTimer`` class to create and and control timer functions in the system. A timer function is called when a time period expires, so both one-shot and periodic timers are possible. A timer can be started, restarted, or stopped. 

Timers are handled in the thread ``osTimerThread``. Callback functions run under the control of this thread and may use CMSIS-RTOS API calls.

<span class="images">![](Images/Thread/rtos_timer.png)</span>

### RtosTimer example

Control the timing of four LEDs.

[![View code](https://www.mbed.com/embed/?url=https://developer.mbed.org/teams/mbed/code/rtos_timer/)](https://developer.mbed.org/teams/mbed/code/rtos_timer/file/9dd7d49be2c3/main.cpp) 

### RtosTimer class reference

[![View code](https://www.mbed.com/embed/?type=library)](https://docs.mbed.com/docs/mbed-os-api/en/mbed-os-5.1.0/api/classrtos_1_1RtosTimer.html) 

## Interrupt Service Routines

The same RTOS API can be used in ISR. The only two warnings are:

* You cannot use ``Mutex``.
* Wait in ISR is not allowed; all the timeouts in method parameters have to be set to 0.

### ISR example

Using a message from the queue to trigger an interrupt.

[![View code](https://www.mbed.com/embed/?url=https://developer.mbed.org/teams/mbed/code/rtos_isr/)](https://developer.mbed.org/teams/mbed/code/rtos_isr/file/b3d67501ac55/main.cpp) 

## Default Timeouts

The mbed RTOS API has made the choice of defaulting to ``0`` timeout (no wait) for the producer methods, and ``osWaitForever`` (infinite wait) for the consumer methods.

A typical scenario for a producer could be a peripheral triggering an interrupt to notify an event; in the corresponding interrupt service routine you cannot wait (this would deadlock the entire system). On the other side, the consumer could be a background thread waiting for events; in this case the desired default behaviour is not using CPU cycles until this event is produced, hence the ``osWaitForever``.

<span class="warnings">**Warning**: No wait in ISR </br>When calling an RTOS object method in an ISR all the time out parameters have to be set to 0 (no wait); waiting in ISR is not allowed. </span>

## Status and error codes

The CMSIS-RTOS functions will return the following statuses:

* ``osOK``: function completed; no event occurred.
* ``osEventSignal``: function completed; signal event occurred.
* ``osEventMessage``: function completed; message event occurred.
* ``osEventMail``: function completed; mail event occurred.
* ``osEventTimeout``: function completed; timeout occurred.
* ``osErrorParameter``: a mandatory parameter was missing or specified an incorrect object.
* ``osErrorResource``: a specified resource was not available.
* ``osErrorTimeoutResource``:  a specified resource was not available within the timeout period.
* ``osErrorISR``: the function cannot be called from interrupt service routines (ISR).
* ``osErrorISRRecursive``: function called multiple times from ISR with same object.
* ``osErrorPriority``: system cannot determine priority or thread has illegal priority.
* ``osErrorNoMemory``: system is out of memory; it was impossible to allocate or reserve memory for the operation.
* ``osErrorValue``: value of a parameter is out of range.
* ``osErrorOS``: unspecified RTOS error - run-time error but no other error message fits.

## API - RTOS header

[![View code](https://www.mbed.com/embed/?type=library)](https://docs.mbed.com/docs/mbed-os-api/en/mbed-os-5.1.0/api/rtos_8h_source.html) 
