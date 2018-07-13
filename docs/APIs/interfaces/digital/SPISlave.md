<span class="warnings">**Out of date**: This is not the most recent version of this page. Please see [the most recent version](https://os.mbed.com/docs/latest/reference/spislave.html)</span>
# SPISlave

Use the SPISlave class to communicate with an SPI master device.

The default format is set to 8-bits, mode 0, and a clock frequency of 1MHz. Synchronization level: not protected.

## API


[![View code](https://www.mbed.com/embed/?type=library)](https://docs.mbed.com/docs/mbed-os-api/en/mbed-os-5.2/api/SPISlave_8h_source.html)


## Example

Reply to an SPI master as slave:

```c++
#include "mbed.h"
 
SPISlave device(p5, p6, p7, p8); // mosi, miso, sclk, ssel
 
int main() {
   device.reply(0x00);              // Prime SPI with first reply
   while(1) {
       if(device.receive()) {
           int v = device.read();   // Read byte from master
           v = (v + 1) % 0x100;     // Add one to it, modulo 256
           device.reply(v);         // Make this the next reply
       }
   }
}
```

