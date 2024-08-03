# nftables

Successor of iptables.

Provides hooks to `conntrack`.

## Basics

Supports stateless and stateful rules.

Supports bitwise operators on packets.

Rules are processed from top to bottom until a match is found.
If no match is found, the default policy is found (if not explicitly stated it is deny).

Basic approach is to create a *table*, then a *chain*, then a *rule*. There are no predefined tables or chains in nftables, they have to be created.

Each command should include an *address family*, which are one of

- ip, IPv4 
- ip6, IPv6
- inet, internet (IPv4/IPv6)
- arp, ARP (IPv4 ARP packets)
- bridge, Bridge (L2)
- netdev, Netdev

Chains are *input*, *output* or *forward*.

### Install

```
sudo apt install nftables
systemctl enable nftables.service
```

```
sudo nft list ruleset
```

Check if enabled in the kernel with

```
lsmod | grep ^nf
```

To see the current configuration, use

```
nft list ruleset
```

Be sure not to have Xtables and iptables on at the same time

```
systemctl stop iptables ;\
 iptables --flush ; iptables --list
```

```
systemctl stop ip6tables ;\
 ip6tables --flush; ip6tables --list
```

*Note*: `nft flush ruleset` to disable `nftables`.

---

## Config

_/etc/nftables.conf_

```
#!/usr/sbin/nft -f

flush ruleset

table inet filter {
	chain input {
        # default drop
        type filter hook input priority 0; policy drop;

        ct state invalid drop

        # established, related
        ct state {established,related} accept comment "accept traffic originating from us"

        # loopback
        iif lo accept comment "accept loopback"
        iif != lo ip daddr 127.0.0.1/8 drop comment "drop connections to loopback not coming from loopback" 
        iif != lo ip6 daddr ::1/128 drop comment "drop connections to loopback not coming from loopback" 

        # accept neighbour discovery otherwise connectivity breaks
        icmpv6 type { nd-neighbor-solicit, nd-router-advert, nd-neighbor-advert } accept

        # accept ssh
        #ip saddr <ip> tcp dport 22 accept
	}
	chain forward {
        # default drop
		type filter hook forward priority 0; policy drop;
	}
	chain output {
        # default accept
		type filter hook output priority 0; policy accept;
	}
}
```

*Note*: Changes with `nft` CLI will dissappear after reboot unless explicitly saved. To save current setup use
```
nft -s list ruleset | tee filename
```

*Note*: Use `counter` keyword before verb and watch with `sudo watch -n 1 "nft -n list table inet filter"`

