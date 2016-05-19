<div class="alert-box info" title="Ethernet Network Interface"> The Ethernet peripheral is of little use without an IP networking stack on top of it. You could be interested in the [Ethernet Interface](https://developer.mbed.org/handbook/Ethernet-Interface) library. </div>

The Ethernet Interface allows the mbed Microcontroller to connect and communicate with an Ethernet network. This can therefore be used to talk to other devices on a network, including communication with other computers such as web and email servers on the internet, or act as a webserver.

## Hello World!

```

#include "mbed.h"

Ethernet eth;

int main() {
    char buf[0x600];

    while(1) {
        int size = eth.receive();
        if(size > 0) {
            eth.read(buf, size);
            printf("Destination:  %02X:%02X:%02X:%02X:%02X:%02X\n",
                    buf[0], buf[1], buf[2], buf[3], buf[4], buf[5]);
            printf("Source: %02X:%02X:%02X:%02X:%02X:%02X\n",
                    buf[6], buf[7], buf[8], buf[9], buf[10], buf[11]);
        }

        wait(1);
    }
}
```

## API

API summary

[![View code](https://www.mbed.com/embed/?type=library)](https://developer.mbed.org/users/mbed_official/code/mbed/docs/3d0ef94e36ec/classmbed_1_1Ethernet.html) 

## Interface

[![/media/uploads/chris/pinout-thumbnails_lpc.jpg](https://developer.mbed.org/media/uploads/chris/pinout-thumbnails_lpc.jpg)](https://developer.mbed.org/handbook/Pinouts)  
---  
[See the Pinout page for more details](https://developer.mbed.org/handbook/Pinouts)  
  
All of the required passive termination circuits are implemented on the mbed Microcontoller, allowing direct connection to the ethernet network.

The Ethernet library sets the MAC address by calling a weak function `extern "C" void mbed_mac_address(char * mac);` to copy in a 6 character MAC address. This in turn performs a semihosting request to the mbed interface to get the serial number, which contains a MAC address unique to every mbed device. If you are using this library on your own board (i.e. not an mbed board), you should implement your own `extern "C" void mbed_mac_address(char * mac);` function, to overwrite the existing one and avoid a call to the interface.
