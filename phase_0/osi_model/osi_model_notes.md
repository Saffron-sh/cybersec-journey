# Open System Interconnection referance model

## Layer 1: Physical Layer
- Cables, routers, adapters. This ain't really about protocols, this layer works on literally the physical level of the network. Problems here can be fixed via replacing the equipment and running loopback tests.
### Questions
- The fuck are loopback tests?
> Loopback tests are when you do this kind of shit `ping localhost` that if the device can talk to itself, it can probably talk to the world outside (because the loopback test only tells us that our network sturcutre is working fine, not that the world is not in the middle of World War-Z) and if not, well get a new cable dumbass
---
## Layer 2: Data Link Layer
- Is actually how devices on the same network communicate. It has DLC (not the steam games one) Data Link Controls. It uses the MAC address of the devices to establish a connection between them. 
### Questions
- (establish a connection between? then tf does layer 5 (session layer)  do?)
>  Layer 2 connection is still local, like "router <-> computer" and just that, while the layer 5 connection is more logical, and on a bigger scale like "Amazon server <-> My computer".
> Layer 2 check if "can we share data physically between us" while layer 5 is more like asking "are we still in the same convo" cause amazon got priorities man, it's a harbour for multiple ships, it doesn't wanna get confused whether it's talking to you or to bringus studios ordering the next batch of adapters.
- (The fuck are Data link controls?)
> Data link controls let devices on the same network communicate correctly. (yeah that's it for phase 0, don't you go rabbit hole-ing)
- (It's just that???)
> Mostly... yeah.
---
## Layer 3: Network Layer
- The 'routing' layer. Used by routers to determine the next hop of the data which is being sent.
- I remeber hops from the web series Mr. robot lol, first episode and the tor network exit node.
- It associates to IP (kinda like how layer 2 does to MAC?)
- It is also the layer  where we can cut down frames into fragments depending on what size the given network accepts.
### Questions
- Does layer 3 associate to IP how layer 2 does to MAC address
> YUP (didn't expect that did you) layer 2 is like a local neighbourhood, while layer 3 is like across manhattan addressing.
- And tf does it mean by cutting down to frames? TF ARE FRAMES?
> Frames are to packets what envelops are to letters, the post office delivers your letter in an envelope, a frame has the packet, Tcp segment, then layer two binds it with the source and the destination mac for `router <-> laptop` traversal and then it leaves the machine

> MAP
```
Imagine you request:

GET /index.html

Layer 7 creates:

HTTP Data

Layer 4 wraps it:

TCP Segment
└── HTTP Data

Layer 3 wraps that:

IP Packet
└── TCP Segment
    └── HTTP Data

Layer 2 wraps that:

Ethernet Frame
└── IP Packet
    └── TCP Segment
        └── HTTP Data

Layer 1 turns all that into:

010101010101010101

and fires it across the cable.
```
> When a frame is cut down, it's acutally the IP packet getting cut down like: data=[AAAAAAAAAAAAAAAAAAAAAAAAAAAAA] -> Three packets: [AAAAAAAAAAAAAAAAA] [AAAAAAAAAAA] [AAAAAAAAAA] and each packet gets its own frame then.
- Does the cutting down work like how i use this `aria2c -s 16 -x 16 <url>`
> Nope, aria2c does: download-> split into chunks-> download simultaneously. FOR SPEED
> IP fragmentation is: Packet too big -> network can't carry -> break into smaller packets FOR COMPATIBILITY
---
## Layer 4: Transport Layer
- Quite literally the "Post office" layer. 
- Yeah it just transports (literally) your data packets from one end of the net to the other (OHH so it is what lives off of those hops?)
- Uses TCP/UPD to transport 'IP Packets' (tf? i thought ip was like an address)
### Questions:
- Does layer 4 live off of the network hops and uses the exit node to exit the web and come to the machine where then layer 6 and 7 do shit.
> NOPE, layer 3 does that, layer 4 is tcp, it only cares that: 
    - I am talking to google
    - I sent packet #1 to google
    - I sent packet #2 to google
    - Did packet #2 response arrive?
> Whereas hopping is done by layer 3, like: Your pc-> Router A -> Router B -> Router C -> Google's servers
- TF are IP packets? wasn't ip like the address of a machine on the internet?
> YUP, ip is both a protocol and an addressing system, so if i say an IP packet, it means a packet following the rules of the IP protocol.
> That means the header (is it the header that has the info or the body??) has a `Source IP` `Destination IP` and `Actual data` in it.
- Here you go Scenario:{So if i am in a LAN, and have two laptops, i use either `python -m http.server` or `systemctl start httpd` to host a html or any othe file, and another device from the lan tries to access it, what transports shit? is it layer 4 cause transport is its cup of tea, or is it layer 2 cause dealing within a network, and between devices based on mac address is what it does????}
> ALL OF THEM, not just 2 and 4, each one, you host, and when someone fetches with a get request, layer 7:HTTP uses that GET request to get that webpage, layer 4 uses TCP to get that page across to you, layer 2 knows the destination mac address and layer 3 knows the destiantion IP address, layer 1 actually transers the data from the host to the one requesting.
> Interesting, ain't it?
---
## Layer 5: Session Layer
- Before two devices (or should i call them nodes?) can talk to each other, they have to form a session (kinda like you can't drive from moscow ot ulan-batar if there ain't no road). 
- Oh so this is why i don't have to log in on amazon every 3 minutes, and on some payment gateway pages, if i stay too long or keep the page open, it says "session expired" when i refresh ohhh.
- It controls "Control protocols" (tf are they?)
### Questions
- Tf are control protocols? are they what let the session layer establish/stop/resume a session? thus 'controlling the shit'
> YUP, they are what i just described lmao. They control `Start/stop/resume/end Session` shit.
- Tf is tunneling? and how's it important in this context, prof messer mentioned it.
> Broadly? putting one protocol inside another. 
> Think you put a boat in a bigger boat, and that in an even bigger boat (noah's ark for fuck's sake)
> The arc transports the smaller boats as it moves
> Used heavily by VPNs (Virtual Private Networks)
---
## Layer 6: Presentation Layer
- Shows us data in a form where humans can conveniently interpret tf is being shown to them.
- Controls the encryption and decryption of shit (SSL? TLS?)
- Works or is said combined with layer 7.
### Questions:
- If it controls encryption/decryption, isn't it technically the most important layer for both offensive security guys and defensive security guys, cause if there's a live goat hanging from a tree in the middle of the forest, it's a piece of interest for both the panthers and the other goats. (lmao tf kind of analogies am i giving)
> Nope this ain't the most important. As attackers, we attack EVERY-FUCKING-WHERE. 
> Layer 7: XSS,SQL injection | Layer 6: HSTS Hijack, breaking tls | Layer 4: port scan (at layer 4? i don't understand) | 
- Tf do they mean combined with layer 7?
> Cause modern networking combines a lot of stuff from layer 6 & 7 into applications, we'll learn more about this simplification in the TCP/IP model.
---
## Layer 7: Application Layer
- This is what we visually se on the screen.
- Every 'application' 'app' has this as the point of human contact.
- Uses HTTP, HTTPS, DNS (ayo hold up, DNS???)
### Questions
- Shouldn't the DNS be in layer 4 transport, or layer 3 network???
> Nope, bro is fine where it is, cause it answers questions like `what IP is google.com at` that, my captain, is a service and an application.
> Justifies its parley on the layer 7.
- Same for POP3, YOU LITERALLY ARE THE POST OFFICE PROTOCOL, WHAT BUSINESS DO YOU HAVE ANYWHERE BUT LAYER 4
> Cause layer 4 ain't like a trash can lablled "organic" that you throw anything even remotely organic, it is only associated with TRANSPORT PROTOCOLS. TCP/UDP. And protocols like POP3 and DNS use TCP/UDP respectively, they're not the layer 4 ship, they're passengers on it. They ride on top of transport protocols but work at layer 7. (kinda understood, will need more eventual explanations for a concrete understanding)
---

## MISC
### MAP of the ocean
```txt
Ethernet Frame
├── Source MAC
├── Destination MAC
└── IP Packet
     ├── Source IP
     ├── Destination IP
     └── TCP Segment
          ├── Source Port
          ├── Destination Port
          └── HTTP Data
```

### ARP
#### Address Resolution Protocol
- Read the name? well that's essentially what ARP does, it resolves your IP address to your MAC/hardware address on your local network: LAN, not just you, it does that for all devices on the LAN, and then remembers it (unlike you in the maths exam) in an ARP table.

### ICMP
#### Internet Control Message Protocol
ICMP is a network diagnostic and control protocol.

Ping uses ICMP Echo Request and Echo Reply messages, but ICMP also carries messages such as:
- Destination unreachable
- TTL expired
- Network errors
- Exists just to check whether the host is alive or chilling with davy jhones.
- It happens right after DNS thus sits at layer 3, right alongside IP.
- It just... exists and tells you "Yo, something happened" and that something can vary anywhere from host reachable to host offline.
- But fret not, cause host unreachable after an ICMP echo request doesn't always mean that the machine is offline, it can just be the firewall being picky.
