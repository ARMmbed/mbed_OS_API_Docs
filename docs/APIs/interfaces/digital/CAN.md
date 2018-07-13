<span class="warnings">**Out of date**: This is not the most recent version of this page. Please see [the most recent version](https://os.mbed.com/docs/latest/reference/can.html)</span>
# CAN

CAN or Controller-Area Network is a bus standard that allows microcontrollers and devices to communicate with each other without going through a host computer.

## API

API summary

[![View code](https://www.mbed.com/embed/?type=library)](https://docs.mbed.com/docs/mbed-os-api/en/mbed-os-5.2/api/CAN_8h_source.html) 

## Hello World!

This example sends a counter from one CAN bus (can1) and listens for a packet on the other CAN bus (can2). Each bus controller should be connected to a CAN bus transceiver. These should be connected together at a CAN bus.

```
#include "mbed.h"

Ticker ticker;
DigitalOut led1(LED1);
DigitalOut led2(LED2);
CAN can1(p9, p10);
CAN can2(p30, p29);
char counter = 0;

void send() {
    printf("send()\n");
    if(can1.write(CANMessage(1337, &counter;, 1))) {
        printf("wloop()\n");
        counter++;
        printf("Message sent: %d\n", counter);
    } 
    led1 = !led1;
}

int main() {
    printf("main()\n");
    ticker.attach(&send;, 1);
    CANMessage msg;
    while(1) {
        printf("loop()\n");
        if(can2.read(msg)) {
            printf("Message received: %d\n", msg.data[0]);
            led2 = !led2;
        } 
        wait(0.2);
    }
}
```


## Details

The CAN interface can be used to write data words out of a CAN port and will return the data received from another CAN device. The CAN clock frequency can be configured.

## Additional resources

  * [Wikipedia](http://en.wikipedia.org/wiki/Controllerarea_network)
