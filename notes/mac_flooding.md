# MAC Flooding

> A switch uses a MAC table to store the mappings of individual MAC addresses to the physical ports in a network.

## Vulnerability

- When a switch's memory is fully occupied it turns into a hub (it treats received unicast frames as broadcast frames).
- An attacker can send multiple spoofed Ethernet frames to a switch, each containing different source MAC addresses.

## Implementation 

```bash
ifconfig <device> hw ether 5D:5D:5D:5D:5D:5D
```

```bash
macchanger --mac=5D:5D:5D:5D:5D:5D <device>
```

## Fixes
- **Port security:** limit the number of MAC addresses that can be learned from a specific port.