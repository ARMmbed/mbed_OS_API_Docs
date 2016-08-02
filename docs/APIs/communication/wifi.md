# WiFi

The [WifiInterface](https://github.com/mbedmicro/mbed/blob/master/features/net/network-socket/WiFiInterface.h#L37) provides a simple C++ API for connecting to the internet over a WiFi device.

There are multiple WiFi components that implement the WiFiInterface class. For this example,
the [ESP8266Interface](https://github.com/armmbed/esp8266-driver) is used.

``` cpp
class WiFiInterface: public NetworkInterface
{
public:
    /** Start the interface
     *
     *  Attempts to connect to a WiFi network. If passphrase is invalid,
     *  NSAPI_ERROR_AUTH_ERROR is returned.
     *
     *  @param ssid      Name of the network to connect to
     *  @param pass      Security passphrase to connect to the network
     *  @param security  Type of encryption for connection
     *  @return          0 on success, negative error code on failure
     */
    virtual int connect(const char *ssid, const char *pass, nsapi_security_t security = NSAPI_SECURITY_NONE) = 0;

    /** Stop the interface
     *
     *  @return          0 on success, negative error code on failure
     */
    virtual int disconnect() = 0;

    /** Get the local MAC address
     *
     *  @return         Null-terminated representation of the local MAC address
     */
    virtual const char *get_mac_address() = 0;

    /** Get the local IP address
     *
     *  @return         Null-terminated representation of the local IP address
     *                  or null if not yet connected
     */
    virtual const char *get_ip_address() = 0;
};
```

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

