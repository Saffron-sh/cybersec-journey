# Attack Methods & Internals

## Wi-Fi

### Evil Twin
Now this guy an interesting one, you can use any machine (normally i'd suggest linux) and using a compatible wifi card, turn it into a whole router.  
- But why'd i do that?
> Think dumbass, normally you'd have to arpspoof someone and be the MITM to see their activity.
> But even before that, you have to break into the netwokr they're in.

> But what if do two birds with one stone? Who's the MITM between you and the internet by default? **The Router**
> So, once you become the 'router', people connect to you, and you just forward their request/responses to/from an actual router LOL.

> The router is also just a tiny computer running router software.
> Compared to that you got a fucking ballista. 
> Your laptop can absolutely run all the software a router can. You just need a compatible wifi card.

- But why don't you use your onboard NIC?
> Well... you can, but dont' come crying to me when neither works then.
> Morever, if your NIC is acting as the router, where'll you get the actual internet from? Stalingrad??

> An ideal setup would be your onboard NIC as the one actually connected to the internet and the wifi adpater acting as an AP and forwarding traffic. Nothing unsual, just hops+=2.

- Some tools you might need btw:
```txt
A DHCP >> hostapd                   | Or you can use whatever
A NAT  >> Routing                   | CLI/GUI tool you want
A firewall (optional) >> iptables   | I won't judge.
A DNS >> dnsmasq                    | (Or i might ;))
```

