# Linux Essential Tools

- cron
- systemctl

## Networking

- curl
- ping
- traceroute
- dig
- ss
- lsof
- ip
- tcpdump
- wireshark
- strace
- gdb
- bpftrace
- ebpf

## Disk Management

Nuke disk

```sh
dd conv=fdatasync bs=4M count=64 if=/dev/zero of=usb.img
```

Write to USB

```sh
dd conv=fdatasync status=progress bs=4M if=new.iso of=/dev/dfjhsdfsh
```
