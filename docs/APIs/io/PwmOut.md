<span class="warnings">**Out of date**: This is not the most recent version of this page. Please see [the most recent version](https://os.mbed.com/docs/latest/reference/pwmout.html)</span>
# PwmOut

The PwmOut interface is used to control the frequency and mark-space ratio of a digital pulse train.

## Hello World!

This code example uses the default period of 0.020s and ramps the duty cycle from 0% to 100% in increments of 10%. 


[![View code](https://www.mbed.com/embed/?url=https://developer.mbed.org/teams/mbed_example/code/PwmOut_HelloWorld/)](https://developer.mbed.org/teams/mbed_example/code/PwmOut_HelloWorld/file/5160ea45399b/main.cpp) 


## API

API summary

[![View code](https://www.mbed.com/embed/?type=library)](https://docs.mbed.com/docs/mbed-os-api/en/mbed-os-5.1.0/api/PwmOut_8h_source.html) 

## Details

The default period is 0.020s, and the default pulse width is 0.

The PwmOut interface can express the pulse train in many ways to fit different use cases. The period and pulse width can be expressed directly in units of seconds, millisecond or microseconds. The pulse width can also be expressed as a percentage of the the period.
  
## Code Examples

### Example one

This code example sets the period in seconds and the duty cycle as a percentage of the period in floating point (range: 0 to 1). The effect of this code snippet will be to blink LED2 over a 4 second cycle, 50% on, for a pattern of 2 seconds on, 2 seconds off.

```
#include "mbed.h"

PwmOut led(LED2);

int main() {
    // specify period first, then everything else
    led.period(4.0f);  // 4 second period
    led.write(0.50f);  // 50% duty cycle
    while(1);          // led flashing
}
```   

### Example two

The following example does the same thing, but instead of specifying the duty cycle as a relative percentage of the period it specifies it as an absolute value in seconds. In this case we have a four-second period and a two-second duty cycle, meaning the LED will be on for two seconds and off for two seconds. 

```
#include "mbed.h"

PwmOut led(LED2);

int main() {
    // specify period first, then everything else
    led.period(4.0f);  // 4 second period
    led.pulsewidth(2); // 2 second pulse (on)
    while(1);          // led flashing
}

```

### Example three

This code example is for an RC Servo. In RC Servos you set the position based on the PWM signal's duty cycle or pulse width. This example code uses a period of 0.020s and increases the pulse width by 0.0001s on each pass. This will cause an increase of .5% of the servo's range every .25s. In effect the servo will move 2% of its range per second, meaning after 50 seconds the servo will have gone from 0% to 100% of its range. 

```

#include "mbed.h"

PwmOut servo(p21);

int main() {
    servo.period(0.020);          // servo requires a 20ms period
    while (1) {
        for(float offset=0.0; offset<0.001; offset+=0.0001) {
            servo.pulsewidth(0.001 + offset); // servo position determined by a pulsewidth between 1-2ms
            wait(0.25);
        }
    }
}
```
