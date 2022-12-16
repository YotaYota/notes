### Screen control

```
arandr
xrandr
```

### Sound control

```
pavucontrol
pulseaudio
```

### WiFi control from terminal

```
nmtui
```

### Prevent screen from sleeping

```
xset dpms 0 0 0
```

### Set java (javac) version

```
sudo update-alternatives --display java
sudo update-alternatives --config java
```

### Change TTL

```
sudo sysctl -w net.ipv4.ip_default_ttl=128
```
to change temporarily.
Edit
```
/etc/sysctl.conf
```
to permanently change.

### Update firmware

```
sudo apt update && sudo apt upgrade -y
fwupdmgr get-updates
sudo fwupdmgr refresh
sudo fwupdmgr update
```

### Network debugging

```
sudo lshw -c network
```

```
dmesg | grep ath10k
```

```
lspci -nnk | grep 0280 -A3
```
