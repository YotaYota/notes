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


- DST address 6 bytes
- SRC address 6 bytes
- EtherType 2 bytes
- Payload
- Ethernet trailer is a *Frame Check Sequencenumber* (FCS) - 32 Bit.

|IEEE Standards|Name used by engineers|Speed/Frequency|
|:--|:--|:--|
|IEEE 802.3|Ethernet|10 Mbps|
|IEEE 802.3u|Fast Ethernet|100 Mbps|
|IEEE 802.3z or IEEE 802.3ab|Gigabit Ethernet|1000 Mbps|
|IEEE 802.3ae|10 Gigabit Ethernet|10 Gbps|
|IEEE 802.3ba|100 Gigabit Ethernet|100 Gbps|
|IEEE 802.3bs and IEEE 802.3cm|400 Gigabit Ethernet|400 Gbps|
|IEEE 802.11|Wireless|2.4 GHz, 5 GHz, and many more|

#### Error Control

Two types of action destination can take

1. Error detection or
2. Error correction.

#### CSMA/CD (IEEE 802.3)

`CSMA/CD` allows a device to recieve or transmit data, but not both simultaneously. This is called `Half-duplex` data transmission (like a walkie-talkie) (cf `Full-duplex`.) 

In half-duplex ethernet networks, there is a `Collision Domain`. When data is sent, device listens to a duplicated response as a confirmation. If the response is not the same, it assmes there has been a collission, and transmitting devices send a `jam signal` indicating other listening devices should disregard previous message, and makes other transmitting devices to set a random transmission timer.

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

#### Copper Wire

4 twisted pairs, connected to an RJ45 connector.


### MAC - Media Access Control

Uniquely defines a network interface (NIC) on the network.

Hexadecimal

`00:90:69:00:00:00`

The first 3 uniquely identifies the manufacturer.

A **Unicast** MAC address is an address pointing to a unique NIC.
- Starts with an even number: `00:`, `02:`, ...

a **broadcast** mac address is a special address for all devices.
- `FF:FF:FF:FF:FF:FF` targets all on the network, eg ARP.

**Note**: An ARP request is a broadcast that asks the device with a certain ip address to respond with its mac address.

**Note**: It is possible to configure in the switch which ports should belong to a *broadcast domain* (VLAN).

A **Multicast** MAC address is a special address for several devices; a *multicast group*.
- First bytes are always: `01:00:5E`

**Note**: Broadcast and flooding can cause loops.

---

## Modern Ethernet LANs

Switches' primary benefit is high-speed connectivity.

It saves MAC addresses in the **Ethernet Switching Table**. Saved in RAM memory.

If the switch recieve a frame with a DST MAC that is not in its table, it forwards out the frame out on all interfaces except the one it came in on. This is called **flooding**.
When the DST MAC responds, the switch will update its table so that it does not need to flood the next time.

### Spanning Tree (STP)

Allows to connect switches in a loop without causing a broadcast storm. The switches can communicate with each other and temporarly close links between them.

### VLAN

VLAN segments a network at Layer 2.

Creating multiple *broadcast domains* in the switch by creating VLANs. Ie frames with DST MAC `FF:FF:FF:FF:FF:FF` will only be sent to ports in the VLAN.

For a device to communicate from one VLAN to another, the traffic needs to be sent to a router, ie go through Layer 3.


### MAC Tables

There are two mechanisms for a switch to remove entries from its switching table:
- MAV Ageing timer: it removes the entry automatically after a certain period of time (defaults to 300 s)
- CLI
- (not recommended) unplugging a cable causes the switch to clear the table of all MAC addresses associated with that port

The ethernet switching table usually contains VLAN information as well.

```junos
show ethernet-switching table
```

```junos
clear ethernet-switching table interface ge-0/0/4.0
```

### Legacy Networks and Modern Solutions

Shared Bus Network: devices connect to the same network with a Tee connector.

Collission Detection: send a jamming signal and makes everybody stop sending data.

Carrier-Sense: detect if there is a signal on the wire before starting to send.

Half-duplex: Before switches, devices could only speak in Half-duplex. A limitation of this is that you cannot send and recieve data at the same time.

Full-duplex: Additional pair of wires. Switches are called switches because the switch from source device's outgoing cables to destination device's incoming cables.

**Note**: Full-duplex is default. Half-duplex exists for backward compitability.


### Repeater, Hubs and Bridges

Repeaters: amplifies signal to be able to reach longer distances. Used in eg Atlantic cables.

**Note**: Switches also repeats signals, so repeaters are not well used.

Hub (legacy): Simply repeats a signal and broadcasts it over all ports. Replaced by switches.

Bridge (legacy): Early version of switch that used half-duplex. It helped limit traffic by placing between hubs.

**Note**: Switches are sometimes called multiport bridges. And switches and bridges are at times used interchangeably.

## Cables

### Copper

Last long (~100 years)

Speed is lowered with distance.

Susceptible to electro-magnetic interference.

#### UTP and STP

Unshielded vs Shielded.

Shielded is more rigid because of metal that shields from EMI.

#### RJ45

RJ45 has 4 twisted pairs, some are recieve, some transmit. A switch switches from transmit to recieve.

**Note**: **Cross-over cables** is an ugly hack (when no switch is available) to connect one end's transmit directly to the other end's recieve and vice verca.

#### Console Port

Cannot be assigned an IP-address. It is for getting a direct connection into a device.

RJ45 on one end, either DB9 or USB (S is for serial) on the other.

### Fiber


**NEXT** Fiber Cables
