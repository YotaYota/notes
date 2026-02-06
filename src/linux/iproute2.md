# iproute2

iproute2 is a collection of userspace utilities for controlling and monitoring various aspects of networking in the Linux kernel. They communicate with the kernel using the **netlink** protocol.

iproute2 collection contains the following command-line utilities:

- `arpd`
- `bridge`
- `ctstat`
- `dcb`
- `devlink`
- `ip`
- `lnstat`
- `nstat`
- `rdma`
- `routef`
- `routel`
- `rtacct`
- `rtmon`
- `rtstat`
- `ss`: sockets
- `tc`: used for traffic control
- `tipc`
- `vdpa`

## `ip`

`ip link show`: information about network interface device. Shows information on **OSI layer 2** ("data link layer").

**Note**: `ip -d link show` shows extra information. Can also be combined with types, eg `ip -d l show type bridge`

`ip address show`: Same information as `ip l` but also **OSI layer 3** ("network layer").

`ip route show` the connection to other ip networks

default gateway and routes available on system.

A default gateway needs to be on the same network.

The lower the metric, the higher the priority for that network interface.

### Routing

All addresses in 127/9 network point to localhost.

Three classes of IP addresses:

- local
- connected networks
- remote (reach via router on directly connected link)

destination, gateway, interface.

route scope defines the visibility of a route. Primarily 3 types of scopes which are connected to the classes of IP addresses.:

- host (target specific IP addresses directly on the local machine)
```sh
ip route show scope host table all
```

- link (target IP addresses within directly connected network that don't go through a gateway or router)
```sh
ip route show scope link table all
```

- global (target IP addresses that are reachable through a gateway or router)
```sh
ip route show scope global
```

#### Routing Tables

Routes are grouped into tables.

*/etc/iproute2/rt_tables* contains the list of routing tables, mapping number to human readable name.

3 tables are available by default:

- local (255) (cannot be changed)
- main (254)
- default (253)

```
ip route show table local
```

Longest match wins. If there is a tie you can add a `metric` to the route which breaks the tie (lowest wins).
```
ip route add 1.1.1.1 via 192.168.0.1 metric 100
```

#### Policy-Based Routing

We can only route on destination address, we cannot route on source address. What we can do instead is to add a `rule` so when traffic arrives from a destination it uses a specific routing table.

**Example**:

Create routing table. Open file with routing table mappings; */etc/iproute2/rt_tables* and add new routing tables, eg 

```
100     wan1
200     wan2
```

Add rules. Then add rules to route traffic from specific source IP addresses to the corresponding routing tables:

```sh
ip rule add from 10.100.0.0/24 table wan1
ip rule add from 10.200.0.0/24 table wan2
```

Add default routes to the new routing tables:

```sh
ip route add default via <gateway1> table wan1
ip route add default via <gateway2> table wan2
```

Inspect with

```sh
ip route show scope global
ip route show scope global table wan1
ip route show scope global table wan2
```

and

```sh
ip route get 8.8.8.8 fibmatch
ip route get 8.8.8.8 from 10.100.0.1 fibmatch
ip route get 8.8.8.8 from 10.200.0.1 fibmatch
```

#### Load balancing

Set ICMP hash policy to layer 4
```sh
sysctl -w net.ipv4.fib_multipath_hash_policy=1
```

set 2 nexthops for the same destination with the same metric
```sh
ip route add <ip> nexthop via <gateway1> dev <interface1> weight 1 \
             nexthop via <gateway2> dev <interface2> weight 1
```

#### Unreachable, Prohibited, Blackhole

```sh
ip route add unreachable <ip>
ip route show scope global
```

The system will respond with an ICMP "destination unreachable" message to the sender and drops the package.

```sh
ip route add prohibit <ip>
ip route show scope global
```

The system will respond with an ICMP "administratively prohibited" message to the sender and drops the package.

```sh
ip route add blackhole <ip>
ip route show scope global
```

Silently drops the package.

**Note**: in BGP, if we want to advertise a route that is not in the routing table, we can add a blackhole route for that prefix, then BGP can advertise it.

### Output Options

- `-br (--brief)`
- `-o (--oneline)`
- `-j (--json)`
- `-p (--pretty)`

## `bridge`

A Linux Bridge is a kernel module that behaves like a network switch.
It forwards packets between interfaces that are connected to it.

"Virtual Bridge Interface" ("bridging interfaces")

