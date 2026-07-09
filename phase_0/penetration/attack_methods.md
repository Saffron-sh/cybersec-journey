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

## Web App Hacking
### SQL Injection
No Jack, the injection here is not a plastic tube with a piston and a needle which you'd start twerking if you saw it.  
It actually is *an application layer vulnerability* which allows unsanitized user input to be passed directly to the SQL parser as a query.

Let's take the example of a website which takes input from the users for their credentials and use SQL for their database management.  
They'd expect a normal user to enter
```txt
username = admin
passowrd = SuperSecret123
```
Now, an application that fails to treat user input as data will take the user input, and build that directly into a query like:
```sql
SELECT * FROM USERS WHERE NAME='admin' AND PASS='SuperSecret123';
```
Looks normal, right?  
Yup, cause this is the normal user.

**THIS** is what an attacker will try to do:
```txt
username = admin' OR 1=1;--
password = anything
```
Now, when **THAT** goes into the parser directly, it becomes:
```sql
SELECT * FROM USERS WHERE NAME='admin' OR 1=1;--' AND PASS='anything';
                                           ^
                                           ^
                                Condition always TRUE
```
Smell anything suspicious?  
**YUP**  
1. The attacker adds an apostrophe themselves to complete the username search
2. He/She added an OR opearator with a conditon that's **always true**.
3. Used the `mySQL` inline comment indicator "--" and commented out everything after the conditional.
4. The condition now always evaluates to true, potentially bypassing authentication or returning unintended results depending on the application. 
5. Now, **THAT** kids, is SQL injection.

> You'd take a note that the SQL parser wasn't the one at fault actually. In fact, it did exactly what it was supposed to do.  
> It was the *application* that fabricated the query that had the bug. {Layer 7}

### Underlying Principle

The bug isn't that SQL understands attackers.

The bug is that the application failed to distinguish between:

- Data
- SQL Syntax

Once user-controlled input becomes SQL syntax, the attacker can change the meaning of the query itself.

### XSS (Cross Site Scripting)
XSS is yet another application layer vulnerability which, because of an application that fails to treat user input as data, lets the attacker inject HTML/JavaScript that another user's browser interprets as part of the page.  

Assume there is a website which allows you to post comments, a normal user would enter something like:
```txt
Damn, apple cider is great.
```
And if the application treats user input as raw data, it will put the user input directly into the html, and then the text will become:
```html
<p> Damn, apple cider is great. </p>
```
No harm, right?
**NOPE**, just no harm *yet*.

What an attacker would do would be closer to:
```html
<script>malicious javascript code</script>
```
And now the browser will see the script tag and will execute the javascript that is inside those tags, why? Cause that is exactly what the browser is supposed to do.  
Again, it is the fault of the application to let unsanitized user input to be put directly into the page's `html`

> Once again, the browser wasn't wrong.
> The browser faithfully executed the HTML it received.
> The vulnerability exists because the application allowed user input to become part of the page's HTML.

### Session Hijacking
The persistent feature of the session-id cookies make them conveninet, and thus -- at the same time -- make them very valuable as well. Cause anyone who manages to get your persistent cookie doesn't need any password or TFA OTP then, they can just spoof being you, cause the session-id is the only thing proving to the server that it's you. That is why cookies are sent **only** over HTTPS.
