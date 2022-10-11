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


## The Network Edge

### Access Networks

The closest router for a given end system is called an *edge router*.

This edge router may be connected to the network core through different technologies.

In homes, those technologies often are:

- **DLS** (Digital Subscriber Line): Telephone and Internet sharing the same cable with frequency division and multiplexing. Asymmetric (upload speed != download speed)
- **Cable Internet**: Internet and TV sharing the same cable. Fiber and coaxial may be combined (which is called **HFC** (Hybrid Fiber Coax)). Similar properties as DLS
- **FTTH** (Fiber to the Home): Similar scheme as before, greater speeds.
- **Satellite Link**: Available theoretically everywhere, in the beggining slower but improving.

In enterprises, more complex schemes are used to better satisfy the needs of the network.

[This website](https://wigle.net) crowdsources detected Wifi access points around the World.

For bigger areas, such as whole cities, technologies loke LTE, 3G, etc., are used. This depend on a quite different technology than home access points.

### Physical Media

When trasmitting data through a network, this transmission can be done in two types of media:

- *Guided* media: Through a solid medium
- *Unguided* media: Through the atmosphere or outer space


In guided media, installation costs are much greater than material costs.

Some guided media examples are:

#### Twisted-Pair Copper Wire

Used initially by telephones. The twisting prevents interferences. Some cables can get to 10 Gbps. These are predominant in LAN networking
#### Coaxial Cable

Used in TV networks. Concentric wires, shielded. Usually used as a guided shared medium

#### Fiber Optics

Immune to interference, low signal attenuation, very fast and hard to tap. Used in long tranmissions and expensive networks. Naming of *OC-[n]*, where *speed = n · 51.8Mbps*

Some unguided media examples are:

#### Terrestial Radio Channels

Its characteristics are highly dependant on the environment. They can be classified by the area they cover:

- Short distance: Few meters (e.g. Bluetooth)
- Local Areas: Tens or hundreds of meters (e.g. WiFi)
- Wide Areas: Tens of kilometers (e.g. 3G)

Due to being unguided, some problems may arise:

- Path loss
- Shadow fading (on passing through objects)
- Multipath fading (on reflecting on objects)
- Interferences with other transmissions

#### Satellite Radio Channels

Two ground stations are linked through a satellite. This satellite can be:

- Geostationary: Higher latency (280 ms)
- LEO (Low Earth Orbiting): Many are needed to cover an area.
