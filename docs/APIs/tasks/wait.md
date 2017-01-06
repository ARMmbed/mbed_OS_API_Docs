# Wait

Wait functions provide simple wait capabilities. The OS scheduler will put the current thread in ``waiting state``, allowing another thread to execute. Even better: if there are not other threads in ``ready state``, it can put the whole microcontroller to ``sleep``, saving energy.

```
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

## Example

```

 #include "mbed.h"

 DigitalOut heartbeat(LED1);

 int main() {
     while (1) {
         heartbeat = 1;
         wait(0.5);
         heartbeat = 0;
         wait(0.5);
     }
 }
```
