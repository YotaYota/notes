# Network

```
ip a
```

```
sudo ss -nltpu
```

```
nstat
```

## DNS

On Ubuntu `/etc/resolv.conf` is a link to `/etc/systemd/resolved.conf`.

systemd-resolved.service

```
resolvectl status
```

If DHCP, then DNS server will be set automatically.

