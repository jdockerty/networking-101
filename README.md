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

Essentially, they are the same thing though; you'll more commonly see the TCP/IP model, but people will refer to things using the OSI layers, so this is something to keep in mind.
