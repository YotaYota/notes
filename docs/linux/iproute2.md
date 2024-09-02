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

# ip

`ip link show`: information about network interface device. Shows information on **OSI layer 2** ("data link layer").

**Note**: `ip -d link show` shows extra information. Can also be combined with types, eg `ip -d l show type bridge`

`ip address show`: Same information as `ip l` but also **OSI layer 3** ("network layer").

`ip route show` the connection to other ip networks

default gateway and routes available on system.

A default gateway needs to be on the same network.

The lower the metric, the higher the priority for that network interface.
