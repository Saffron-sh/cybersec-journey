# Computer System Network

## TCP
### Transmission Control Protocol
- #TODO
---
## IP
### Internet Protocol
- #TODO
---
## ARP
### Address Resolution Protocol
- Read the name? well that's essentially what ARP does, it resolves your IP address to your MAC/hardware address on your local network: LAN, not just you, it does that for all devices on the LAN, and then remembers it (unlike you in the maths exam) in an ARP table.
---
## ICMP
### Internet Control Message Protocol
ICMP is a network diagnostic and control protocol.

Ping uses ICMP Echo Request and Echo Reply messages, but ICMP also carries messages such as:
- Destination unreachable
- TTL expired
- Network errors
- Exists just to check whether the host is alive or chilling with davy jhones.
- It happens right after DNS thus sits at layer 3, right alongside IP.
- It just... exists and tells you "Yo, something happened" and that something can vary anywhere from host reachable to host offline.
- But fret not, cause host unreachable after an ICMP echo request doesn't always mean that the machine is offline, it can just be the firewall being picky.

## Ports
Not talking about ships & pirates here, but actually; that's where the name comes from. 
Each machine is like a harbor, it has an IP address, say `192.168.43.24` and the harbor wants to dock two ships: *google.com & github.com* 
Now my friends, what does a harbor have? PORTS. Yup same case for computer network ports. Each machine has 65536 ports (yeah, it's 2<sup>16</sup>) which lets it have that many connections.
But why that number specifically? Because the port is stored in the TCP/UDP header as a 16 bit integer, and that can stores values going from 0->65535, which are **65536** total values. 
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
