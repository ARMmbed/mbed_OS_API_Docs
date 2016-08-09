# WiFi

The [WifiInterface](https://docs.mbed.com/docs/mbed-os-api/en/mbed-os-5.1.0/api/classWiFiInterface.html) provides a simple C++ API for connecting to the internet over a WiFi device.

There are multiple WiFi components that implement the WiFiInterface class. For this example,
the [ESP8266Interface](https://github.com/armmbed/esp8266-driver) is used.

## API

[![View code](https://www.mbed.com/embed/?type=library)](https://docs.mbed.com/docs/mbed-os-api/en/mbed-os-5.1.0/api/classWiFiInterface.html)

## Usage

To bring up the network interface:

1. Instantiate an implementation of the WiFiInterface class (for example the [ESP8266Interface](https://github.com/armmbed/esp8266-driver)).
1. Call the ``connect`` function with an SSID and password for the WiFi network. 
1. Once connected, the WiFiInterface can be used as a target for opening [network sockets](network_sockets.md).

## Example

Here is a quick example of a simple HTTP client program. The program brings up an ESP8266 as the underlying network interface, and uses it to perform an HTTP transaction over a TCPSocket:

``` cpp
#include "mbed.h"
#include "TCPSocket.h"
#include "ESP8266Interface.h"

ESP8266Interface wifi;
TCPSocket socket;

int main()
{
    printf("Example network-socket HTTP client\n");

    // Brings up the network interface
    wifi.connect("wifissid", "password");
    const char *ip = wifi.get_ip_address();
    const char *mac = wifi.get_mac_address();
    printf("IP address is: %s\n", ip ? ip : "No IP");
    printf("MAC address is: %s\n", mac ? mac : "No MAC");

    // Open a socket on the network interface, and create a TCP connection to mbed.org
    socket.open(&wifi);
    socket.connect("developer.mbed.org", 80);

    // Send a simple http request
    char sbuffer[] = "GET / HTTP/1.1\r\nHost: developer.mbed.org\r\n\r\n";
    int scount = socket.send(sbuffer, sizeof sbuffer);
    printf("sent %d [%.*s]\r\n", scount, strstr(sbuffer, "\r\n")-sbuffer, sbuffer);

    // Recieve a simple http response and print out the response line
    char rbuffer[64];
    int rcount = socket.recv(rbuffer, sizeof rbuffer);
    printf("recv %d [%.*s]\r\n", rcount, strstr(rbuffer, "\r\n")-rbuffer, rbuffer);

    // Close the socket to return its memory and bring down the network interface
    socket.close();
    wifi.disconnect();

    printf("Done\n");
}
```

