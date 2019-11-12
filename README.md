# Networking Crash Course 
*Written content for networking class to 1st years.*

## OSI and TCP/IP Models ##
These are just conceptual models for referencing when a protocol is being designed, it gives the engineer an open reference and provides standardisation to how it should function so that it can be used on various different hardware. You don't have to get too bogged down on *exactly where* a protocol would sit in the stack, some of them can be vague, but it is nice to have a reference to what you're talking about with someone, i.e. being able to say *'it is a layer 3 protocol'* means that you have some idea it functions using an IP address or doesn't utilise a transport mechanism like TCP or UDP to traverse a network, whereas if something is a layer 7 protocol (e.g. HTTP) you know it may be built on top of underlying protocols like IP and TCP in order for it to function.

### OSI (Open Systems Interconnect) Model ###
![OSI Model](https://www.cloudflare.com/img/learning/ddos/what-is-a-ddos-attack/osi-model-7-layers.svg)
> Image from Cloudflare and for further reading.
> https://www.cloudflare.com/learning/ddos/glossary/open-systems-interconnection-model-osi/

### TCP/IP (Transmission Control Protocol over Internet Protocol) Model ###
![TCP/IP Model](https://www.studytonight.com/computer-networks/images/Figure35-1.png)

The TCP/IP model takes multiple layers of the OSI model and puts them into a single layer, simplifying it. 
* Application (7), Presentation (6), and Session (5) layers  --> Application
* Data-Link (2) and Physical (1) --> Network Access

Essentially, they are the same thing though; you'll more commonly see the TCP/IP model, but people will refer to things using the OSI layers, so this is something to keep in mind, although mentioning a term removes the ambiguity, e.g. *'a transport layer protocol'*, you know where you are, in a logical sense, without thinking of different layer numbers for the models.

A good thing to remember too, is what type of data is being transmitted or constructed at a particular OSI reference layer:
* Layer 4 - Segments
* Layer 3 - Packets
* Layer 2 - Frames
* Layer 1 - Bits on the wire

From these reference models we can delve further into fundamental protocols and understand how they function, given their respective layers.

### Address Resolution Protocol (ARP) - Layer 2 ###

You will see ARP occur quite frequently within a LAN when a machine does not know the corresponding MAC address for a particular IP address. Since we are referring to a LAN, this means we're dealing with MAC addresses, a physical address which is burnt into the device and unqiuely identifies it.
This is considered layer 2 because the protocol itself does not concern itself with IP addresses, a logical addressing mechanism, for traversing a network, only the physical (MAC) address is used for construction of a frame. Owing to this, the traffic at layer 2 will not *traditionally* traverse the internet as it is not internet routable and it will only stay within your local LAN segment. 
*(The caveat is concerning the use of VPNs, as they can extend a L2 segment, although you can research that further if you find it interesting)*

> Further reading - https://learningnetwork.cisco.com/docs/DOC-23702

### Internet Protocol (IP) - Layer 3 ###

IP is quite familiar to most people, it provides a logical addressing structure for devices to communicate with each other. An IP address will look something like 10.1.1.5, this is a single address. A range of IP addresses are referred to as a *prefix* and will be shown in the form of something like 10.1.1.0/24, the /24 denotes a subnet mask of 255.255.255.0, this indicates where the network address is (10.1.1.0, the identifier for the network starting) and where the broadcast address is, this is the last address within a particular range of addresses (pinging this would mean you're pinging each device in that particular prefix range).

This brings us onto the concept of public and private address ranges.

The private ip address ranges are:
* 10.0.0.0 to 10.255.255.255 (this would be 10.0.0.0/8)
* 172.16.0.0 to 172.31.255.255 (172.16.0.0/12)
* 192.168.0.0 to 192.168.255.255 (192.168.0.0/16) 

* 127.0.0.0 to 127.255.255.255 (127.0.0.0/8)  are reversed for loopback/testing purposes on a local machine, you'll hear this referred to as *localhost* sometimes.

The most familiar are probably 192.168.1.1 or 192.168.1.254, this is the IP address for accessing the web interface of your home router, although different manufacturers may vary their scheme.
Private addresses are used as a logical addressing scheme for a private/local network. Traffic within it, such as the university campus, can use the other private addresses to communicate with other devices that are using the same addressing scheme. For example, your home network may use 192.168.1.0/24 (192.168.1.0 to 192.168.1.255 full range of addresses), your PC may be assigned 192.168.1.16 and your router has 192.168.1.1, from here you could ping the router and receive a reply - communicating into another network entirely would require a device with routing capabilities e.g. a router

Public ranges are internet routable, you can buy a block of public addresses for your own business use or use services like AWS to utilise a cloud providers pool for your applications. A public address ranges from anything in the range, excluding those above, of 1.0.0.0 to 223.255.255.255. 224.X.X.X and above are for multicast addresses and are out of scope for basic understanding of IP.

> RFC 1918 for private addressing scheme - https://tools.ietf.org/html/rfc1918

> General IP address and subnetting usage - https://support.microsoft.com/en-gb/help/164015/understanding-tcp-ip-addressing-and-subnetting-basics

### Transmission Control Protocol (TCP) and User Datagram Protocol (UDP) - Layer 4 ###

TCP is a connection-oriented transport mechanism used to guarantee segment delivery across a network. TCP is a very feature-rich protocol, providing features such as:
* Reliable delivery through Acknowledgement packets
* Multiplexing via port numbers
* In-order delivery
* Flow control through windowing

#### What does connection-oriented mean? #### 
This is in relation to the 3-way handshake which establishes a TCP connection before any data is sent, this must succeed before any data is exchanged.

![TCP 3-way handshake](https://www.cloudflare.com/img/learning/cdn/tls-ssl/tcp-handshake-diagram.png)

Once the handshake has been completed, an end to end connection exists between the 2 hosts. Ensuring that a bidirectional stream of data can be sent.

When the data that was needing to be sent has succeeded, the client may send a packet with the FIN (Finish) bit set meaning that the connection will close.

#### Reliable Delivery ####

Acknowledgements are what ensures that delivery is reliable. Acknowledgements are simply just another packet which is sent, practically an empty packet, with the ACK bit set in the TCP header - ACK packets can also *piggyback* off other segments being sent, so that an additional packet is not needed to be sent by itself.

An acknowledgement ensures that the delivery is reliable since the data which is sent is confirmed that it has arrived, if the acknowledgement is not received by the sender then they will know it has not be received (since they expected an acknowledgement for it), so it can be retransmitted.

#### Multiplexing through port numbers ####

A port number is a concept used to allow multiplexing, simply put, this allows a device to differentiate different connection streams to a different services that is coming from the same IP address. Port numbers provide a way to uniquely identify a particular stream and ensure it receives the correct service, e.g. a destination of port 80 to a particular IP would mean it was a webserver, connecting with HTTP to receive some sort of web data.

*You don't need to memorise port numbers since you can just Google them if you're unsure, although you eventually just pick up the standard ones you hear about a lot and commit those to memory naturally. HTTP = 80, SSH = 22, HTTPS = 443 etc.*



>Further reading on TCP - https://tools.ietf.org/html/rfc793`

>Further reading on UDP - https://tools.ietf.org/html/rfc768`

#### In-order Delivery ####

Segments are held in a buffer until they all arrive, at this point they are processed. This ensures they are processed in the order that they were sent.

#### Flow Control ####

