<span class="warnings">**Out of date**: This is not the most recent version of this page. Please see [the most recent version](https://os.mbed.com/docs/latest/reference/i2cslave.html)</span>
# I2CSlave

An I2C Slave, used for communicating with an I2C Master device.

Synchronization level: not protected.

## API


[![View code](https://www.mbed.com/embed/?type=library)](https://docs.mbed.com/docs/mbed-os-api/en/mbed-os-5.2/api/I2CSlave_8h_source.html)

## Example

Simple I2C responder

```c++
#include <mbed.h>
 
I2CSlave slave(p9, p10);
 
int main() {
   char buf[10];
   char msg[] = "Slave!";
 
   slave.address(0xA0);
   while (1) {
       int i = slave.receive();
       switch (i) {
           case I2CSlave::ReadAddressed:
               slave.write(msg, strlen(msg) + 1); // Includes null char
               break;
           case I2CSlave::WriteGeneral:
               slave.read(buf, 10);
               printf("Read G: %s\n", buf);
               break;
           case I2CSlave::WriteAddressed:
               slave.read(buf, 10);
               printf("Read A: %s\n", buf);
               break;
       }
       for(int i = 0; i < 10; i++) buf[i] = 0;    // Clear buffer
   }
}
```

