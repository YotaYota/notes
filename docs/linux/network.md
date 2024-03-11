# Network

```
sudo ss -nltpu
```

```
nstat
```

## DNS

DNS - Domain Name System. Communicates on port 53.

On Ubuntu `/etc/resolv.conf` is a link to `/etc/systemd/resolved.conf`.

systemd-resolved.service

```
resolvectl status
```

DHCP is to dynamically get an IP. It communicates on ports 67 and 68.

If DHCP, then DNS server will be set automatically.

```
dhclient
```

## Network Interface

```
ip a
```

