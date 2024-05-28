# Wireguard

WireGuard securely encapsulates IP packets over UDP.

You add a WireGuard interface, configure it with your private key and your peers' public keys, and then you send packets across it.

All issues of key distribution and pushed configurations are out of scope of WireGuard.

WireGuard works by adding a network interface. This network interface can then be configured normally using ifconfig(8) or ip-address(8), with routes for it added and removed using route(8) or ip-route(8), and so on with all the ordinary networking utilities. The specific WireGuard aspects of the interface are configured using the wg(8) tool. This interface acts as a tunnel interface.

## Cryptokey Routing Table

Simple association of peer public key and allowed IPs.

A **Peer** is defined by its public key. If the packet, when decrypted, is also from an allowed ip, it will be allowed onto the interface.

Outgoing traffic destination address will be matched on **AllowedIPs**, and in that case be put on the WireGuard interface.

## Configuration

### Generate Keys

```
wg genkey | tee privatekey | wg pubkey > publickey
```

### Example

```
# /etc/wireguard/wg0.conf

[Interface]
PrivateKey = AAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAEE=
Address = 10.0.0.1/32
ListenPort = 51821

[Peer]
PublicKey = fE/wdxzl0klVp/IR8UcaoGUMjqaWi3jAd7KzHKFS6Ds=
AllowedIPs = 192.168.200.0/24
Endpoint = 203.0.113.2:51822
```

- Interface
    - `PrivateKey`: your private key used to decrypt messages
    - `Address`: virtual address of the local WireGuard peer, it’s the IP address of the virtual network interface that WireGuard sets up for the peer
    - `ListenPort`: Port WireGuard will listen for incoming UDP traffic
- Peer
    - `PublicKey`: public key of peer used to encrypt messages
    - `AllowedIPs`: whitelist of IPs from messages with PublicKey
    - `Endpoint`: When traffic is routed to a virtual WireGuard endpoint, endpoint tells which real IP to send the traffic to

Using this example, after running `wg-quick up wg0`, the host will have an network interface with IP 10.0.0.1.


`wg showconf wg0` will show the current **ListenPort**. Running `ss -ulpn | grep <ListenPort>` will show that process. Note that lsof will not show anything since WireGuard is running in the kernel.

## Flow

Outgoing

1. When the host sends a request to an ip inside **AllowedIPs**, eg 192.168.200.22, the OS will check its routing table to see which local network interface to bind the new TCP socket to - in this case *wg0*.
2. *wg0* is selected as interface, and it will use the source IP of 10.0.0.1 (**Address** field).
3. A packet is created that is handled by the *wg0* interface that checks its lists of **AllowedIPs** to see if a **Peer** is associated with the packet's destination address (in this case 192.168.200.22 is).
4. WireGuard encrypts the message with the **PublicKey** of the peer, and wraps it into an UDP packet and sends it to the **Endpoint** ip of the **Peer**.
5. WireGuard will consult the routing table for which local network interface to use for the new UDP packet created by WireGuard. The source ip will be that of the interface used (eg *wlan0*'s), and the source port will be **ListenPort** from configuration.

Incoming

1. WireGuard will be listening for UDP on **ListenPort** (51821)
2. WireGuard will decrypt it using **PrivateKey** and place any decrypted packets on the network stack as if they had come directly from *wg0* interface.

## Useful Commands

- `sudo wg`
- `wg showconf`
- `wg show`
- `wg-quick up <interface name>`

