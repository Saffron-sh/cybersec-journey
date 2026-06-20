# DNS
## Domain Name System

- So you wanna reach `archlinux.org`?
- Well the internet works on numbers, not names. Who knows the IP?

Let's go on a journey of the request your browser sends to find the *IP*.
I'll explain things as we go.

### Journey
#### The LAN
- You enter `https://archlinux.org` in the firefox searchbar.
- You'll be surprised to know that the first place it'll look does not even need the internet.
- On your linux machine there's a file `/etc/hosts` it's like your computer's own DNS.
- Every linux distribution has one. Anything in it, **any** entry overrides all other external responses by default.
> Honestly, if Davy Jones comes out from the depths of the navy green & says `archlinux.org` is at `203.12.25.18` & `/etc/hosts` says it is at `127.0.0.1`, your laptop will look at Mr. Jones and say "Yeah, and am a Macintosh" and load up your localhost.
>That is why you don't expose `/etc/host` it to a user with SSH permission, cause the attack surface is **HUGE**.
- Now let's imagine your internal DNS isn't starting a vendetta with the Pirate God, where does the request go if the `/etc/hosts` file doesn't have an entry?
- The one place where everything from your machine's network goes: Your Router.
> Yeah, quite literally.
- When DHCP gives you a *lease* for the IP address it assigns to you, it actually gives you quite the starter kit.
```txt
IP Address:  192.168.1.43
Subnet Mask: 255.255.255.0
Gateway Address: 192.168.1.1
DNS Server:  192.168.1.1
```
- And if you look into your `/etc/resolv.conf`, you'll see something like:
```txt
nameserver 192.168.1.1
```
- That's your router acting as the DNS for anything on the LAN. 
- That is where your request goes next.
> NetworkManger generates the `resolv.conf` BTW...


- But your router doesn't magically know every IP on earth.
- Let's see where it forwards the request.
- Depending on how your router is configured, there would be a DNS it'll be set to use for getting IPs of domains.
- Most common ones are:
```txt
Google: 8.8.8.8
Cloudflare: 1.1.1.1
Quad9 (Encrypted DNS): 9.9.9.9
```
So the request then goes to the DNS server.

#### The DNS Server
- It is the heavy lifter who gets request from all around the world asking it to find IP addresses for multiple domains.
- Even after your router forwards the request here, it's not like your DNS server has a huge spreadsheet with every `Domain < - > IP`.
- There is a **HEIRARCHY**. (Yup, computers yk, no one's a democrat)
- The DNS asks someone, who? That's the next section.
- But what does a DNS server do then?

**Read this after completing the journey, or things might get confusing**

- What the DNS essentially does it store the responses from the *Authoritative Name Servers* of the domain whose IP you are asking it for.
- It does that so that the next time someone asks it where `archlinux.org` is, it doesn't have to go all the way getting referalls.
- In addition to the IPs, it also stores the **TTL** for each response.
- Yup, we're back at it.
- TTL here also stands for time to live, but in this context, every entry of `domain<->IP has its own TTL.
- The reason for that being IP addresses can change, they are not static, so if cloudflare resolves `archlinux.org` to `203.12.49.17` and the TTL is 3600. It'll keep that entry only for an hour, after which when someone acesses it again, cloudflare DNS will re-traverse the whole path.

#### The Root Server
- Root Servers are like the **Librarians Of the Internet**
- This is the place your DNS will ask the whereabouts of `archlinux.org`
- These are the guys who keep the information about the Top Level Domain servers
- So when the request comes to the Root Servers from you DNS, and the root server sees it is for `archlinux.org`. It'll basically say:
```txt
Oh, me? Nah I Don't know where that guy is, but i know someone who does.
Ask the TLD servers of ".org" they'll know.
(That's what's called a Referral, exactly what you're not getting for a new job)
```
- The request goes to the **TLD SERVER**

#### The Top Level Domain (TLD) Server
- These are servers which have the registeries of each TLD.
- There's a TLD each for `.com`, `.org`, `.in`, `.ie` et cetra.
- In our example, the request for `archlinux.org` will now come to the TLD servers for `.org`.
- And what does the TLD server says?
```txt
Well, you might not have expected this... but actually even i don't know
where your guy is, oh but i know his landlord (Authoritative Name Server)
They'll know where you can find him.
```
- So TLD servers again "Referred" (get a job bro) the request to the authoritative name serves who maintain the records for `archlinux.org`

#### The Authoritative Name Servers/ DNS servers
- Finally, after so long.
- These are the servers which keep keeps records of the domains that are online.
- They are stored like:
```txt
A   archlinux.org   209.126.35.79
MX  archlinux.org   mail.archlinux.org
TAT archlinux.org   "Welcome Tinkerers"
```
- This guy won't refer, cause he knows where `archlinux.org` actually lives.
- Reason? Cause bro got the *Authority*
---
### Response:
- So, after all, the response of the request goes back from the Authoritative NS to the DNS.
- One thing to keep in mind is that the response does not go the whole way to TLD servers and Root servers, cause they only referred, they didn't transmit the request.
- So the response goes from the Authoritative NS to the DNS and comes back to you and loads.

- So the journey is like:
```txt
You (192.168.1.29) "https://archlinux.org"
 ↓
Router (192.168.1.1/<Router Public IP>)
 ↓
DNS (9.9.9.9 | 1.1.1.1 | 8.8.8.8)
 ↓
Root Server "Idk, ask TLD .org"
 ↓
TLD .org "Idk, ask authoritative NS"
 ↓
Authoritative NS "Yup, here: 209.126.35.79"
 ↓
DNS (9.9.9.9 | 1.1.1.1 | 8.8.8.8)
 ↓
Router (192.168.1.1/<Router Public IP>)
 ↓
You (192.168.1.29) "https://archlinux.org"
```

### Buying A Domain
- See, whenver you buy a domain, let's say:
```txt
captainpleaseleavesomerumfortherestofustoo.org
```
**This is all that happens:**
- The ICANN (Internet Corporation for Assigned Names & Numbers) 
- It has registeries. Yours in this case will be the `.org` registery.
- You buy the domain from a registry with the help of a registrar.
> Quite like how you would buy stocks with the help of a stock broker.
- You don't talk to the main instituion yourself.
- After you do that, the registrar says:
```txt
Cool, so what nameservers you want for your domain?
```
- And you'd tell it you want:
```txt
barcadi.captainpleaseleavesomerumfortherestofustoo.org
oldmonk.captainpleaseleavesomerumfortherestofustoo.org
```

- The registerar then updates the .org registery.
- So now the TLD server for `.org` knows to forward any request for `captainpleaseleavesomerumfortherestofustoo.org` to `barcadi.captainpleaseleavesomerumfortherestofustoo.org` that comes asking for your IP.
