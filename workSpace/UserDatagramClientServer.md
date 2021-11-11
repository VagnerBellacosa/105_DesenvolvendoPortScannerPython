# User Datagram Client and Server[¶](https://pymotw.com/2/socket/udp.html#user-datagram-client-and-server)

The user datagram protocol (UDP) works differently from TCP/IP. Where TCP is a *stream oriented* protocol, ensuring that all of the data is transmitted in the right order, UDP is a *message oriented* protocol. UDP does not require a long-lived connection, so setting up a UDP socket is a little simpler. On the other hand, UDP messages must fit within a single packet (for IPv4, that means they can only hold 65,507 bytes because the 65,535 byte packet also includes header information) and delivery is not guaranteed as it is with TCP.

## Echo Server

Since there is no connection, per se, the server does not need to listen for and accept connections. It only needs to use `bind()` to associate its socket with a port, and then wait for individual messages.

```
import socket
import sys

# Create a TCP/IP socket
sock = socket.socket(socket.AF_INET, socket.SOCK_DGRAM)

# Bind the socket to the port
server_address = ('localhost', 10000)
print >>sys.stderr, 'starting up on %s port %s' % server_address
sock.bind(server_address)
```

Messages are read from the socket using `recvfrom()`, which returns the data as well as the address of the client from which it was sent.

```
while True:
    print >>sys.stderr, '\nwaiting to receive message'
    data, address = sock.recvfrom(4096)
    
    print >>sys.stderr, 'received %s bytes from %s' % (len(data), address)
    print >>sys.stderr, data
    
    if data:
        sent = sock.sendto(data, address)
        print >>sys.stderr, 'sent %s bytes back to %s' % (sent, address)
```

## Echo Client

The UDP echo client is similar the server, but does not use `bind()` to attach its socket to an address. It uses `sendto()` to deliver its message directly to the server, and `recvfrom()` to receive the response.

```
import socket
import sys

# Create a UDP socket
sock = socket.socket(socket.AF_INET, socket.SOCK_DGRAM)

server_address = ('localhost', 10000)
message = 'This is the message.  It will be repeated.'

try:

    # Send data
    print >>sys.stderr, 'sending "%s"' % message
    sent = sock.sendto(message, server_address)

    # Receive response
    print >>sys.stderr, 'waiting to receive'
    data, server = sock.recvfrom(4096)
    print >>sys.stderr, 'received "%s"' % data

finally:
    print >>sys.stderr, 'closing socket'
    sock.close()
```

## Client and Server Together

Running the server produces:

```
$ python ./socket_echo_server_dgram.py

starting up on localhost port 10000

waiting to receive message
received 42 bytes from ('127.0.0.1', 50139)
This is the message.  It will be repeated.
sent 42 bytes back to ('127.0.0.1', 50139)

waiting to receive message
```

and the client output is:

```
$ python ./socket_echo_client_dgram.py

sending "This is the message.  It will be repeated."
waiting to receive
received "This is the message.  It will be repeated."
closing socket

$
```

- [index](https://pymotw.com/2/genindex.html)
- [modules](https://pymotw.com/2/py-modindex.html) |
- [next](https://pymotw.com/2/socket/uds.html) |
- [previous](https://pymotw.com/2/socket/tcp.html) |
- [PyMOTW](https://pymotw.com/2/contents.html) »
- [Internet Protocols and Support](https://pymotw.com/2/internet_protocols.html) »
- [socket – Network Communication](https://pymotw.com/2/socket/index.html) »