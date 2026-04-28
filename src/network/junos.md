# JNCIA-Junos
"Networking Technology Foundations" 


- `LAN` Local Area Network

## Module 2: Building Ethernet LANs

- Basic Ethernet Operations
- Repeater
- Hub
- Bridge
- Ethernet Switch Operation

### Ethernet

`Ethernet` is a set of standards for LAN Network technology operating at Layer 1 and 2.

Ethernet uses *Carrier Sense Multiple Access with Collision Detection* (`CSMA/CD`) as its MAC protocol. It regulates the conversation.

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

1. Error detection, or
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

If a device wants to send an Ethernet frame to many recieving devices, it uses the broadcast address, known as `group addess` or `multicast MAC address`.

### MAC - Media Access Control

Uniquely defines a network interface (NIC) on the network. Assigned by hardware manufacturers.

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

VLANS typically map to an IPv4 Network.

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

4 twisted pairs, connected to an RJ45 connector.

Last long (~100 years)

Speed is lowered with distance.

Susceptible to electro-magnetic interference.

Can transfer electical pulse ~100 meters.

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

Fiber-Optic cables can transfer data faster and farther than copper cables.

Can transfer light pulse ~100 km.

Not susceptible to electro-magnetic interference.

NW devices rarely comes with built-in fibre ports, but they come with empty slots for **SFPs** (small form-factor pluggable). SFPs comes in different varieties.
More fragile than copper cables.

- Transmission mode:
    - Single mode (**yellow**): Longer distances, minimal loss (no edge bouncing; less energy loss). Stronger laser needs more energy.
    - Multimode (**aqua**): Bigger core, multiple modes for light to travel, shorter distances. Desires less energy for laser.
- Uses different types of connectors:
    - SC: square connector
    - LC: Lucent connector
    - MPO: Multifiber push-on
- Bandwith: From 1Gpbs to 800 Gpbs.
- Color: Can indicate transmission mode.
- Conditions of use: underground, water, etc.
- Armoring

## IPv4 Addresses and Networks

ARPA created the world's first WAN in 1960s. By requirement it

- must use packet switching,
- controll of network must be decentralized,
- it had to be redundant.

IP protocol (and others) was born from this.

### IP Address Orgination

Each device connected to a network has to have an IP address configured. Two ways: either manual or **DHCP** (Dynamic Host Configuration Protocol).

DHCP server: has a pool of IP-addresses.

1. DHCP client: Broadcast
2. DHCP server responds with IP form its pool and also with additional config.

### IPv4 Address

32 Bit address, organized in octets separated by dots. Approx 4.3 billion possible addresses, of which approx 3 billion are public.

IP addresses are managed globally by the **IANA** (Internet Assigned Numbers Authority). Under IANA there are 5 **RIRs** (Regional Internet Registries), which in turn assign public IPs to local internet providers.

Local devices usually use private addresses which are **NAT**:ed (Network Address Translation) to the router's public address.

Private ranges are (RFC 1918):
- 10.0.0.0/8
- 172.16.0.0/16
- 192.168.0.0/16

### IPv4 Networks

A Network Interface needs 2 addresses to connect to a network: MAC and IP.

An IP address has a **network** portion and a **host** portion.

Networks are defined in either slash notation or decimal notation.
In slash notation `/x` means that the first `x` bits constitutes the network portion.

If two devices on different networks want to communicate, they must forward their traffic to a router.

Keyword `inet` is related to the IPv4 network family, eg

```junos
set interfaces ge-0/0/0 unit 0 family inet address 172.16.10.254/24
```

### IPv4 Data Transmission

Server A looks at the network portion of the DST IP to see if they are on the same subnet.
If they are, A does an ARP broadcast to discover host B's MAC address.
After ARP response, layer 2 ethernet encapsulation can be perfomed.
Now server A can forward traffic to a switch (not a router).

If they are not on the same subnet, A does an ARP broadcast to discover the MAC address of it's default gateway or router.
After ARP response, layer 2 ethernet encapsulation can be perfomed. And it sends the package to the router's MAC address.
When the package arrives at the router, the router can perform an ARP broadcast on the correct network.

3 primary values you configure on a device:

- IP address
- Netmask
- default gateway

A **Default gateway** is an ip address on the local network the device can use to communicate with devices on other networks.

### Unicast, Multicast and Broadcast

The first address in an IPv4 Network is reserved as the **network address**. Eg 192.16.0.0 in 192.16.0.0/16.

The last address in an IPv4 Network is reserved as the **broadcast address**. Eg 192.16.255.255 in 192.16.0.0/16.

Therefore no device can be assigned the first and last address in an IPv4 network.

Three types of addresses:

- Unicast, one-to-one
- Broadcast, one-to-all (eg ARP).
- Multicast, one-to-many

For a broadcast, the MAC address is FF:FF:FF:FF:FF. A switch forwards to all ports, while a router drops.

**Note**: Broadcast traffic is not forwarded outside a network, therefore it's called a **Broadcast Domain**.

Multicast uses a multicast IPv4 address in the range 224.0.0.0 - 239.255.255.255.

A Multicast streamer will inform other host a multicast IPv4 destination that they can request to recieve data for. The hosts will inform the switch that they are interested in the address. The switch will replicate the packages to that IP so it reaches all interested hosts.


## ARP

1. ARP Request. IP Broadcast: Who has <IP>? DST IP is broadcast, and DST MAC is F::F.
2. ARP Reply. Device with matching IP responds with it's MAC and IP.
3. Device saves IP and MAC in its ARP Table. (has an age) `ip -s neigh`


---

## IPv4 Header

The IPv4 Header contains 13 fields.

The typical size of the header is 20 bytes, but it can be up to 60 bytes if options are used.

```

    0                   1                   2                   3
    0 1 2 3 4 5 6 7 8 9 0 1 2 3 4 5 6 7 8 9 0 1 2 3 4 5 6 7 8 9 0 1
   +-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
   |Version|  IHL  |      DS       |          Total Length         |
   +-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
   |         Identification        |Flags|      Fragment Offset    |
   +-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
   |  Time to Live |    Protocol   |         Header Checksum       |
   +-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
   |                       Source Address                          |
   +-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
   |                    Destination Address                        |
   +-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
   |                    Options                    |    Padding    |
   +-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+

```
*source: [RFC791](https://datatracker.ietf.org/doc/html/rfc791)*

- **Version**: 4 for IPv4.
- **IHL**: Internet Header Length. The minimum value is 5 (20 bytes) and the maximum value is 15 (60 bytes). If there are no options, the IHL is 5.
- **DS**: Differentiated Services. Used for setting priority, during network congestion.
- **Total Length**: Total length of the packet, including header and data. Minimum value is 20 (header only) and maximum value is 65535.
    - **Note**: **Maximum Transmission Unit** (MTU) limits the maximum packet size. defaults to 1500. It will be fragmented if allowed, otherwise dropped.
- **Identification**: Each packet generated by an application is assigned an ID. This is used for fragmentation and reassembly for the correct application.
- **Flags**: 3 bits.
    - The first bit is reserved and must be set to 0.
    - **DF**: Do not Fragment
    - **MF**: More Fragments. Set to 1 for all fragments except the last one.
- **Fragment Offset**: If the packet is fragmented, this field indicates where in the original packet this fragment belongs.
- **TTL** each router will decrease the TTL by 1. If TTL reaches 0, the packet is discarded and send ICMP time exceeded message. Note that this means that the **Header Checksum** also needs to be recalculated at each hop.
- **Protocol**: Indicates the protocol of the packet in layer 4, eg TCP (6), UDP (17), ICMP (1).
- **Options**: Optional field, used for special purposes, eg security, record route, timestamp, etc. If options are used, the header length is greater than 20 bytes.

## IPv4 Network Subnetting

**CIDR** (Classless Inter-Domain Routing) is a method for allocating IP addresses and routing IP packets. It replaces the older system based on classes A, B, and C.

**Note**: CIDR allowed subnetting, you are not limited to the fixed sizes of class A, B, and C networks. You can create subnets of any size.

CIDR introduced **VLSM** (Variable Length Subnet Masking).

The point of subnetting an address range is to use the address space more efficiently.

||||||||||
|:--|:--|:--|:--|:--|:--|:--|:--|:--|
|Power|2^7|2^6|2^5|2^4|2^3|2^2|2^1|2^0|
|Value|128|64|32|16|8|4|2|1|

**Note**: there are 2 subnets of `/(n+1)` in `/n`.

**Point-to-Point Network**: A network that connects exactly two devices.

    - It can be subnetted with a /30, which allows for 2 usable host addresses (the first and last addresses are reserved for network and broadcast).
    - Most provders support /31, which allows for 2 usable host addresses without the need for a broadcast address.

## IPv6 Addresses
Consists of 32 Hexadecimal characters. Where 1 hex is 4 binary, so an IPv6 address is like an IPv4 address with 128 bits. That is 2^128  possible addresses.

Divided into 8 groups of 4 hex; a **hextet**.

- Leading 0s can be omitted in a hextet
- Continous 0s can be abbreviated by `::` once

The slash is called a **prefix**.

- _Global Unicast Addresses_ (GUA): `2000::/3` public addresses
- _Unique Local Addresses_ (ULA): `fc00::/7` private addresses 
- _Link-local_: `fe80::/10` automatically assigned to NICs. Only use to connect to other devices that are also connected to the same link. Not broadcasted, nor routed.
- _Multicast_ `ff00::/8`
- `2001:db8` are reserved for documentation.

There is no need to use NAT with IPv6.


### Anycast

Can be assigned to multiple devices, a router will forward traffic to the nearest device with that address. Eg CDNs.

### Neighbor Discovery Protocol (NDP)

IPv6 does not use ARP - it uses Neighbor Discovery Protocol (**NDP**) which is more efficient.
So if a machine deems an address to be on the same network, it will perform NDP to get its MAC address.

NDP is a collection of 5 ICMPv6 message types used to discover hosts and routers on an IPv6 network:

- _Neighbor Solicitation_ (**NS**). Request MAC from host
- _Neighbor Advertisment_ (**NA**). Response to NS
- _Router Solicitation_ (**RS**). Request configuration from router
- _Router Advertisment_ (**RA**). Router configuration
- _Redirect_ (**RR**). Informs on better next-hops.

NDP does not use broadcast (it was one of the goals with IPv6 to eliminate broadcast traffic). Instead it uses multicast.
For NS a host adds the last 6 digits of its IPv6 address to the well-known group `ff02::1:ff00:0/104`, to get its multicast group ip-address.

**Example**: A device with IP `2001:db8:0:10::2`  would add it's last 6 digits `...0000:02` to `ff02:::1:ff00:0000/104` to get the address `ff02::1:ff00:2/104`.

**Note**: `ff02:::1:ff00:0000/104` is called _Solicited-Node Multicast Address_.

Devices can figure out other device's multicast group IP-addresses as well.

For layer 2 encapsulation in NS (MAC is not known yet), the sender calculates the multicast MAC destination address in a similar way; it adds the 6 last digits of the destination device's IP to `33-33-FF`.

Multicast Organisationally Unique Identifiers range (OUI): `33-33-00` to `33-33-FF`.

When IPv6 is operational on a host, it sends a RS Message to the link-local well-known multicast "all-routers" group `ff02::2`. The router responds with a RA message (with eg default gateway and router MAC).

**Note**: `ff02::2`, all-routers, is a multicast address which represents all IPv6 routers on the local link.

IPv6 maintains a Neighbors table (similar to IPv4 ARP table).

### IPv6 Header

IPv6 Header has a fixed length of 40 bytes.

``` +-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
   |Version| Traffic Class |           Flow Label                  |
   +-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
   |         Payload Length        |  Next Header  |   Hop Limit   |
   +-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
   |                                                               |
   +                                                               +
   |                                                               |
   +                         Source Address                        +
   |                                                               |
   +                                                               +
   |                                                               |
   +-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
   |                                                               |
   +                                                               +
   |                                                               |
   +                      Destination Address                      +
   |                                                               |
   +                                                               +
   |                                                               |
   +-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
```

source: [RFC 8200](https://datatracker.ietf.org/doc/html/rfc8200)

- Payload Length
- Hop Limit: same as TTL in IPv6
- Flow Label: added by source device, enables routers to quickly classify and manage traffic flows
- Next Header: Identifies the type of header immediately following the IPv6 header.

Extension headers examples:

- Hop-by-Hop Options
- Routing
- Fragment
- Authentication Header (AH)
- Encapsulation Security Payload (ESP)
- Mobility
- Host Identity Protocol
- Shim6 Protocol

**Note**: Extension headers enables the possibility to extend IPv6 capablity over time.

Comparison with IPv4:

- Header Length not exist; IPv6 header is fixed size.
- Header Checksum is omitted
- Options are replaced with extension headers.
- Identification, Flags and Fragment Offset: There is an extension to the IPv6 header that enables fragmentation.

#### IPv6 Fragmentation (path MTU dicovery - PMTUD)
If a packet is larger than the MTU, it needs to be fragmented. But in IPv6 only the source device can perform fragmentation. If a device along the chain is limited by MTU it sends back an **ICMP Unreachable** to the sender with its MTU. The end result is that the sender fragments according to the lowest MTU on the path; which is more efficient.

## Subnetting IPv6 Networks
Each hexadecimal number can be represented in 4 bits - also called a _nibble_ (a half-byte).

### Hex to Binary
|  Hex Char |  Dec Value |  Binary Value |
|---|---|---|
|  B |  11 = 8 + 2 + 1 |  1011 |
|  A |  10 = 8 + 2 |  1010 |
|  D |  13 = 8 + 4 + 1 |  1101 |
|  3 |  3 = 2 + 1 |  0011 |

Common Subnet Sizes:

- ISP /48
- LAN /64

- **SLAAC**: StateLess Address Auto-Configuration.
    - EUI-64, uses host's MAC (not recommended).
    - Privacy Extensions for SLAAC, random and more secure, needs an extra check for uniqueness.
    - Only works for /64

Point-to-Point: /127 [RFC 6164](https://www.rfc-editor.org/rfc/rfc6164). Makes the link safer since noone else can connect and sniff.

### Routers
The primary role of a router is to forward packets between networks. The router is the "default gateway".

Switches can also forward data, but they recieve frames and consult the dst MAC address and their ethernet switching table - routers consult the dst IP address and their routing table. Many routers can switch, and many switches can route - but they are optimized for their own layer.

All machines know their IP and the LAN's subnet mask. They can calculate if a destination lives outside the subnet.

There are 3 primary methods that routes can use to learn about the networks.

- By default a router only knows about the networks it's directly connected to "**directly connected routes**".
- Manually added routes to a routers routing table: "**static routes**".
- Enable routing protocols that allow routers to dynamically communicate information about the routes on the network: "**dynamic routes**".

**Note**: static routes are easy to understand and preferable in very small networks. But they lack flexibility (eg find alternate paths).

Dynamic Routing Protocols, eg

- OSPF
- IS-IS
- BGP
- RIP

#### Routing Tables

A routing table entry consists of:
- Target network
- interface
- next-hop

Routers store all learned routes from all source, and its selects the best routes for forwarding: "active routes". There are two criteria for chosing the active route:

- local preference (lowest)
- longest prefix match lookup (most number of bits in common)

**Note**: Junos maintains two separate routing tables, **inet.0** (IPv4) and **inet6.0** (IPv6). `show route table inet.0`.

`ip route show` or `ip -6 route show`

Example:

```bash
$ ip r show
default via 192.168.10.1 dev wlp0s20f3 proto dhcp src 192.168.10.143 metric 600 
192.168.10.0/24 dev wlp0s20f3 proto kernel scope link src 192.168.10.143 metric 600
```


- `default via 192.168.10.1 dev wlp0s20f3` means it has a default route with a next-hop of `192.168.10.1` and it uses interface `wlp0s20f3`.
- `proto dhcp` means it was learned by DHCP. `proto static` means it was manually added. `proto kernel` means route was installed by kernel during interface configuration. `proto ra` is from router advertisment.
- 192.168.10.0/24 is a directly connected network. 

### Troubleshooting Routers

`ping` sends ICMP Echo Request (waiting for ICMP Echo Reply).

`traceroute` sends consecutive number of ICMP messages (increasing TTL as you go). Doesn't show return path (do a traceroute from the other end).

## Transport Layer
Src and dst port enables data to be delivered to the correct application.
65535 ports in total

- 0 - 1023 are well-known ports assigned by IANA.
- 1024 - 49151 user or registred ports requested by IANA.
- 49152 -65535 dynamic or ephermal ports.

One approach is to troubleshoot bottom-up in the OSI Model. Eg if ping works, it means layers 1 through 3 are working, and further troubleshooting can begin at layer 4.

## Application Layer
- DHCP - Dynamic Host Configuration Protocol
- DNS - Domain Name System
- HTTP(S) - Hypertext Transfer Protocol (over Secure Sockets Layer)

### DHCP
DHCP uses Client/Server model. Server keeps track of all DHCP reservations.

Client uses UDP port 68.
Server uses UDP port 67.

#### DHCP DORA
DORA process (**D**iscover, **O**ffer, **R**equest, **A**CK):

1. `DHCP Discover` (Broadcast) - since Client does not have an IP at this moment, uses src IP 0.0.0.0.
2. Only Server responds wiht `DHCP Offer` (Unicast) to Client's MAC address. The dst IP is the offered IP.
3. Client sends `DHCP Request` (Broadcast) with src IP 0.0.0.0 to accept the address.
4. Server sends `DHCP ACK` (Unicast) with remaining IP configuration for the Client, eg: subnet mask, default gateway, DNS server, Lease Time

In JunosOS you set `address-assignment pool`.

### DNS
Uses UDP, port 53.

Hierarchy. At the root there are 13 "root DNS servers", each server refer you to the next level down in the hierarchy (unless the answer is cached).

The DNS Client on your device will resolve the name and ask its DNS Server, the DNS Server will ask the root server, and it will refer to the DNS Server one step further down the hierarchy

zone files: hostnames and ip addresses.

|  Hierarchy Layers |  URL section |
|---|---|
|  Root Domain |  . |
|  Top-Level Domain (TLD) |  .net |
|  Domain Name |  juniper.net |
|  Hostname |  learningportal |

**Note**: Hostname + Domain Name = FQDN

The 13 well-known root DNS servers point to DNS servers that are aurhorative for the top-level domain.

**DNS Recursive Resolver**: Recieves DNS queries from hosts and tries to find the answer. Resolvers can **cache**.

**DNS Server**: Stores authorative DNS records in a zone file.

**Stub Resolver**: The term for a client that has the IP address of a local DNS server configured. And it can cache. A browser will forward the request to the stub resolver

### HTTP
With HTTP/3 content is run over UDP, using QUIC protocol, uses port 80 and 443.

QUIC was built since TCP handshakes was becoming the bottleneck of HTTP content. On a request, the webpage is delivered over UDP, then QUIC can handle counting packets and encryption.

## OSI Model
Application Layer 7: DHCP, DNS, HTTP, HTTPS
Transport Layer 4: TCP, UDP
Network Layer3: IPv4, IPv6
Data Link Layer 2: Ethernet
Physical Layer 1: Copper, Optics

**Note**: If Host A can ping Host B, you can deduce there are no cable problems (Layer 1), and you know MAC and IP works (Layer 2 and 3). So you can start to troubleshoot in Layer 4 and above.

Firewalls can be eg Layer 4 or Layer 7, using the information available up to that level (layer 7 is more intrusive).

HTTS example. Application layer creates data to send, presentation and session encrypt HTTPs traffic.
