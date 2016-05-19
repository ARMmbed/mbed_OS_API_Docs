<div side style="float: right"> <div class="alert-box info" title="Networking">

  1. [Introduction](https://developer.mbed.org/handbook/Networking)  

  2. [Socket API](https://developer.mbed.org/handbook/Socket)  

  3. [Protocols and APIs](https://developer.mbed.org/handbook/TCP-IP-protocols-and-APIs)  

  4. **_Ethernet Interface_**  

  5. [VodafoneK3770 Interface](https://developer.mbed.org/handbook/VodafoneK3770-Interface)  
</div> </div> ******* STRIPPED UNSUPPORTED COMMAND NOTOC *******

## Hardware

First of all you have to connect your mbed to a RJ45 jack: [Ethernet RJ45](https://developer.mbed.org/cookbook/Ethernet-RJ45).

## Software

[![View code](https://www.mbed.com/embed/?type=library)](https://developer.mbed.org/users/mbed_official/code/EthernetInterface/latest) The EthernetInterface library provides you with a simple API to connect to the internet. [![View code](https://www.mbed.com/embed/?type=library)](https://developer.mbed.org/users/mbed_official/code/EthernetInterface/docs/tip/classEthernetInterface.html) ```
#include "EthernetInterface.h"
``` First, you need to setup the connection by choosing whether you want to use DHCP or a static IP addressing with the _init()_ function.

Then, simply _connect()_ to the network. When you're done, _disconnect()_ from the network.

When you are connected, you can use either the Socket API or any high-level component (HTTP Client).

## Examples

These examples demonstrate how to get started with the Socket API &amp; Ethernet. [![View code](https://www.mbed.com/embed/?url=https://developer.mbed.org/users/mbed_official/code/TCPSocket_HelloWorld/)](https://developer.mbed.org/users/mbed_official/code/TCPSocket_HelloWorld/file/tip/main.cpp) Running this example the mbed will output something similar from the serial port:
    
    
    IP Address is 10.2.131.195
    Received 293 chars from server:
    HTTP/1.1 200 OK
    Server: nginx/0.7.62
    Date: Fri, 27 Jul 2012 14:36:00 GMT
    Content-Type: text/plain
    Connection: close
    Last-Modified: Fri, 27 Jul 2012 13:30:34 GMT
    Vary: Accept-Encoding
    Content-Length: 14
    X-ServedBy: web_4
    X-Varnish: 230435690
    Age: 0
    Via: 1.1 varnish
    
    Hello world!

[![View code](https://www.mbed.com/embed/?url=https://developer.mbed.org/users/mbed_official/code/UDPSocket_HelloWorld/)](https://developer.mbed.org/users/mbed_official/code/UDPSocket_HelloWorld/file/tip/main.cpp)

## Feedback

We would be glad to get your feedback on this forum thread:

  * [EthernetInterface Library](https://developer.mbed.org/forum/mbed/topic/3741/)
