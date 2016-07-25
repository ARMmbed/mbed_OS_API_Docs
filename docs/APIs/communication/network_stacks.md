# Network Stacks #

The network-socket API is designed to make porting new devices easy and only
requires a handful of functions for a minimal implementation.

## NetworkInterface ##

A new device must provide a [NetworkInterface](https://github.com/mbedmicro/mbed/blob/master/features/net/network-socket/NetworkInterface.h#L24)
class with the naming convention of **DeviceInterface** where **Device** is a 
unique name that represents the device or network processor. The**DeviceInterface** should inherit one of the following unless it is an abstract device:

- EthInterface
- WiFiInterface
- CellularInterface
- MeshInterface

## Socket Handle ##

Most network operations operate on a network socket. In the network-socket API,
specific sockets are represented by a `nsapi_socket_t` handle which is defined 
as a `void *`. The actual value of the socket handle is up to the device, which
can use the socket handle to pass socket-specific context between socket
functions. The implementation-specific socket functions are managed by the
Socket classes and hidden from the user.

## Socket Addresses ##

The [SocketAddress](https://github.com/mbedmicro/mbed/blob/master/features/net/network-socket/SocketAddress.h#L28)
class can be used to represent the IP address and port pair of a unique
network endpoint. The SocketAddress can be used to avoid the overhead of
parsing IP addresses during repeated network transactions and can be passed
around as a first class object.

Alternatively, the primitive [nsapi_addr_t](https://github.com/mbedmicro/mbed/blob/master/features/net/network-socket/nsapi_types.h#L92)
structure can be passed around as a C-compatible type containing the raw
IP address bytes in big-endian order. The nsapi_addr_t type can be converted
to a SocketAddress, or obtained from a SocketAddress through the get_addr
member function.

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

## NetworkStack ##

A new device must provide a [NetworkStack](https://github.com/mbedmicro/mbed/blob/master/features/net/network-socket/NetworkStack.h#L25)
instance through the get_stack member function. The NetworkStack object
provides network operations and can be implemented through either [extending
the NetworkStack object directly](#extending-the-networkstack-class)
or through [providing a nsapi_stack_t structure](#providing-a-nsapi_stack_t-structure).

### Extending the NetworkStack class ###

A device may provide an implementation of the NetworkStack class. This
provides an easy route for porting standalone devices that can be represented
as a C++ class. A device should inherit the NetworkStack class and implement
the abstract member functions. This class should be instantiated as a part of
the DeviceInterface and returned from the DeviceInterface's get_stack member
function.

The [esp8266](https://github.com/ARMmbed/esp8266-driver/) provides a good
example of a class based implementation.

Required member functions:

``` cpp
/** Get the local IP address
 *
 *  @return         Null-terminated representation of the local IP address
 *                  or null if not yet connected
 */
const char *get_ip_address();

/** Opens a socket
 *
 *  Creates a network socket and stores it in the specified handle.
 *  The handle must be passed to following calls on the socket.
 *
 *  A stack may have a finite number of sockets, in this case
 *  NSAPI_ERROR_NO_SOCKET is returned if no socket is available.
 *
 *  @param handle   Destination for the handle to a newly created socket
 *  @param proto    Protocol of socket to open, NSAPI_TCP or NSAPI_UDP
 *  @return         0 on success, negative error code on failure
 */
int socket_open(nsapi_socket_t *handle, nsapi_protocol_t proto);

/** Close the socket
 *
 *  Closes any open connection and deallocates any memory associated
 *  with the socket.
 *
 *  @param handle   Socket handle
 *  @return         0 on success, negative error code on failure
 */
int socket_close(nsapi_socket_t handle);

/** Bind a specific address to a socket
 *
 *  Binding a socket specifies the address and port on which to recieve
 *  data. If the IP address is zeroed, only the port is bound.
 *
 *  @param handle   Socket handle
 *  @param address  Local address to bind
 *  @return         0 on success, negative error code on failure.
 */
int socket_bind(nsapi_socket_t handle, const SocketAddress &address);

/** Listen for connections on a TCP socket
 *
 *  Marks the socket as a passive socket that can be used to accept
 *  incoming connections.
 *
 *  @param handle   Socket handle
 *  @param backlog  Number of pending connections that can be queued
 *                  simultaneously
 *  @return         0 on success, negative error code on failure
 */
int socket_listen(nsapi_socket_t handle, int backlog);

/** Connects TCP socket to a remote host
 *
 *  Initiates a connection to a remote server specified by the
 *  indicated address.
 *
 *  @param handle   Socket handle
 *  @param address  The SocketAddress of the remote host
 *  @return         0 on success, negative error code on failure
 */
int socket_connect(nsapi_socket_t handle, const SocketAddress &address);

/** Accepts a connection on a TCP socket
 *
 *  The server socket must be bound and set to listen for connections.
 *  On a new connection, creates a network socket and stores it in the
 *  specified handle. The handle must be passed to following calls on
 *  the socket.
 *
 *  A stack may have a finite number of sockets, in this case
 *  NSAPI_ERROR_NO_SOCKET is returned if no socket is available.
 *
 *  This call is non-blocking. If accept would block,
 *  NSAPI_ERROR_WOULD_BLOCK is returned immediately.
 *
 *  @param handle   Destination for a handle to the newly created sockey
 *  @param server   Socket handle to server to accept from
 *  @return         0 on success, negative error code on failure
 */
int socket_accept(nsapi_socket_t *handle, nsapi_socket_t server);

/** Send data over a TCP socket
 *
 *  The socket must be connected to a remote host. Returns the number of
 *  bytes sent from the buffer.
 *
 *  This call is non-blocking. If send would block,
 *  NSAPI_ERROR_WOULD_BLOCK is returned immediately.
 *
 *  @param handle   Socket handle
 *  @param data     Buffer of data to send to the host
 *  @param size     Size of the buffer in bytes
 *  @return         Number of sent bytes on success, negative error
 *                  code on failure
 */
int socket_send(nsapi_socket_t handle, const void *data, unsigned size);

/** Receive data over a TCP socket
 *
 *  The socket must be connected to a remote host. Returns the number of
 *  bytes received into the buffer.
 *
 *  This call is non-blocking. If recv would block,
 *  NSAPI_ERROR_WOULD_BLOCK is returned immediately.
 *
 *  @param handle   Socket handle
 *  @param data     Destination buffer for data received from the host
 *  @param size     Size of the buffer in bytes
 *  @return         Number of received bytes on success, negative error
 *                  code on failure
 */
int socket_recv(nsapi_socket_t handle, void *data, unsigned size);

/** Send a packet over a UDP socket
 *
 *  Sends data to the specified address. Returns the number of bytes
 *  sent from the buffer.
 *
 *  This call is non-blocking. If sendto would block,
 *  NSAPI_ERROR_WOULD_BLOCK is returned immediately.
 *
 *  @param handle   Socket handle
 *  @param address  The SocketAddress of the remote host
 *  @param data     Buffer of data to send to the host
 *  @param size     Size of the buffer in bytes
 *  @return         Number of sent bytes on success, negative error
 *                  code on failure
 */
int socket_sendto(nsapi_socket_t handle, const SocketAddress &address, const void *data, unsigned size);

/** Receive a packet over a UDP socket
 *
 *  Receives data and stores the source address in address if address
 *  is not NULL. Returns the number of bytes received into the buffer.
 *
 *  This call is non-blocking. If recvfrom would block,
 *  NSAPI_ERROR_WOULD_BLOCK is returned immediately.
 *
 *  @param handle   Socket handle
 *  @param address  Destination for the source address or NULL
 *  @param data     Destination buffer for data received from the host
 *  @param size     Size of the buffer in bytes
 *  @return         Number of received bytes on success, negative error
 *                  code on failure
 */
int socket_recvfrom(nsapi_socket_t handle, SocketAddress *address, void *buffer, unsigned size);

/** Register a callback on state change of the socket
 *
 *  The specified callback will be called on state changes such as when
 *  the socket can recv/send/accept successfully and on when an error
 *  occurs. The callback may also be called spuriously without reason.
 *
 *  The callback may be called in an interrupt context and should not
 *  perform expensive operations such as recv/send calls.
 *
 *  @param handle   Socket handle
 *  @param callback Function to call on state change
 *  @param data     Argument to pass to callback
 */
void socket_attach(nsapi_socket_t handle, void (*callback)(void *), void *data);
```

### Providing a nsapi_stack_t structure ###

A device may provide primitive nsapi_stack_t structure. this provides an easy
route for porting full network-stacks, which only need a minimal socket
interface. The most important member of the nsapi_stack_t structure is the
nsapi_stack_api_t structure which contains function pointers for the
network operations. Any function left as null will implement the default
behaviour for that operation. The nsapi_stack_t structure should be declared
with a pointer to the nsapi_stack_ops_t structure, and be passed to the
[nsapi_create_stack](https://github.com/mbedmicro/mbed/blob/master/features/net/network-socket/NetworkStack.h#L282)
function to create a NetworkStack instance.

The [lwip](https://github.com/mbedmicro/mbed/tree/master/features/net/FEATURE_IPV4/lwip-interface) 
network stack provides a good example of a nsapi_stack_t based implementation.

Required functions:

``` cpp
/** Get the local IP address
 *
 *  @param stack    Stack handle
 *  @return         Local IP Address or null address if not connected
 */
nsapi_addr_t (*get_ip_address)(nsapi_stack_t *stack);

/** Opens a socket
 *
 *  Creates a network socket and stores it in the specified handle.
 *  The handle must be passed to following calls on the socket.
 *
 *  A stack may have a finite number of sockets, in this case
 *  NSAPI_ERROR_NO_SOCKET is returned if no socket is available.
 *
 *  @param stack    Stack context
 *  @param socket   Destination for the handle to a newly created socket
 *  @param proto    Protocol of socket to open, NSAPI_TCP or NSAPI_UDP
 *  @return         0 on success, negative error code on failure
 */
int (*socket_open)(nsapi_stack_t *stack, nsapi_socket_t *socket, nsapi_protocol_t proto);

/** Close the socket
 *
 *  Closes any open connection and deallocates any memory associated
 *  with the socket.
 *
 *  @param stack    Stack handle
 *  @param socket   Socket handle
 *  @return         0 on success, negative error code on failure
 */
int (*socket_close)(nsapi_stack_t *stack, nsapi_socket_t socket);

/** Bind a specific address to a socket
 *
 *  Binding a socket specifies the address and port on which to recieve
 *  data. If the IP address is zeroed, only the port is bound.
 *
 *  @param stack    Stack handle
 *  @param socket   Socket handle
 *  @param addr     Local address to bind, may be null
 *  @param port     Local port to bind
 *  @return         0 on success, negative error code on failure.
 */
int (*socket_bind)(nsapi_stack_t *stack, nsapi_socket_t socket, nsapi_addr_t addr, uint16_t port);

/** Listen for connections on a TCP socket
 *
 *  Marks the socket as a passive socket that can be used to accept
 *  incoming connections.
 *
 *  @param stack    Stack handle
 *  @param socket   Socket handle
 *  @param backlog  Number of pending connections that can be queued
 *                  simultaneously
 *  @return         0 on success, negative error code on failure
 */
int (*socket_listen)(nsapi_stack_t *stack, nsapi_socket_t socket, int backlog);

/** Connects TCP socket to a remote host
 *
 *  Initiates a connection to a remote server specified by the
 *  indicated address.
 *
 *  @param stack    Stack handle
 *  @param socket   Socket handle
 *  @param addr     The address of the remote host
 *  @param port     The port of the remote host
 *  @return         0 on success, negative error code on failure
 */
int (*socket_connect)(nsapi_stack_t *stack, nsapi_socket_t socket, nsapi_addr_t addr, uint16_t port);

/** Accepts a connection on a TCP socket
 *
 *  The server socket must be bound and set to listen for connections.
 *  On a new connection, creates a network socket and stores it in the
 *  specified handle. The handle must be passed to following calls on
 *  the socket.
 *
 *  A stack may have a finite number of sockets, in this case
 *  NSAPI_ERROR_NO_SOCKET is returned if no socket is available.
 *
 *  This call is non-blocking. If accept would block,
 *  NSAPI_ERROR_WOULD_BLOCK is returned immediately.
 *
 *  @param stack    Stack handle
 *  @param socket   Destination for a handle to the newly created sockey
 *  @param server   Socket handle to server to accept from
 *  @return         0 on success, negative error code on failure
 */
int (*socket_accept)(nsapi_stack_t *stack, nsapi_socket_t *socket, nsapi_socket_t server);

/** Send data over a TCP socket
 *
 *  The socket must be connected to a remote host. Returns the number of
 *  bytes sent from the buffer.
 *
 *  This call is non-blocking. If send would block,
 *  NSAPI_ERROR_WOULD_BLOCK is returned immediately.
 *
 *  @param stack    Stack handle
 *  @param socket   Socket handle
 *  @param data     Buffer of data to send to the host
 *  @param size     Size of the buffer in bytes
 *  @return         Number of sent bytes on success, negative error
 *                  code on failure
 */
int (*socket_send)(nsapi_stack_t *stack, nsapi_socket_t socket, const void *data, unsigned size);

/** Receive data over a TCP socket
 *
 *  The socket must be connected to a remote host. Returns the number of
 *  bytes received into the buffer.
 *
 *  This call is non-blocking. If recv would block,
 *  NSAPI_ERROR_WOULD_BLOCK is returned immediately.
 *
 *  @param stack    Stack handle
 *  @param socket   Socket handle
 *  @param data     Destination buffer for data received from the host
 *  @param size     Size of the buffer in bytes
 *  @return         Number of received bytes on success, negative error
 *                  code on failure
 */
int (*socket_recv)(nsapi_stack_t *stack, nsapi_socket_t socket, void *data, unsigned size);

/** Send a packet over a UDP socket
 *
 *  Sends data to the specified address. Returns the number of bytes
 *  sent from the buffer.
 *
 *  This call is non-blocking. If sendto would block,
 *  NSAPI_ERROR_WOULD_BLOCK is returned immediately.
 *
 *  @param stack    Stack handle
 *  @param socket   Socket handle
 *  @param addr     The address of the remote host
 *  @param port     The port of the remote host
 *  @param data     Buffer of data to send to the host
 *  @param size     Size of the buffer in bytes
 *  @return         Number of sent bytes on success, negative error
 *                  code on failure
 */
int (*socket_sendto)(nsapi_stack_t *stack, nsapi_socket_t socket, nsapi_addr_t addr, uint16_t port, const void *data, unsigned size);

/** Receive a packet over a UDP socket
 *
 *  Receives data and stores the source address in address if address
 *  is not NULL. Returns the number of bytes received into the buffer.
 *
 *  This call is non-blocking. If recvfrom would block,
 *  NSAPI_ERROR_WOULD_BLOCK is returned immediately.
 *
 *  @param stack    Stack handle
 *  @param socket   Socket handle
 *  @param addr     Destination for the address of the remote host
 *  @param port     Destination for the port of the remote host
 *  @param data     Destination buffer for data received from the host
 *  @param size     Size of the buffer in bytes
 *  @return         Number of received bytes on success, negative error
 *                  code on failure
 */
int (*socket_recvfrom)(nsapi_stack_t *stack, nsapi_socket_t socket, nsapi_addr_t *addr, uint16_t *port, void *buffer, unsigned size);

/** Register a callback on state change of the socket
 *
 *  The specified callback will be called on state changes such as when
 *  the socket can recv/send/accept successfully and on when an error
 *  occurs. The callback may also be called spuriously without reason.
 *
 *  The callback may be called in an interrupt context and should not
 *  perform expensive operations such as recv/send calls.
 *
 *  @param stack    Stack handle
 *  @param socket   Socket handle
 *  @param callback Function to call on state change
 *  @param data     Argument to pass to callback
 */
void (*socket_attach)(nsapi_stack_t *stack, nsapi_socket_t socket, void (*callback)(void *), void *data);
```

