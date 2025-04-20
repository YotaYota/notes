# JNCIA-Junos

- `LAN` Local Area Network


## Module 2: Building Ethernet LANs

- Basic Ethernet Operations
- Repeater
- Hub
- Bridge
- Ethernet Switch Operation

### Ethernet

`Ethernet` is a set of standards for LAN Network technology operating at Layer 1 and 2.

Ethernet uses *Carrier Sense Multiple Access with Collision Detection* (`CSMA/CD`) as its MAC protocol. It regulates the converation.

`Ethernet II` and `IEEE 802.3` frames differ slightly. Ie, the latter has length and LLC allowing other protocols than IP, where Ethernet II has Type. The former is more widely used.

Data sent over ethernet must reach its destination within a maximum timeframe, this imposes restrictions on cable lengths, eg for fibres it's about 10km, and Gigabit ethernets it's about 100m.

#### CSMA/CD (IEEE 802.3)

`CSMA/CD` allows a device to recieve or transmit data, but not both simultaneously. This is called `Half-duplex` data transmission (like a walkie-talkie) (cf `Full-duplex`.) 

In half-duplex ethernet networks, there is a `Collision Domain`. When data is sent, device listens toa duplicated response as a confirmation. If the response is not the same, it assmes there has been a collission, and transmitting devices send a `jam signal` indicating other listening devices should disregard previous message, and makes other transmitting devices to set a random transmission timer.

More devices on a Network Segment increases risk of collision, thereby degrading performance.

---

In 1980s a Shared Bus was used.

`Network Segment`: (In this course) Group of network devices that compete with each other for access to the wire.

`Physical Cable Segment`: Actual physical wire.

`Network Interface Controller (NIC)`: Computer hardware component. Used interchangeably with Network Adapter, Network Card and LAN Adapter.

`Media Access Control (MAC)`: Layer 2. Controlls access to physical wire. Burnt into the NIC on a PROM chip. 48-bit or 6 Bytes - First 3 bytes identifies the manufacturer OUI (Juniper has `00:90:69`).

Sending Ethernet frame to one Recieving device:

1. Compares Destination MAC with its own.
2. Checks the CRC
3. Determines frame Type
4. Strips off the ethernet frame and forwards the data to the upper layer protocol.

If device wants to send an Ethernet frame to many Recieving device it uses the broadcast address, known as `group addess` or `multicast MAC address`.

