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
---

## Social Engineering
Remember, do you? The weakest link in the chain of security is a human.  
Social engineering exploits trust rather than software vulnerabilities.
Cause you can use a password with entropy 80, stored in an encrypted password manager, but the moment you click a phishing link while half asleep after a 4 hour long session of gaming, you've **blown yourself**.

**Priciples SE thrives on**:
1. Fear
> I impersonate your boss at work & send you an intimidating email telling you to log into a 'new' company software with your normal creds.
2. Desparation
> You want a paid software for free. I get aquainted to you and give you an infected fake copy.
3. Pressure
4. Desire
5. There's a lot more...

---

## Impersonation

### Email Spoofing
Since `SMTP` is from the era of internet which was much more friendly, a lot of mail clients would trust **ANY** impersonator claiming to be... well, anybody. (sounds funny, doesn't it?)  
I mean seriously, if you sign up for even the free version of any email client, you can use an `SMTP server` & impersonate technically anytone, unless, of course, the actual sender's and the reciever's servers don't enfore e-mail secuirty measures.

**Email security Trio**
##### SPF: Sender Policy Framework
You, the owner of the legitimate mail server of that domain, `captainsum.org` for example, will publish a DNS record telling everyone:
```txt
The ONLY mail servers allowed       (Yup, it's just a TXT record)
to send mail for captainsrum.org        
are:

203.0.113.5
198.51.100.2
```
Now, when the reciever gets the email, it'll check the domain's DNS records and if there's a mismatch, the email will **fail** SPF.

##### DKIM: Domain Keys Identified Mail
The cryptographer of email services. Before sending an email:
- DKIM hashes the important parts
- Sings the hash with its *private key*
Upon receiving, the recipeint server then asks for the domain's public keys and verifies them against the DKIM signature.  
And if someone would have changed anything, the hash would have changed, and thus the DKIM verification will fail.

**Now this one would make you chuckle**
##### DMARC: Domain baed Message Authentication, Reporting & Conformance
Before this bad boy existed, shit was quite confusing.  
Cause all three of these security services can exist standalone. And if someone had set up SPF and DKIM, and not DMARC, then suppose either one fails, or even if both fail. What happens?  
**The email would be at ther mercy of the recipeint server**  
And each email server had (still has) different underlying rules. Some might drop the email, some might still deliever it, some might deliever it but put it in the spam folder.

With `DMRC` set up, the domain owner (the legitimate sender's domain, which just got impersonated), gets to *Prescribe* what to be done with the email.  
Do take a note that "prescribe" and "enforce" aren't the same, the decision is still largely the reciepent's. But most comply with the DMRC prescriptions.
