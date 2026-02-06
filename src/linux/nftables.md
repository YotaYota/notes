# nftables

Successor of iptables.

Provides hooks to `conntrack`.

## Basics

Supports stateless and stateful rules.

Supports bitwise operators on packets.

Rules are processed from top to bottom until a match is found.
If no match is found, the default policy is found (if not explicitly stated it is deny).
Each is rules is processed from left to right; this has implications on eg `counter` placement.

Basic approach is to create a *table*, then a *chain*, then a *rule*. There are no predefined tables or chains in nftables, they have to be created.
In order for packets to traverse a chain, it need to be jumped to from another chain or attach to a Netfilter hook. Eg,

```
type filter hook input priority filter
```

Each command should include an *address family*, which are one of

- ip, IPv4
- ip6, IPv6
- inet, internet (IPv4/IPv6)
- arp, ARP (IPv4 ARP packets)
- bridge, Bridge (L2)
- netdev, Netdev

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

**Note**: `nft flush ruleset` to disable `nftables`.

---

### State

Stateful rules use `conntrack` module. It can be listed with

```sh
sudo conntrack -L
```

In nft syntax conttrack states can be used with eg

```
ct state established,related counter accept
```

## Config

There are three ways of configuring:

- `nft add` CLI commands
- `nft -f` with a file containing the ruleset
- `nft -i` for interactive mode

**Note**: `nft -f` is preferred since it allows nft to parse the whole file and apply it atomically, which is not the case with `nft add` CLI commands.

**Note**: Changes with `nft` CLI will dissappear after reboot unless explicitly saved. To save current setup use

```
nft -s list ruleset | tee filename
```

### Examples

Example _/etc/nftables.conf_

```
#!/usr/sbin/nft -f

flush ruleset

table inet filter {
    chain input {
        # attached to netfilter input hook. default drop.
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

#### Allow SSH

```
ct state established,related counter accept
ct state new tcp dport 22 counter accept
```

This will allow new SSH connections (by the second rule) and allow the connection to continue (by the first rule).

#### Named counters

```
flush ruleset


table inet filter {

    counter cnt_ssh {
        comment "SSH counter"
    }

    chain input {
        type filter hook input priority 0; policy accept;

        ct state established,related counter accept
        ct state new tcp dport 22 counter name cnt_ssh accept
        ct state new tcp dport 8080 counter name cnt_ssh accept
    }
}
```

#### Named Sets

```
flush ruleset


table inet filter {

    # A set containing elements with the correct type
    set allowed_ips {
        typeof ip saddr
        elements = { 172.27.97.2, 172.27.97.3 }
    }

    chain input {
        type filter hook input priority 0; policy accept;

        # A rule using the set with @
        ct state new ip saddr @allowed_ips counter accept
    }
}
```

The set need to have a type.

Now new elements can be added with eg

```
nft add element ip filter allowed_ips { 1.1.1.1 }
```

#### Concatenation

```
flush ruleset


table inet filter {

    # A set
    set allowed_ips {
        typeof ip saddr . tcp dport
        elements = { 172.27.97.2 . 22,
                     172.27.97.3 . 22,
                     172.27.97.2 . 8080,
                     172.27.97.3 . 8080 }
    }

    chain input {
        type filter hook input priority 0; policy accept;

        ct state new ip saddr . tcp dport @allowed_ips counter accept
    }
}
```

### Debug

**Note**: Use `counter` watch with `sudo watch -n 1 "nft -n list table inet filter"`

