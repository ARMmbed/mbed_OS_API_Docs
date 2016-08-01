# Ethernet

The [EthernetInterface](https://github.com/mbedmicro/mbed/blob/master/features/net/FEATURE_IPV4/lwip-interface/EthernetInterface.h#L28) provides a simple C++ API for connecting to the internet over an Ethernet connection.

``` cpp
/** EthernetInterface class
 *  Implementation of the NetworkStack for LWIP
 */
class EthernetInterface : public EthInterface
{
public:
    /** Start the interface
     *  @return             0 on success, negative on failure
     */
    virtual int connect();

    /** Stop the interface
     *  @return             0 on success, negative on failure
     */
    virtual int disconnect();

    /** Get the internally stored IP address
     *  @return             IP address of the interface or null if not yet connected
     */
    virtual const char *get_ip_address();

    /** Get the internally stored MAC address
     *  @return             MAC address of the interface
     */
    virtual const char *get_mac_address();
};
```

## Usage

To bring up the network interface:

1. Instantiate the ``EthernetInterface`` class.
1. Call the ``connect`` function. 
1. Once connected, the EthernetInterface can be used as a
target for opening [network sockets](network_sockets.md).

## Example

Here is a quick example of a simple HTTP client program. The program brings up ethernet as the underlying network interface, and uses it to perform an HTTP transaction over a TCPSocket:

``` cpp
#include "mbed.h"
#include "TCPSocket.h"
#include "EthernetInterface.h"

EthernetInterface eth;
TCPSocket socket;

int main()
{
    printf("Example network-socket HTTP client\n");

    // Brings up the network interface
    eth.connect();
    const char *ip = eth.get_ip_address();
    const char *mac = eth.get_mac_address();
    printf("IP address is: %s\n", ip ? ip : "No IP");
    printf("MAC address is: %s\n", mac ? mac : "No MAC");

    // Open a socket on the network interface, and create a TCP connection to mbed.org
    socket.open(&eth);
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
    eth.disconnect();

    printf("Done\n");
}
```

