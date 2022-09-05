Computer Networking: A Top-Down Approach, Sixth Edition - James F.Kurose & Keith W.Ross

# Computer Networks and the Internet

## What is the Internet

### A Nuts and Bolds Description

The internet can be seen as a set of connected devices in a certain way. Some notation:

- **Host / End System**: A server, PC, tablet... connected to the network
- **Communication Link**: Medium to communicate devices, like copper, fiber, radio spectrum...
- **Packet**: Information sent though the network, consisting on the header and the data
- **Packet Switches**: Devices that recieves packages and forwards them accordingly
	- Routers: Used in network core
	- Link-layer switches: Used in access networks
- **Route**: Path between two hosts

Organisations that define standards:
- **ITFS**: IP, TCP, HTTP, SMTP...
- **IEEE**: Wifi, Ethernet...

### A Services Description

The Internet can also be seen as an infrastructure that provides services to applications.

These applications are said to be *distributed*, as more than one end system is needed. These end systems communicate through APIs.

### What is a Protocol?

All communication between end systems require the use of a predefined protocol. A formal definition would be (citation):

- **Protocol**: A protocol defines ther format and the order of messages exchanged between two or more communication entities, as well as the actions taken on the transmission and/or receipt of a message or other event.
