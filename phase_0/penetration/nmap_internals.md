# Network Mapper
- I ain't defining it cause of fucking course if you're reading my notes you know what nmap is, but if you genuinely do not:
> Nmap is a tool which scans device(s) and give you useful information about the device(s) like what ports are open/closed/filtered and what service they are running on what version and that what Operating System they're using.

### Some useful flags:
```bash

nmap -sT  # Scans the first 1000 ports on the device for TCP connections

nmap -sV  # Attempts service/version detection on discovered open ports

nmap -O   # Probes and finds out what OS is running on the device

nmap -sS  # Similar to -sT but does not complete the full 3 way handshake

# All the options will scan only the first 1000 ports by default, if you want
# to scan all 65536 ports you can use:

nmap -p- #with any option.

```

### Questions:
- How does nmap know what ports are open/close/filtered
> Cause it does with packets what you couldn't do with your resume: send it.
> If the host responds to the *SYN* with a *SYN-ACK*, nmap now knows that the port is open, it sends an *RST* 'cause we ain't here to talk, just scan, talking would consume time too, btw.
> So, if a host responds like that, that port **OPEN** baby.

> If the response for the initial *SYN* comes back as *RST* that's the machine telling you  "It's as dry as her replies to you here brother, nothing running on this port".
> If that is the response, the port is **CLOSED**.

> And the third scenario, if the host give you the same reply that she did after the first date: NONE. That means there might be a firewall or something else blocking the connection.
> Which means, that port **FILTERED** baby. Using a fucking sieve lol.

- When you run `-sV` how does nmap know what service is running & what version.
> When you runit with the `-sV` flag, nmap doesn't just send a *SYN*, it connect, completes the full handshake, and -- most services -- the moment you connect tell you what they are and what version they be running on.

> And even if a sysadmin acts smarts and hides the banner or modifies it to something else, nmap has a **HUGE** fingerprint database, (*cause your target might be smart, not all are, and when someone else -- long back -- ran the scan and the machine responded with its true self, nmap fingerprinted that, and even the devs and contributers of nmap contribute actively to expand that database*) so nmap basically sees that the service behaves like openSSH, responds like openSSH, and makes mistakes like openSSH. So even if the banner says ngnix.
> The service must be openSSH

- How come some flag needs superuser access while some do not?
> Honestly? Depends on what you're trying to do.

> No like really, if you want to access the normal TCP stack of the kernel, that's as exposed as your broke ass in the job market.
> Every user has access to it, 'cause they need to connect to the internet. No root needed for that.

> But if you step into the territory where you have to modify the raw packets, different from the normal packets and flags which the kernel normally would send, that is when you need explicit autharization for.
> That is reason why everything that either fabricates raw packets of modifies the flow of a normal TCP connection, needs root acces. E.g- nmap stealth scans, hping3, netdiscover et cetra.

> PS: But it's not like nmap takes over the kernel's network stack in all the connections. It works alongside as well.
> Take the stealth scan for example.  
> Nmap gets root privileges, crafts a *SYN* and sends it to the host.  
> The host acknowledges it and sends a *SYN-ACK* back, and... 
> Nmap says "Yeah, am out. Go talk to **HIM**"
> HIM being our linux kernel, sees that *SYN-ACK* like her dad looked at you for the first time "And... Who tf are you?"
> The kernel basically sees that it is not a connection that it opened (cause it didn't nmap did), and thus it is **foregin** to it.
> So it does what the TCP rules say to do to a foregin connection *SYN-ACK*.
> Bro sends an *RST* and is like "The door's behind you, have a nice day".
> Connection Reset.

- How does OS detection work with `-O`
> Quite like service detection. Principle remains the same here as well.
> Nmap uses probes and notices how the OS responds to mutilple kinds of requests and messages, cause every OS has a different rule set for network connections.
> After that, it again compares that with entries in its fingerprint database and then predicts what versionof what OS that machine be running on.

- How does nmap know that a target even exists before scanning the shit out of it?
> Now now now, we did a lil experiment cause we had a hypothesis that nmap actually uses *ARP* for host discovery before scanning.
> So, we opened two terminals and ran these two commands
> `tcpdump -i wlan0 arp`
> `nmap -sn 192.168.1.0/24`

> And YOO! the hypothesis was correct, we could see our sworn brother sending **ARP ping** requests for every IP on the subnet and then getting replies almost instantaneously.

> So, **Nmap uses ARP to deiscover hosts** before actually scanning, and the scan goes through only if the host is up and running.

- But what if the target is actively dropping discovery pings (ARP/ICMP)
> Nmap got you covered for this scenario as well. 
> If a sysadmin playing it smart and dropping those packets, nmap `-Pn` skips the whole "host discovery" path entirely.
> It treats the host as, and scans the living shit out of it.
> (The ports themselves are still discovered normally.)
> If the host is actually down or some is actually closed, we'll get that info anyways.
> Rare "low risk high reward" event.
