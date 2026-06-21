# Wireshark

**Purpose:** Observe network protocols as actual packets rather than theory.

## Setting Up

Start by installing Wireshark and configuring your user permissions:

```bash
sudo pacman -S wireshark-qt wireshark-cli
sudo usermod -aG wireshark $USER
newgrp wireshark
``` 

The last two commands add your user to the wireshark group so you don't need root privileges for the GUI.

## ARP Lab

### Testing ARP

Command used:

````bash
sudo arping -c 3 192.168.1.1
```

| Aspect | Details |
|---|---|
| Laptop MAC | 74:4c:a1:91:6f:b7 |
| Router MAC | e8:6e:44:36:e7:ee |
| ARP Request Direction | Laptop → Router |
| ARP Reply Direction | Router → Laptop |

**Key Finding:** ARP packets are observable in Wireshark as separate request and reply packets.

## DNS Lab

### Testing DNS Resolution

Command used:

```bash
dig archlinux.org
```

| Attribute | Value |
|---|---|
| Source IP | 192.168.1.27 |
| Destination IP | 192.168.1.1 |
| Protocol | UDP |
| Source Port | 53804 (ephemeral) |
| Destination Port | 53 (DNS) |
| Query | archlinux.org |
| Response (A Record) | 209.126.35.79 |
| TTL | 60 seconds |

**Key Finding:** DNS queries use UDP on port 53. The response packet reverses source and destination IPs/ports.

## Summary of Observations

- ✓ ARP requests and replies are visible as distinct packets in Wireshark
- ✓ DNS queries operate over UDP/53
- ✓ Response packets flip source/destination information from the query
- ✓ DNS responses include TTL values that determine cache duration
```
