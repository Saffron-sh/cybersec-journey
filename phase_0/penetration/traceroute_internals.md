# Traceroute
**The friend who your parents told you to stay away from but you still are a bro with**
 
- Bro does what routers can't. 
- Traces the path your packets took across the internet to reach the target.

**How?**
> Remember TTL & ICMP? Yeah
> Yeah, with their help.

- If you send a packet with `TTL=1`, it'll reach your home router & will be dropped, but you'll get an *ICMP* response telling you that your packet died.
- Now what is worth knowing here?
- That the response will come with a SOURCE IP that'll be the router's IP.

- Then if we send a packet with `TTL=2`, the packet is dropped by the next router, and with its SOURCE IP, you'll get an *ICMP* response telling you that your packet was dropped.

- Your brother Traceroute, does that for every router your packets hop off of till they reach the target and then, quite literally, traces the route they took.
