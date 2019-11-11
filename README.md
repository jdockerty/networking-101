# Networking Crash Course 
*Written content for networking class to 1st years.*

## OSI and TCP/IP Models ##
These are just conceptual models for referencing when a protocol is being designed, it gives the engineer an open reference and provides standardisation to how it should function so that it can be used on various different hardware. You don't have to get too bogged down on *exactly where* a protocol would sit in the stack, some of them can be vague, but it is nice to have a reference to what you're talking about with someone, i.e. being able to say *'it is a layer 3 protocol'* means that you have some idea it functions using an IP address or doesn't utilise a transport mechanism like TCP or UDP to traverse a network, whereas if something is a layer 7 protocol (e.g. HTTP) you know it may be built on top of underlying protocols like IP and TCP in order for it to function.

### OSI (Open Systems Interconnect) Model ###
![OSI Model](https://www.cloudflare.com/img/learning/ddos/what-is-a-ddos-attack/osi-model-7-layers.svg)


### TCP/IP (Transmission Control Protocol over Internet Protocol) Model ###
![TCP/IP Model](https://www.studytonight.com/computer-networks/images/Figure35-1.png)

The TCP/IP model takes multiple layers of the OSI model and puts them into a single layer, simplifying it. 
* Application (7), Presentation (6), and Session (5) layers  --> Application
* Data-Link and Physical --> Network Access

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

Public ranges are internet routable, you can buy a block of public addresses for your own business use or use services like AWS to utilise a cloud providers pool for your applications. A public address ranges from anything in the range, excluding those above, of 1.0.0.0 to 223.255.255.255. An exception is for 127.0.0.0 - 127.255.255.255 

