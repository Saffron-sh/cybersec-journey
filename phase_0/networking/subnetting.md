# Subnets
### Yeah, quite literally "sub-nets"
- Imagine that your LAN is a city. There are multiple addresses like:
```txt
192.168.1.12 <-- YOU
192.168.1.18 <-- Friend
192.168.1.27 <-- Printer
10.15.2.16   <-- Neighbour
```
*How does your device know that you, your friend & your printer are together and that to share files with your neighbour it'll have to send the packets through the router?*

- Well, IP addresses actually are:
[Network Part] + [Host Part]
- Quite like:
[City Name] + [House Number]
```txt
e.g:
NYC, House #15      SAME CITY
NYC, House #179     DIFFERENT HOUSES
NYC, House #1526
```

- But stil, how does your laptop know `192.168.1.29` is nearby and `8.8.8.8` is somewhere on the internet.
- Because of THIS: 192.168.1.29**/24**

- The `/24` part tells us that the first 24 bits of the IP address belong to the network, the remaining go to the host.
> Yup, IP address is 4 bytes in the IP header.
> And since 1 byte = 8 bits => 4*8=32 bits.
> IP address is just  32 bits long.
```txt
See for yourself:
192 . 168 . 43 . 12
11000000 . 10101000 . 00101011 . 00001100
```
- So, for a `/24` subnet, the first 3 octets define what network you're in
- The last one tells you your house number.

**WHICH MEANS**
```txt
192.168.43.12  |
192.168.43.29  | Are in the same hood.
192.168.43.15  |
```
**BUT**
```txt
192.168.44.59 | This guy is from across the street
```

**AND**
```txt
10.19.29.57 | Is in another fucking city.
```
---
- So when your laptop sends a packet to `192.168.43.129` The laptop will see "Same hood"
- And thus it will find the device using an ARP broadcast. No need to bother the router.
- But when we gotta send something to `8.8.8.8` your laptop goes "HELL NAH, I ain't ARPing the whole internet.
- So it forwards the packet to the **Gateway**, which at home, is your router.

### What is a Subnet Mask
- Kay, so in `192.168.1.29/24` the `/24` is just *shorthand* (yup, shocked?)
- The older way of writing that would be:
```txt
IP: 192.168.43.24
Subnet Mask: 255.255.255.0
```
- But why **255**?
- Cause (255)<sub>10</sub> = (11111111)<sub>2</sub>
- And thus:
```txt
        255 . 255 . 255 . 0
=   11111111 . 11111111 . 11111111 . 00000000
```
- Number of **1**s = 8+8+8+0 = 24
- Thus `/24`
#### What does the **1**s mean?
- The **1**s are the bits which belong to the *NETWORK*
- The **0**s are the bits which are the *HOST*'s
```txt
11111111 . 11111111 . 11111111 . 00000000
Network  | Network  | Network  |   HOST

THUS:
192.168.1.12    | Are in the same network because
192.168.1.5     | they share the same network bits.
192.168.1.9     |
```

### Rule of Thumb

Same subnet?
→ ARP and communicate directly.

Different subnet?
→ Send packet to the default gateway (router).

Broadcast IP?
→ Everybody in the subnet receives it.

Network address?
→ Represents the subnet itself.

Broadcast MAC?
→ FF:FF:FF:FF:FF:FF

Host bits all 0?
→ Network address.

Host bits all 1?
→ Broadcast address.
---
### Special Addresses 
#### (Reserved Ones)
- **ONE**
- If the host bits are all **1**s.
- `192.168.43.255`
- This address is not just another IP which will be assigned to a node (it won't be).
- It is the **Broadcast IP**.
- Which means, anything sent here, will be sent to THE WHOLE SUBNET.
##### HOW?
- The ethernet frame of anything going to the *broadcast IP* will have the destination mac as FF:FF:FF:FF:FF:FF
- Which itself is the broadcast MAC. I.e- every NIC on the LAN will accept the frame.
> (ARP USES THIS BTW)

- **TWO**
- If the host bits are all **0**s.
- `192.168.43.0`
- This again is not some random addres which will be assigned to a node.
- It is the **Network IP**
- Kinda like the address of a colony.
- Anything sent here... Well, it'll be found in davy jhones locker. Cause think for yourself where will the letter go which is addressed to "The New York City"??
---
### Usable Addresses
- After considering that each subnet will have **ONE** network IP and **ONE** boradcast IP.
- For a `/24` subnet, the host has `32-24 = 8` bits.
- 2<sup>8</sup> = 256 Possible Addresses
- So Total - Reserved = Usable Addresses
- In the case of `/24` => 256 - 2 = 254 Usable addresses.

```txt
PS: DO FUCKING NOT "macchanger" your MAC to FF:FF:FF:FF:FF:FF cause the moment you do that
every NIC on the LAN will be accepting every packet you send. The ARP cache and tables would go crazy
and you'll probably have to reboot everything. So save yourself that choas (or not ;) )
```
---
### Exercises
#### The old school way (BINARY)

For `192.168.64.0/18` find:
- Subnet Mask
- Network Address
- Broadcast Address
- Number of usable hosts

**Aight, let's do this:**
- We convert the IP to binary:
```txt
192 . 168 . 64 . 0 
11000000 . 10101000 . 01000000 . 000000
    Network Bits        | Host Bits
```

##### For Subnet Mask:
Network Bits -> 1
Host BIts    -> 0
`11111111.11111111.11000000.00000000` => `255.255.192.0`

##### For Network Address:
Host Bits -> 0
`11000000.10101000.01000000.0000000` => `192.168.64.0`

##### For Broadcast Address:
Host Bits -> 1
`11000000.10101000.01111111.11111111` => `192.168.127.255`

##### For Usable Hosts:
=> 32 - 18 = 14
=> 2<sup>14</sup> = 16384
=> 16384 - 2 = 16382

#### The modern way (Network Blocks and interesting bits)
For `192.168.48.124/27` find:
- Broadcast Address
- Network Address
- First Valid Host
- Last Valid Host

**Jumping in again:**
- We again convert IP to binary
```txt
192 . 168 . 48 . 124
11000000 . 10101000 . 00110000 . 01111100
    Network Bits                   | Host bits
```
- Subnet Mask: 255.255.255.224 (using the binary method)
- Interesting bit is the number in the subnet mask that is NOT 255 or 0
- Here it is 224
- Block size is = 256 - Interesting Bit
- => 256 - 224 = 32
- Which tells us that *For a /27 subnet, addresses in the interesting octet changes by 32.*
```txt
Which means, these are the possible network addresses for this /27 subnet:
192.168.48.0
192.168.48.32
192.168.48.64
192.168.48.96  | Our IP lives in between here
192.168.48.128 | 
192.168.48.160
```
- Which means, our network address = **192.168.48.96**
- The broadcast address is one address less than the next network address
- So our broadcast address = **192.168.48.127**
- First Valid IP = 192.168.48.97
- Last Valid IP  = 192.168.48.126
