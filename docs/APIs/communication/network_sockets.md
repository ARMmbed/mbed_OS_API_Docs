# Network Sockets

The network-socket API provides a common interface for using
[sockets](https://en.wikipedia.org/wiki/Network_socket) on network devices. It's a class-based interface, which should be familiar to users experienced with other socket APIs.

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

## The Socket classes

The [Socket](https://github.com/mbedmicro/mbed/blob/master/features/net/network-socket/Socket.h#L30) classes are used for managing network sockets. Once opened, a socket provides 
a pipe through which data can be sent and received to a specific endpoint. The type of the instantiated socket indicates the underlying protocol to use:

- The [UDPSocket](https://github.com/mbedmicro/mbed/blob/master/features/net/network-socket/UDPSocket.h#L26) class provides the ability to send packets of data over [UDP](https://en.wikipedia.org/wiki/User_Datagram_Protocol), using the ``sendto`` and ``recvfrom`` member functions. Packets can be lost or arrive out of order, so we suggest using a TCPSocket (described below) when garunteed delivery is required.

- The [TCPSocket](https://github.com/mbedmicro/mbed/blob/master/features/net/network-socket/TCPSocket.h#L26) class provides the ability to send a stream of data over [TCP](https://en.wikipedia.org/wiki/Transmission_Control_Protocol).  TCPSockets maintain a stateful connection that starts with the ``connect`` member function. After successfully connecting to a server, you can use the ``send`` and ``recv`` member functions to send and receive data (similar to writing or reading from a file).

- The [TCPServer](https://github.com/mbedmicro/mbed/blob/master/features/net/network-socket/TCPServer.h) class provides the ability to accept incoming TCP connections. The `listen` member function sets up the server to listen for incoming connections, and the `accept` member function sets up a stateful TCPSocket instance on an incoming connection.

## The NetworkInterface classes

A socket requires a NetworkInterface instance when opened to indicate which NetworkInterface the socket should be created on. The NetworkInterface provides a network stack that implements the underlying socket operations.

Existing network interfaces:

- [EthernetInterface](ethernet.md)
- [WiFiInterface](wifi.md)

## The SocketAddress class

Use the [SocketAddress](https://github.com/mbedmicro/mbed/blob/master/features/net/network-socket/SocketAddress.h#L28)
class to represent the IP address and port pair of a unique
network endpoint. Most network functions are also overloaded to accept string representations of IP addresses, but SocketAddress can be used to avoid the overhead of parsing IP addresses during repeated network transactions, and can be passed around as a first class object.

## Network errors

The convention of the network-socket API is for functions to return negative error codes to indicate failure. On success, a function may return zero or a non-negative integer to indicate the size of a transaction. On failure, a function must return a negative integer, which should be one of the error codes in the `nsapi_error_t` enum ([here](https://github.com/mbedmicro/mbed/blob/master/features/net/network-socket/nsapi_types.h#L27)):

``` cpp
/** Enum of standardized error codes 
 *
 *  Valid error codes have negative values and may
 *  be returned by any network operation.
 *
 *  @enum nsapi_error_t
 */
typedef enum nsapi_error {
    NSAPI_ERROR_WOULD_BLOCK   = -3001,     /*!< data is not available but call is non-blocking */
    NSAPI_ERROR_UNSUPPORTED   = -3002,     /*!< unsupported functionality */
    NSAPI_ERROR_PARAMETER     = -3003,     /*!< invalid configuration */
    NSAPI_ERROR_NO_CONNECTION = -3004,     /*!< not connected to a network */
    NSAPI_ERROR_NO_SOCKET     = -3005,     /*!< socket not available for use */
    NSAPI_ERROR_NO_ADDRESS    = -3006,     /*!< IP address is not known */
    NSAPI_ERROR_NO_MEMORY     = -3007,     /*!< memory resource not available */
    NSAPI_ERROR_DNS_FAILURE   = -3008,     /*!< DNS failed to complete successfully */
    NSAPI_ERROR_DHCP_FAILURE  = -3009,     /*!< DHCP failed to complete successfully */
    NSAPI_ERROR_AUTH_FAILURE  = -3010,     /*!< connection to access point failed */
    NSAPI_ERROR_DEVICE_ERROR  = -3011,     /*!< failure interfacing with the network processor */
} nsapi_error_t;
```

## Non-blocking operation

The network-socket API also supports non-blocking operation. The ``set_blocking`` member function changes the state of a socket. When a socket is in non-blocking mode, socket operations return ``NSAPI_ERROR_WOULD_BLOCK`` when a transaction cannot be immediately completed.

To allow efficient use of non-blocking operations, the socket classes provide an ``attach`` member function to register a callback on socket state changes. The callback will be called when the socket can successfully receive, send or accept, or when an error occurs. The callback may be called spuriously without reason.

The callback may be called in an interrupt context and should not perform expensive operations such as receive and send calls.
