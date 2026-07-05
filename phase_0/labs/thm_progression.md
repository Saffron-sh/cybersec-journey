# THM Pre Security

---

## Intro to Offensive Security

#### Concept(s) Learnt:
- Web page/path discovery

#### Tool(s) Used:
- Unix `dirb`

#### Command(s) Used:
```bash
sudo dirb <url> #concept
sudo dirb http://fakebank.thm #actual
```

#### Practical Skills
- Directory enumeration
- Basic web discovery

#### Key Takeaways
- Websites often expose hidden directories that are not linked anywhere.
- Directory brute-forcing is a form of content discovery, not exploitation.

---

## Intro to Defensive Security

#### Concept(s) Learnt:
- How SOC analysts work. Their methodology

#### Tool(s) Used:
- Simulated Traffic Analyser
- Simulated Firewall system.

#### Command(s) Used:
- *NONE*

#### Practical Skills:
- SIEM surface level understanding.

---

## Careers In Cybersec
- Read about security analyst's role (blue team):
    - Observing, Monitoring, and Investigating
- Read about role of security engineers in an organisation. They create and maintain the systems responsible for the security. E.g - IDS
- **Read about pentesting, learnt what an engagement is in the official sense (you don't just barge in, you have an engagement, sounds funny lol), and learnt about the progression from junior pentester -> pentester -> senior pentester -> Red Team Operator (Yeah, they're progressive not, parallel as I thought. lol)**

---

## Networks

### What is Networking:

#### Concept(s) Learnt:
- MAC spoofing can help bypass services which filter based on MAC.

#### Tool(s) Used:
- Simulated network graph with mac as `<form>`

#### Key Takeaways
- Networks, in the general sense, are simply 'things connected'!
- The internet is just many small private networks joined together.
- IP addresses are used in a network to talk & identify devices.
- MAC addresses are hardware assigned addresses used to identify and talk to devices in the 'intranet'

### Ping

#### Concepts Learnt:
- Ping uses `ICMP` PACKETS TO "determine the performance of connections between devices".

#### Tools Used:
- unix `ping`

#### Commands Used:
```bash
ping -c 4 10.10.10.10
```

---

## Intro To LAN:

### Introducing LAN Topologies

#### Concepts Learnt:
- Multiple types of topologies exist, each with their own pros and cons.
|Toplogy|Pros|Cons|
|---|---|---|
|Star|Scalable, Doesn't break if one node breaks|Expensive AF, Single point of failure at router/hub|
|Bus|Cost efficient, Easier to maintain|Prone to bottlenecks,Stem's the single point of failure|
|Ring|Cost efficient, Less prone to bottlnecks|Unidirectional flow of data resulting in slow speeds, You're fucked if the cable breaks|
- What a switch is and its role inside a network.
> They increase the number of devices that can be connected to a network.
> Reduce downtime by changing paths if one breaks.
- Learnt what a router is and what routing is.
> A router is a router because it routes data from one node to the next.

#### Practical Skills:
- Learnt how each topology can be sabotaged using their own cons in a gamified GUI.

### A primer on subnetting {Hell YEAH}
#### Key Takeaways
- A subnet is a smaller section of a network.
- Subnetting refers to dividing one network into smaller different subnets.
- Is done to inscrease security, efficiency, and establish full control.
- Network addresses are suffixed with `.0`
- Default gateways with either `.1` or `.254` (first or last usable address)

### ARP
#### Key Takeaways
- ARP lets devices associate MAC to IP address.
- ARP cache stores the association records of devices on the same network.
- ARP has to identifiers: Request|Response

### DHCP
#### Key Takeaways
- DHCP assigns IP addresses automatically to new devices in a network (unless they are configured with static ones)
- DORA

---

## OSI Model

### What's the OSI model?
- Full form (Open Systems Interconnection)
- Layers n' Encapsulation

### Layer-1 Physical
- Physical devices which transmit n' receive electric signals in binary.

### Layer-2 Data-Link
- Takes IP packets and wraps it in the ethernet frame.
- NIC has MAC *literally* burnt in.
- Its job is to present data in a fromat suitable for transmission.

### Layer-3 Network
- Routing happens here, oh and re-assembly as well.
- Everyone talks in IP.
- Protocols: OSPF & RIP
> RIP is a layer-3 dynamic routing protocol by which routers are able to share their routing tables with nearby routers so that they can learnt different routes.
> RIP historically filters by HOP lenght.

### Layer-4 Transport
- Responsible for data transfer.
- TCP, UDP, their usage, pros and cons of each and how they work.

### Layer-5 Session
- Responsible for creating and maintaining a session for data transfer.
- Sessions are unique. i.e- data can't travel over different sessions, only across each session.

### Layer-6 Presentation
- Acts as a translator to/from the application layer.
- Standardisation takes place here.
- For example, both G-mail and Proton mail are mail servers, at application level they are different, but inherently at presentation level they are similar.

### Layer-7 Application
- All protocol rules are in place here.
- This is what **YOU** interact with.
- Services include *DNS,HTTP(S)* et cetra.

---

## Packets and Frames:
### What are Packets & Frames
- Distinction:
```txt
Packet <0101010> IP
Frame  <1010101> Ethernet
```

### TCP/IP
- TCP/IP model (summarized OSI)
- TCP Packet internals (Header fields: Source/Destination port, checksum, flag, etc)
- TCP Flags (SYN,ACK,SYN/ACK,FIN,DATA,RST)
- Sequence and Acknowledgement numbers:
> Sequence Number: The byte number of the first data byte in the current TCP segment.
> Acknowledgement Number: The next byte number that the receiver expects to receive, indicating that all the previous bytes have successfully arrived.
- Played and interactive game putting TCP flags in order in a connection.

### UDP/IP:
- Stateless connection.
- No regard for confirmation on sent data. (Bro's just a chill guy)
- Fucking faster than TCP

### Ports:
- Ports as connections points in a machine.
- Common ports for different services.

---

## Extending Your Network
### Port Forwarding
- Making yourself visible on the public internet.
- Can be done at home with your router's GUI.

### Firewall
- Two types of firewall:
    - Stateful: Determines the behaviour of a device based off of its connection behaviour and then acts. (Resource Intensive)
    - Stateless: Follows a static set of rules and doesn't necessarily block a device/port based off a connection behaviour. Performs more packet level checks. (Resource Frugal).
- Played an interactive GUI game where implemented firewall rules to block oncoming malicious traffic.

### VPN Basics
- VPN as a connector forming a *virutal private network* between two otherwise logically disconnected devices.
- VPN technologies.
> (Old) PPP:   Point to Point protocol for connecting exactly 2 devices.
> (Old) PPTP:  VPN protocol that tunnels PPP packets over IP. Now *deprecated*
> (New) IPSec: A suite of protocols that secure IP packets themselves using encryption & authentication, commonly used for enterprise VPN.

### LAN Routing Devices
- Routers, and their purpose (ayo wait, again? HOW MANY TIMES DO WE GOTTA READ THIS)
- Factors affecting routing:
    - Shortest path between source and destination.
    - Medium of transportCopper/radio/fibre.
    - Most reliable path.
- Switches, and their use: *to connect multiple devices*
- Different types of switches:
    - Layer 2: Just send frames within and across the LAN.
    - Layer 3: Can create vLANs within the same network for segregation and control.
**Practical: Network Simulator**:
- Sent a packet from one LAN to other in a GUI, traced the handshake.

---

## DNS in Detail
### What is DNS?
- Explanation, and that it associates domain names with IP addresses.

### Domain Hierarchy
- Root-->TLD-->NameServer-->SecondLevelDomain-->Subdomain
- Creation restrictions:
> Must not be longer than 63 charcters in a combination of {a-z,0-9,-} {hyphen can't be initial or terminal and can't be repeated consequtively}
- Types of TLD:
    - ccTLD: Country codes
    - gTLD: General Purpose

### Record Types
|Record|Data|
|------|----|
|A|IPv4|
|AAAA|IPv6|
|CNAME|Redirections|
|MX|Mailserver|
|TXT|Misc text field|

### Making a request:
- Recursive DNS complete route traversal.
- From root server to top level domain server to authoritative name servers back to the dns.

### Practical
- Used `nslookup` to look up the different records from a temporary website.

---

## HTTP in detail:

### What is HTTP(S)
- Definition, use, and history of HTTP (Tim-Burners Lee)

### Request & Responses
#TODO

