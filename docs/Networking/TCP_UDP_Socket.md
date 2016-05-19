******* STRIPPED UNSUPPORTED COMMAND NOTOC ******* <div side style="float: right"> <div class="alert-box info" title="Networking">

  1. [Introduction](https://developer.mbed.org/handbook/Networking)  

  2. **_Socket API_**  

  3. [Protocols and APIs](https://developer.mbed.orghttps://developer.mbed.org/handbook/TCP-IP-protocols-and-APIs)  

  4. [Ethernet Interface](https://developer.mbed.org/handbook/Ethernet-Interface)  

  5. [VodafoneK3770 Interface](https://developer.mbed.org/handbook/VodafoneK3770-Interface)  
</div> </div> The mbed C++ Socket API provides a simple and consistent way to communicate using bsd-like TCP and UDP sockets over various transports such as ethernet, wifi and mobile networks.

The Socket library is included as part of the networking libraries that implement the different transports, for example:

  * [Ethernet Interface](Ethernet Interface)
  * [VodafoneK3770 Interface](VodafoneK3770 Interface)

To use the sockets interfaces, include the appropriate transport library in your program. This page documents the Sockets API they will make available.

There are also various common protocols implemented on top of the sockets libraries to provide higher level APIs such as HTTP Clients; see:

  * [TCP/IP Protocols and APIs](https://developer.mbed.orghttps://developer.mbed.org/handbook/TCP-IP-protocols-and-APIs)

## TCP Socket

![https://developer.mbed.org/media/uploads/emilmont/tcp.png](https://developer.mbed.org/media/uploads/emilmont/tcp.png) [![View code](https://www.mbed.com/embed/?type=library)](https://developer.mbed.org/users/mbed_official/code/Socket/docs/tip/classTCPSocketServer.html) [![View code](https://www.mbed.com/embed/?type=library)](https://developer.mbed.org/users/mbed_official/code/Socket/docs/tip/classTCPSocketConnection.html)

### TCP Echo Server

[![View code](https://www.mbed.com/embed/?url=https://developer.mbed.org/teams/mbed/code/TCPEchoServer/)](https://developer.mbed.org/teams/mbed/code/TCPEchoServer/file/tip/main.cpp) You can test the above server running on your mbed, with the following Python script running on your PC: ```
import socket
import signal
import sys


def signal_handler(signal, frame):
    print 'You pressed Ctrl+C!'
    s.close()
    sys.exit(0)

signal.signal(signal.SIGINT, signal_handler)
 
ECHO_SERVER_ADDRESS = "192.168.2.2"
ECHO_PORT = 7
message = 'Hello, world'
 
s = socket.socket(socket.AF_INET, socket.SOCK_STREAM) 
s.connect((ECHO_SERVER_ADDRESS, ECHO_PORT))

print 'Sending', repr(message) 
s.sendall(message)
data = s.recv(1024)
s.close()
print 'Received', repr(data)
```

### TCP Echo Client

[![View code](https://www.mbed.com/embed/?url=https://developer.mbed.org/teams/mbed/code/TCPEchoClient/)](https://developer.mbed.org/teams/mbed/code/TCPEchoClient/file/tip/main.cpp) You can test the above Client running on your mbed, with the following Python script running on your PC: ```
import socket
import signal
import sys


def signal_handler(signal, frame):
    print 'You pressed Ctrl+C!'
    s.close()
    sys.exit(0)

signal.signal(signal.SIGINT, signal_handler)

print 'Server Running at ', socket.gethostbyname(socket.gethostname()) 
s = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
s.bind(('', 7))
s.listen(1)
 
while True:
    conn, addr = s.accept()
    print 'Connected by', addr
    while True:
        data = conn.recv(1024)
        if not data: break
        conn.sendall(data)
    conn.close()
```

## UDP Socket

![https://developer.mbed.org/media/uploads/emilmont/udp.png](https://developer.mbed.org/media/uploads/emilmont/udp.png) [![View code](https://www.mbed.com/embed/?type=library)](https://developer.mbed.org/users/mbed_official/code/Socket/docs/tip/classUDPSocket.html) [![View code](https://www.mbed.com/embed/?type=library)](https://developer.mbed.org/users/mbed_official/code/Socket/docs/tip/classEndpoint.html)

### UDP Echo Server

[![View code](https://www.mbed.com/embed/?url=https://developer.mbed.org/teams/mbed/code/UDPEchoServer/)](https://developer.mbed.org/teams/mbed/code/UDPEchoServer/file/tip/main.cpp) You can test the above Server running on your mbed, with the following Python script running on your PC: ```
import socket
import signal
import sys


def signal_handler(signal, frame):
    print 'You pressed Ctrl+C!'
    sock.close()
    sys.exit(0)

signal.signal(signal.SIGINT, signal_handler)
 
ECHO_SERVER_ADDRESS = '10.2.131.195'
ECHO_PORT = 7

sock = socket.socket(socket.AF_INET, socket.SOCK_DGRAM)

sock.sendto("Hello World\n", (ECHO_SERVER_ADDRESS, ECHO_PORT))
response = sock.recv(256)
sock.close()

print response
```

### UDP Echo Client

[![View code](https://www.mbed.com/embed/?url=https://developer.mbed.org/teams/mbed/code/UDPEchoClient/)](https://developer.mbed.org/teams/mbed/code/UDPEchoClient/file/tip/main.cpp) You can test the above Client running on your mbed, with the following Python script running on your PC: ```
import socket
import signal
import sys

def signal_handler(signal, frame):
    print 'You pressed Ctrl+C!'
    sock.close()
    sys.exit(0)

signal.signal(signal.SIGINT, signal_handler)
 
ECHO_PORT = 7

print 'Server Running at ', socket.gethostbyname(socket.gethostname()) 
sock = socket.socket(socket.AF_INET, socket.SOCK_DGRAM)
sock.bind(('', ECHO_PORT))
 
while True:
    print "waiting for UDP data packet..."
    data, address = sock.recvfrom(256)
    print "Received packet from", address, "with data",data
    print "Sending  packet back to client"
    sock.sendto(data, address)
```

## Blocking/Non-Blocking and Timeout

The standard Berkeley sockets functions (accept, send, receive, etc) are blocking, for this reason also our Socket methods are blocking by default.

If you want a non-blocking behaviour, waiting for a certain event only for a certain amount of time, you have to explicitly make a call to the ##set_blocking(bool blocking, unsigned int timeout)## method: ```
// [Default] Blocking: wait the socket being (readable/writable) forever
socket.set_blocking(true)

// Non-Blocking: Return after a maximum of (timeout)ms.
socket.set_blocking(false, timeout)
```

## Broadcasting

![https://developer.mbed.org/media/uploads/emilmont/broadcast.png](https://developer.mbed.org/media/uploads/emilmont/broadcast.png)

### Send Broadcast

[![View code](https://www.mbed.com/embed/?url=https://developer.mbed.org/users/mbed_official/code/BroadcastSend/)](https://developer.mbed.org/users/mbed_official/code/BroadcastSend/file/e18c3e98ed3d/main.cpp) You can receive the messages sent by the above broadcaster running on your mbed, with the following Python script running on your PC: ```
import socket

BROADCAST_PORT = 58083

s = socket.socket(socket.AF_INET, socket.SOCK_DGRAM)
s.bind(('0.0.0.0', BROADCAST_PORT))

while True:
    print s.recvfrom(256)
```

### Receive Broadcast

[![View code](https://www.mbed.com/embed/?url=https://developer.mbed.org/users/mbed_official/code/BroadcastReceive/)](https://developer.mbed.org/users/mbed_official/code/BroadcastReceive/file/28ba970b3e23/main.cpp) To broadcast messages to the above broadcast receiver running on your mbed you can use the following Python script running on your PC: ```
import socket
from time import sleep, time

BROADCAST_PORT = 58083

s = socket.socket(socket.AF_INET, socket.SOCK_DGRAM)
s.bind(('', 0))
s.setsockopt(socket.SOL_SOCKET, socket.SO_BROADCAST, 1)

while True:
    print "Broadcasting..."
    data = 'Hello World: ' + repr(time()) + '\n'
    s.sendto(data, ('', BROADCAST_PORT))
    sleep(1)
```

## Multicasting

![https://developer.mbed.org/media/uploads/emilmont/multicast.png](https://developer.mbed.org/media/uploads/emilmont/multicast.png)

## Send Multicast

[![View code](https://www.mbed.com/embed/?url=https://developer.mbed.org/users/mbed_official/code/MulticastSend/)](https://developer.mbed.org/users/mbed_official/code/MulticastSend/file/1d4435904b0b/main.cpp) You can receive the messages sent by the above multicaster running on your mbed, with the following Python script running on your PC: ```
import socket
import struct

MCAST_GRP = '224.1.1.1'
MCAST_PORT = 5007

sock = socket.socket(socket.AF_INET, socket.SOCK_DGRAM, socket.IPPROTO_UDP)
sock.setsockopt(socket.SOL_SOCKET, socket.SO_REUSEADDR, 1)
sock.bind(('', MCAST_PORT))
mreq = struct.pack("4sl", socket.inet_aton(MCAST_GRP), socket.INADDR_ANY)

sock.setsockopt(socket.IPPROTO_IP, socket.IP_ADD_MEMBERSHIP, mreq)

while True:
    print sock.recv(10240)
```

## Receive Multicast

[![View code](https://www.mbed.com/embed/?url=https://developer.mbed.org/users/mbed_official/code/MulticastReceive/)](https://developer.mbed.org/users/mbed_official/code/MulticastReceive/file/c30ede6e9b30/main.cpp) To multicast messages to the above multicast receiver running on your mbed you can use the following Python script running on your PC: ```
import socket
from time import sleep, time

MCAST_GRP = '224.1.1.1'
MCAST_PORT = 5007

sock = socket.socket(socket.AF_INET, socket.SOCK_DGRAM, socket.IPPROTO_UDP)
sock.setsockopt(socket.IPPROTO_IP, socket.IP_MULTICAST_TTL, 2)

while True:
    print "Multicast to group: %s\n" % MCAST_GRP
    data = 'Hello World: ' + repr(time()) + '\n'
    sock.sendto(data, (MCAST_GRP, MCAST_PORT))
    sleep(1)
```
