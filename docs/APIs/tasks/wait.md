# Wait

```
/** Generic wait functions.
 *
 * These provide simple NOP type wait capabilities.
 *
 * Example:
 * @code
 * #include "mbed.h"
 *
 * DigitalOut heartbeat(LED1);
 *
 * int main() {
 *     while (1) {
 *         heartbeat = 1;
 *         wait(0.5);
 *         heartbeat = 0;
 *         wait(0.5);
 *     }
 * }
 */

/** Waits for a number of seconds, with microsecond resolution (within
 *  the accuracy of single precision floating point).
 *
 *  @param s number of seconds to wait
 */
void wait(float s);

/** Waits a number of milliseconds.
 *
 *  @param ms the whole number of milliseconds to wait
 */
void wait_ms(int ms);

/** Waits a number of microseconds.
 *
 *  @param us the whole number of microseconds to wait
 */
void wait_us(int us);
``` 

<span class="notes">**Note:** OS delay </br>When you call ``wait`` the CPU of your mbed will be busy spinning in a loop waiting for the required time to pass. Using the [mbed RTOS](../RTOS/mbed_RTOS.md) you can make a call to ``Thread::wait`` instead. In this way the OS scheduler will put the current thread in ``waiting state`` and it will allow another thread to execute, or even better, if there are not other threads in ``ready state``, it can put the whole microcontroller to ``sleep`` saving energy. </span> 
