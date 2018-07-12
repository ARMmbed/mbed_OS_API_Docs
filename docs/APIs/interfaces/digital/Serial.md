<span class="warnings">**Out of date**: This is not the most recent version of this page. Please see [the most recent version](https://os.mbed.com/docs/latest/reference/serial.html)</span>
# Serial

Serial is a generic protocol used by computers and electronic modules to send and receive control information and data. The Serial link has two unidirection channels, one for sending and one for receiving. The link is asynchronous, and so both ends of the serial link must be configured to use the same settings.

One of the Serial connections goes via the mbed USB port, allowing you to easily communicate with your host PC.

## Hello World!

[![View code](https://www.mbed.com/embed/?url=https://developer.mbed.org/users/mbed_official/code/Serial_HelloWorld_Mbed/)](https://developer.mbed.org/users/mbed_official/code/Serial_HelloWorld_Mbed/file/879aa9d0247b/main.cpp) 

## API

API summary

[![View code](https://www.mbed.com/embed/?type=library)](https://docs.mbed.com/docs/mbed-os-api/en/mbed-os-5.1.0/api/Serial_8h_source.html) 


<span class="notes">**Note**: on a windows machine, you will need to install a USB Serial driver. See [Windows serial configuration](https://docs.mbed.com/docs/mbed-os-handbook/en/5.1/getting_started/what_need/).</span>

Serial channels have a number of configurable parameters:

  * _Baud Rate_ - There are a number of standard baud rates ranging from a few hundred bits per second, to megabits per second. The default setting for a serial connection on the mbed microcontroller is 9600 baud.
  * _Data length_ - Data transferred can be either 7 or 8 bits long. The default setting for a serial connection on the mbed microcontroller is 8 bits.
  * _Parity_ - An optional parity bit can be added. The parity bit will be automatically set to make the number of 1's in the data either odd or even. Parity settings are *Odd*, *Even* or *None*. The default setting for a serial connection on the mbed microcontroller is None.
  * _Stop Bits_ - After data and parity bits have been transmitted, one or two stop bits are inserted to "frame" the data. The default setting for a serial connection on the mbed microcontroller is one stop bit.

The default settings for the mbed microcontroller are described as _9600 8N1_, a  common notation for serial port settings.

## See Also

  * [Serial Port on Wikipedia](http://en.wikipedia.org/wiki/Serial_port)

## Examples

### Example one 

Write a message to a device at a baud rate of 19200

```
#include "mbed.h"

Serial device(p9, p10);  // tx, rx

int main() {
    device.baud(19200);
    device.printf("Hello World\n");
}
```
### Example two

Provide a serial pass-through between the PC and an external UART

```
#include "mbed.h"

Serial pc(USBTX, USBRX); // tx, rx
Serial device(p9, p10);  // tx, rx

int main() {
    while(1) {
        if(pc.readable()) {
            device.putc(pc.getc());
        }
        if(device.readable()) {
            pc.putc(device.getc());
        }
    }
}
```

### Example three

Attach to RX interrupt

```
#include "mbed.h"

DigitalOut led1(LED1);
DigitalOut led2(LED2);

Serial pc(USBTX, USBRX);

void callback() {
    // Note: you need to actually read from the serial to clear the RX interrupt
    printf("%c\n", pc.getc());
    led2 = !led2;
}

int main() {
    pc.attach(&callback;);
    
    while (1) {
        led1 = !led1;
        wait(0.5);
    }
}
```
