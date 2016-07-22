# Network Sockets #

The network-socket API provides a common interface for using
[sockets](https://en.wikipedia.org/wiki/Network_socket) on network devices.
The network-socket API provides a simple class-based interface that should
be familiar to users experienced with other socket APIs.

## Example ##

Here is a quick example of a simple HTTP client program:

``` cpp
/* NetworkSocketAPI Example Program
 * Copyright (c) 2015 ARM Limited
 *
 * Licensed under the Apache License, Version 2.0 (the "License");
 * you may not use this file except in compliance with the License.
 * You may obtain a copy of the License at
 *
 *     http://www.apache.org/licenses/LICENSE-2.0
 *
 * Unless required by applicable law or agreed to in writing, software
 * distributed under the License is distributed on an "AS IS" BASIS,
 * WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
 * See the License for the specific language governing permissions and
 * limitations under the License.
 */

#include "mbed.h"
#include "TCPSocket.h"
#include "EthernetInterface.h"

EthernetInterface eth;
TCPSocket socket;

int main()
{
    printf("Example network-socket HTTP client\n");

    eth.connect();
    const char *ip = eth.get_ip_address();
    const char *mac = eth.get_mac_address();
    printf("IP address is: %s\n", ip ? ip : "No IP");
    printf("MAC address is: %s\n", mac ? mac : "No MAC");

    socket.open(&eth);
    socket.connect("developer.mbed.org", 80);

    char sbuffer[] = "GET / HTTP/1.1\r\nHost: developer.mbed.org\r\n\r\n";
    int scount = socket.send(sbuffer, sizeof sbuffer);
    printf("sent %d [%.*s]\r\n", scount, strstr(sbuffer, "\r\n")-sbuffer, sbuffer);

    char rbuffer[64];
    int rcount = socket.recv(rbuffer, sizeof rbuffer);
    printf("recv %d [%.*s]\r\n", rcount, strstr(rbuffer, "\r\n")-rbuffer, rbuffer);

    socket.close();
    eth.disconnect();

    printf("Done\n");
}
```

## The Socket classes ##

The [Socket](https://github.com/mbedmicro/mbed/blob/master/features/net/network-socket/Socket.h#L30)
classes are used for managing network sockets. Once opened, a socket provides 
a pipe through which data can be sent and received to a specific endpoint. The 
type of the instantiated socket indicates the underlying protocol to use.

- The [UDPSocket](https://github.com/mbedmicro/mbed/blob/master/features/net/network-socket/UDPSocket.h#L26)
  class provides the ability to send packets of data over [UDP](https://en.wikipedia.org/wiki/User_Datagram_Protocol)
  using the sendto/recvfrom member functions. Packets can be lost or arrive out
  of order, so a TCPSocket is suggested when delivery is required.

- The [TCPSocket](https://github.com/mbedmicro/mbed/blob/master/features/net/network-socket/TCPSocket.h#L26)
  class provides the ability to send a stream of data over [TCP](https://en.wikipedia.org/wiki/Transmission_Control_Protocol).
  TCPSockets maintain a stateful connection that starts with the connect member
  function. After successfully connecting to a server, the send/recv member
  functions can be used to send and recieve data similarly to writing/reading
  from a file.

- The [TCPServer](https://github.com/mbedmicro/mbed/blob/master/features/net/network-socket/TCPServer.h)
  class provides the ability to accept incoming TCP connections. The listen
  member function sets up the server to listen for incoming connections, and 
  the accept member function sets up a stateful TCPSocket instance on an
  incoming connection.

## The Network Interface classes ##

A socket requires a NetworkInterface instance when opened to indicate which
NetworkInterface the socket should be created on. The NetworkInterface
provides a network stack that implements the underlying socket operations.

Existing network interfaces:
- [EthernetInterface](ethernet.md)
- [WiFiInterface](wifi.md)

More information on network interfaces can be found in the porting
documentation for the network-socket API ([here](network_stacks.md)).

## The SocketAddress class ##

The [SocketAddress](https://github.com/mbedmicro/mbed/blob/master/features/net/network-socket/SocketAddress.h#L28)
class can be used to represent the IP address and port pair of a unique
network endpoint. Most network functions are also overloaded to accept string
representations of IP addresses, but SocketAddress can be used to avoid the
overhead of parsing IP addresses during repeated network transactions and
can be passed around as a first class object.

## Network Errors ##

The convention of the network-socket API is for functions to return negative
error codes to indicate failure. On success a function may return zero or a
non-negative integer to indicate the size of a transaction. On failure a
function must return a negative integer which should be one of the following
error codes in the `nsapi_error_t` enum ([here](https://github.com/mbedmicro/mbed/blob/master/features/net/network-socket/nsapi_types.h#L27)):

``` cpp
/** Enum of standardized error codes 
 *
 *  Valid error codes have negative values and may
 *  be returned by any network operation.
 *
 *  @enum nsapi_error_t
 */
typedef enum nsapi_error {
    NSAPI_ERROR_WOULD_BLOCK   = -3001,     /*!< no data is not available but call is non-blocking */
    NSAPI_ERROR_UNSUPPORTED   = -3002,     /*!< unsupported functionality */
    NSAPI_ERROR_PARAMETER     = -3003,     /*!< invalid configuration */
    NSAPI_ERROR_NO_CONNECTION = -3004,     /*!< not connected to a network */
    NSAPI_ERROR_NO_SOCKET     = -3005,     /*!< socket not available for use */
    NSAPI_ERROR_NO_ADDRESS    = -3006,     /*!< IP address is not known */
    NSAPI_ERROR_NO_MEMORY     = -3007,     /*!< memory resource not available */
    NSAPI_ERROR_DNS_FAILURE   = -3008,     /*!< DNS failed to complete successfully */
    NSAPI_ERROR_DHCP_FAILURE  = -3009,     /*!< DHCP failed to complete successfully */
    NSAPI_ERROR_AUTH_FAILURE  = -3010,     /*!< connection to access point faield */
    NSAPI_ERROR_DEVICE_ERROR  = -3011,     /*!< failure interfacing with the network procesor */
} nsapi_error_t;
```

## Non-blocking operation ##

The network-socket API also supports non-blocking operation. The set_blocking
member function changes the state of a socket. When a socket is in non-blocking
mode, socket operations return NSAPI_ERROR_WOULD_BLOCK when a transaction can
not be immediately completed.

To allow efficient use of non-blocking operations, the socket classes provide
an attach member function to register a callback on socket state changes.
The callback will be called when the socket can recv/send/accept successfully
or if an error occurs. The callback may be called spuriously without reason.

The callback may be called in an interrupt context and should not perform
expensive operations such as recv/send calls.
