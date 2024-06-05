# Network

- `ip link list`
- `ip address show`
- `ip neigh show` ARP table
- `ip route show`

Default gateway is know throught the word **via** in the output.

## OSI

|||
|-|-|
|**Userspace**||
|Layer 7|Application|
|Layer 6|Presentation|
|Layer 5|Session|
|**Kernel**||
|Layer 4|Transport|
|Layer 3|Network|
|Layer 2|Data Link|
|**Physical**||
|Layer 1|Physical|

## Routing Table

`ip rule list` lists the rules.

By default there are 3 tables: *main*, *local* and *default*. ip tool modifies main and local.

`ip route list table main`

route tables are in */etc/iproute2/rt_tables*.

`ip route get <ip>` shows kernel's routing for an ip.

### Example

```
echo 200 John >> /etc/iproute2/rt_tables
```

creates a table *John*.

```
ip rule add from 10.0.0.10 table John
```

creates a rule for *John* table.

Now `ip rule ls` will also list

```
32765:	from 10.0.0.10 lookup John
```

## IP

These IPs are reserved for private use ([RFC 1918](https://datatracker.ietf.org/doc/html/rfc1918))
```
     10.0.0.0        -   10.255.255.255  (10/8 prefix)
     172.16.0.0      -   172.31.255.255  (172.16/12 prefix)
     192.168.0.0     -   192.168.255.255 (192.168/16 prefix)
```

Similar for IPv6 is [RFC 4193](https://datatracker.ietf.org/doc/html/rfc4193).

loopback 127.0.0.1 address is so that the system can talk to itself and do self diagnostics.

## Random commands

```
sudo ss -nltpu
```

```
nstat
```

## DHCP

UDP, then Port 67 and 68 unicast.

1. DHCP Discover (broadcast)
2. DHCP Offer (broadcast beacuse no ip assigned yet): network information such as client ip, subnet mask, default gateway ip, dns ip, ip lease time, dhcp server ip
3. DHCP Request: approves the ip
4. DHCP Ack (broadcast) with same information as in offer

dhcpd deprecated in favor of kea.

## DNS

DNS - Domain Name System. Communicates on port 53.

Traditionally resolvconf, but replaced with systemd-resolved.

On Ubuntu `/etc/resolv.conf` is a link to `/run/systemd/resolve/stub-resolv.conf`.

systemd-resolved.service

```
resolvectl status
```

DHCP is to dynamically get an IP. It communicates on ports 67 and 68.

If DHCP, then DNS server will be set automatically.

```
dhclient
```

### letencrypt and ACME

letencrypt uses ACME protocol ([RFC 8555](https://datatracker.ietf.org/doc/html/rfc8555)).


## Network Interface

Each connection to a node is called a "network interface". Linux gives these names like "eth0". `ip a` lists them.

The first of theese `lo` (loopback) represents the linux host itself.

- `UP` means that the kernel thinks the interface is up
- `LOWER_UP` means that we have established a link at the physical layer; an electrical signal.

## Protocols

- TCP
- UDP
- ICMP
- BGP for updating routing tables (Bird)
