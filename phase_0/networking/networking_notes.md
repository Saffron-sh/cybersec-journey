# Computer System Network

## TCP
### Transmission Control Protocol
- Is like the inventory manager of the ship. IP might take you to barbados, but TCP is what makes sure that your 22 barrels of goods is sent there and 12 barrels of rum are recieved.
- How?
    - Imagine sending packets via TCP
    ```txt
    Packet #1 sent | #1 response noted (ACK)
    Packet #2 sent | #2 response noted (ACK)
    Packet #3 sent | #3 response noted (ACK)
    Packet #4 sent | #4: WAIT A DAMN MINUTE
    Where TF is the ACK for packet 4, RETRANSMIT IT000000
    ```
    - TCP will keep re-transmitting #4 till either it gets a ACK response for it, or it gets timed out or reaches the retransmission limit.
---
## UDP
### User Datagram Protocol 
##### The Rum induced cousin of TCP
- It also trasmits data... just a lil differently.
- It keeps transmitting data and if a packet is lost, it does what `pass` does in python.
- Now, it may sound unconventional, but it has it's own great application in live audio/video communication.
- UDP provides no guarantee of:
    - Delivery
    - Ordering
    - Retransmission
---
## On both TCP & UDP
### Uses
- TCP is preferred in applications where the ordered delivery is more important the the speed of the delivery.
- UDP has its use in applications where speed is of importance, a partially sent/recieved data doesn't make the economy crash here.
### Question:
- What impact does it have when TCP stops the process due to one missing packet. What actually happens?
> Correction first, no process is getting stopped. This is what happens:
```txt
Imagine TCP sends packets:
#17 | #18 | #19 | #20

And the internet does:
#17 lost
#18 arrived
#19 arrived
#20 arrived

The reciever now sees:
#18, #19,#20, and goes
"Oi, where TF is #17, you threw that bastard down to davy jhones locker?"

And thus the reciever does not hand an incomplete packet bunch to the application, because it made a BLOOD OATH to the application, an oath of "Ordered Delivery". TCP honours the oath and keeps the later packets in a buffer a sort of "waiting room" and then tells the sender:
"Oi #17 ain't on board yet mate"
The sender then re-transmits #17.
The whole pair gets completed and ONLY THEN does TCP hand over all 4 to the application. Keeping to the oath.
The application is the only thing that gets somewhat stalled because of it, but all other connections are intact and working, packets are being transmitted and recieved, just the ones from #17's pair that were recieved were held in the stall.

```

---
## IP
### Internet Protocol
#### The Ship's Navigator
- Takes the ship to the destination.
- See, each packet has a **SOURCE IP** and a **DESTINATION IP**. IP makes use of these and makes sure your packet reaches the destination.
- Let's imagine a packet's journey.
    - You type "https://archlinux.org" in your browser's search bar.
    - The browser creates a HTTP get request.
    - It gets wrapped into a TCP segement which stores the source and destination port.
    - That gets stored into a IP packet's payload whose header defines the source and destination IP addresses.
    - That gets wrapped into an Ethernet frame which stores the source and destination MAC addresses.
    ```txt
    It will look something like:
    Ethernet frame
    ├── Header
    │   ├── Destination MAC 11:22:33:44:55:66 (ROUTER)
    │   └── Source MAC      12:50:33:87:46:3e (YOU)
    └── Payload
        └── IP Packet
            ├── Header
            │   ├── Destination IP  209.126.35.79 (archlinux.org)
            │   └── Source IP       192.168.1.33 (YOU on the LAN)
            └── Payload
                └── TCP Segment
                    ├── Header
                    │   ├── Destination Port 443:HTTPS
                    │   └── Source Port      22313:ephemeral
                    └── Payload
                        └── HTTP Data
    ```
    - That is what gets transmitted from your laptop to your home router: THE ENTIRE ETHERNET FRAME.
    - The source port 22313 identifies the connection inside the TCP segment.
    - Here the router carries out **TWO** things.
    - **ONE**
    - It takes the frame, strips it down to the IP packet, looks at the Destination IP.
    - Takes down the IP packet, finds the TCP header, looks at the source port.
    - Recreates the packet with a new source port, and a new source IP (it's own public IP, and ephemeral port)
    - NAT comes in and takes a note of what INTERAL IP and ephemeral port the frame originally had.
    - It also takes a note of what the new SOURCE IP and the new ephemeral port is.
    - Enters them into the NAT tables as one entry like 192.168.1.33:22313 -> 45.25.65.142:55426
    - **TWO**
    - It looks at its routing table and sees multiple entries.
    ```txt
    Something like:
    25.16.0.0/16    -->     Router O
    209.126.0.0/16  -->     Router Q
    209.126.32.0/24 -->     Router X
    0.0.0.0/0       -->     ISP 
    ```
    - Routing tables are what routers use to "route" (forward) the packets from one node to the next.
    - In this case we have two candidates: Router Q | Router X
    - Where will our home router forward the packet to?
    - Router X, because it's subnet is much smaller and describes a much specified network.
    - REMEMBER: Routers use **Longest Prefix Matching** while deciding where to route a packet
    - Thus the router will forward the packet to X.
    - X will strip down the ethernet frame, see the destination IP, look at its routing table, and then will forward it.
    - X will recreate the ethernet frame with its own MAC address as the source MAC, this happens at each hop.
    - Each forwarding cycle is called a "HOP"
    - This happens at each hop till the packets reaches the destination (archlinux.org|209.126.32.79) <- in our case.
    - Then when arch linux responds, the whole process repeats, just with one distinction.
    - Source,Destination = Destination,Source at IP level
    ```txt
    NOTE: TTL (Time to Live)
    --> Defines for how many hops a packet would live before it is dropped by the network.
    --> Is measured in HOPs
    --> Suppose a packet has TTL=64
    --> At each hop: `TTL-=1`
    --> If it doesn't reach the destination IP before TTL becomes zero the packet gets dropped and we get a.
    --> ICMP Time exceeded
    ```
---
## ARP
### Address Resolution Protocol
- Read the name? well that's essentially what ARP does, it resolves your IP address to your MAC/hardware address on your local network: LAN, not just you, it does that for all devices on the LAN, and then remembers it (unlike you in the maths exam) in an ARP table.
---
## ICMP
### Internet Control Message Protocol
ICMP is a network diagnostic and control protocol.

- ICMP is the network's way of reporting status, errors and diagnostic information.

Ping uses ICMP Echo Request and Echo Reply messages, but ICMP also carries messages such as:
- Destination unreachable
- TTL expired
- Network errors
- Exists just to check whether the host is alive or chilling with davy jhones.
- It happens right after DNS thus sits at layer 3, right alongside IP.
- It just... exists and tells you "Yo, something happened" and that something can vary anywhere from host reachable to host offline.
- But fret not, cause host unreachable after an ICMP echo request doesn't always mean that the machine is offline, it can just be the firewall being picky.
---
## DHCP 
### Dynamic Host Configuration Protocol
- The guy who tells you where you live. (sounds funny doesn't it?)
- Well, imagine your brand new laptop, never been into your home LAN.
- You connect it to the wifi. **How does it get the IP Address??**
- Here is what happens:
    - Your laptop (No IP yet) sends a frame.
    - But to whom? and via what source IP?
    - The frame looks like:
```txt
Source IP:          0.0.0.0
Destination IP:     255.255.255.255

Source MAC:         Laptop MAC
Destination MAC:    FF:FF:FF:FF:FF:FF

Makes sense?
```

- Yup, you read that right. **Your Laptop sends a broadcast frame across the LAN**
- It is basically asking:
```txt
"Yo! Anyone know where I can get an IP here?"
```

- The **DHCP** in the router recieves that frame and **DORA** happens.
```txt
D:  Discover    <-- Your laptop just did this
O:  Offer
R:  Request
A:  Acknowledge
```
- The next step, **DHCP** offers you something like:
```txt
IP Address:
192.168.1.141

Subnet Mask:
255.255.255.0

Default Gateway Address:
192.168.1.1

DNS:
192.168.1.1
```
- Your machine requests that offered IP.
- The **DHCP** acknowledges that request & finally you get an IP.

### DHCP Lease
- But remember, you don't *OWN* that IP, it is merely yours on a lease. A DHCP lease.
- Just like TTL, when the lease expires, you either get to renew it, or you get assigned a new IP.
- DHCP decides, thus *Dynamic*.

#### Question:
- What if DHCP runs out of IP addresses to lease?
> Yup, that **CAN** happen. Suppose you configure your LAN like:
> Subnet: 192.168.1.0/24
> DNS Pool: 192.168.1.100 - 192.168.1.199
> Then the DHCP can only assign those 100 addresses dynamically, not all.
> And when DHCP runs out of those? Good luck gettin an IP.
> You can argue that "This subnet should have 254 usable addresses", and you'd be correct. It's just that all of them are not handed over to DHCP to lease dynamically.
> A lot of the IP addresses are reserved on the LAN for devices like your NAS, CCTVs, Router et cetra.
> And whatever is remaining after the reserved ones and DHCP pool are at the mercy of the admin to be allocated statically.
- So if you can set your IP to anything using linux `ip` or `ifconfig` why does DHCP exist at all?
> Yup, you sure can, if the DHCP's pool is from 100-199 and you assign yourself 192.168.1.43, ain't not shit gonna go down.
> The router will now see that a request came from 192.168.1.43, no biggie.
> The router only cares that the request comes from within in the LAN, not that it came from a device whose IP was assigned by the DHCP. Traverse the OSI map in your mind, when the ethernet frame goes from internal node to router, is there any kind of IP authentication? NOPE.
> So, you'll be fine as long as you assign yourself an IP out of the DHCP pool.
> Cause if you do that...
**IP CONFLICT**
- Remeber that the router's ARP cache (or any device's arp cache) DOES NOT have any kind of authentication?
- YUP, if you assign yourself an ip address of 192.168.1.150 in a LAN whose DHCP pool is 100-199 and later DHCP leases that address out to another node as well?
- Now every time your laptop tells the router "Yo am at 150" the router's ARP cache will go "oh, okay"
- And when Laptop B says the same thing, the ARP cache will again go "oh, okay"
- See the problem?
- Either you both will exprience a very weird internet connection and if you don't you'll start receiving half ass packets and maybe even packets the other node asked.
- SO? **DO NOT DO THAT** 
---
## Ports
Not talking about ships & pirates here, but actually; that's where the name comes from. 
Each machine is like a harbor, it has an IP address, say `192.168.43.24` and the harbor wants to dock two ships: *google.com & github.com* 
Now my friends, what does a harbor have? PORTS. Yup same case for computer network ports. Each machine has 65536 ports (yeah, it's 2<sup>16</sup>) which lets it have that many connections.
But why that number specifically? Because the port is stored in the TCP/UDP header as a 16 bit integer, and that can stores values going from 0->65535, which are **65536** total values. 

### Common Ports Used by different services by default:
| Category | Port Number | Service |
|----------|-------------|---------|
|   Web    |      80     |  HTTP   |
|   Web    |     443     |  HTTPS  |
|
| Remote Access|22|SSH|
| Remote Access|23|Telnet|
| Remote Access|3389|RDP|
|
|Networking|53|DNS|
|Networking|67/68|DHCP|
|
|Mail|25|SMTP|
|Mail|110|POP3|
|Mail|143|IMAP|
|
|File Sharing|21|FTP|
|File Sharing|445|SMB|
|
|Databases|3306|MySQL|
|Databases|5432|PostgreSQL|

### Question
**BUT** if HTTPS is used by port 443 by both google and github, how can two services use https at the same time, it's not like two ships can dock at one port together?
> Cause **YOU** are not using port 443, *google/github* is. Actually if you connect to both at the same time, it'll look like this:
```txt
192.168.43.24:55825 --> <google_ip>:443  | See that? The servers are using port 443. NOT YOU.
192.168.43.24:55826 --> <github_ip>:443  | What you're using are random ports, called EPHEMERAL PORTS
```
> Every connection is actually identifying using a **Source IP**, **Source Port**, **Destination IP**, & **Destination Port**

## Sockets
- Well, if you're thinking of the ports as... sea ports in a harbor, then sockets are the port's portmaster, equipment, and logbook. 
- Technically speaking, sockets are what controls what goes in/on/around/about a port. As an example if you do
```bash
python -m http.server 1212
```
- Python creates the process and:
```txt
Python: "Oi kernel, I'd like a TCP socket please"
Kernel: <creates a socket>
Python: "Bind this to port 1212"
Kernel: "Aye"
```
- Now the socket is created and is bound to port 1212, and is listening for any incoming connection.
- The moment you visit `http://localhost:1212" on firefox, a TCP SYN request is sent to the kernel for port 1212, the kernel sees it and checks whether there is a socket on that port or not. When it finds that the socket is opne and is there, it hand over the request to the socket and in turn the connection is established.
- Life is good.
##### BUT
- Now when you hit `CTRL+C`:
    - The python process dies
    - The socket it created dies (yeah the kernel created it technically, but it got that socket created)
    - The port 1212 still exists, but rn it is barren, ain't no one listening to shit there.
    - So now when firefox tries to go to "http://localhost:1212"
    - The kernel again checks if there is any socket open and listening on this port
    - Finds none; Sighs
    - Sends a TCP-RST (reset) response back to the browser saying "There ain't no one for you mate"
    - The browser sees this RST reponse and shows connection refused to you, cause YOU killed the process.
---
## NAT
### Network Address Translation
- Remember the ephemeral ports in the above section? This is what keeps record of them and resolves them to you internal IP.
- Essentially,  this crew member is our book keeper for whenever a connection is establised, like in the above example of `192.168.43.24:55826 --> <github_ip>:443`
- That IP is a local IP, the machine won't be talking talking to server of github directly, it'll have to go through the router, and thus go through as a public IP so the connection would actually look like.
```txt
192.168.43.24:6999      ] Internal Private IP:Ephemeral Port
       ↓
42.69.127.58:1212       ] External Public IP:Ephemeral Port
       ↓
   <google>:443         ] Destination. Google's servers
```
### Question
- What the fuck happens when two machines decide to randomly use the same ephemeral port, after all, two harbors can both have the same port number 5999
> Here this will better explain that (a picture is a thousand words, even an ASCII one lol)
```txt
                            {ROUTER}
192.168.43.24:5999 --> 42.69.58.125:1978 --> <google>:443
192.168.43.29:5999 --> 42.69.58.125:1979 --> <google>:443
                            {φ}
```
> See that? Internally they might be two machines, or ever 20, but on the public internet, they'll be one IP, the router's IP using different services on different ephemeral ports.
> Nat works with the router and stores this ip:port translation for internally forwarding the response at point {φ}. It works within the router.
