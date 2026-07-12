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
- HTTP request: GET
- URL as an instruction to the browser for how and where to retrieve a resource from.
- HTTP response headers.

### HTTP Methods:
- Explanation (used by a client to show intended action)
- **GET**: Getting info *from* a web server;
- **POST**: Submitting data to a web server or [potentially] creating new records;
- **PUT**:  Submitting data to a web server to update info;
- **DELETE**: Used for deleting info/records from a web server;

### HTTP Status Codes:
- When a web server responds, it always includes a status code as well.
- Status Code Ranges:

| Status Code Range | Category | Description |
|-------------------|----------|-------------|
| **100–199** | Information Response | Sent to tell the client the first part of their request has been accepted and they should continue sending the rest of their request. These codes are no longer very common. |
| **200–299** | Success | Used to tell the client their request was successful. |
| **300–399** | Redirection | Used to redirect the client's request to another resource, either a different webpage or a different website. |
| **400–499** | Client Errors | Used to inform the client that there was an error with their request. |
| **500–599** | Server Errors | Reserved for errors happening on the server side and usually indicate a major problem with the server handling the request. |

- Common HTTP Status Codes

| Status Code | Name | Description |
|-------------|------|-------------|
| **200** | OK | The request was completed successfully. |
| **201** | Created | A resource has been created (e.g., a new user or blog post). |
| **301** | Moved Permanently | Redirects the client's browser to a new webpage or tells search engines that the page has permanently moved. |
| **302** | Found | Similar to 301, but the redirect is temporary and may change again in the future. |
| **400** | Bad Request | The request was invalid or missing required information. |
| **401** | Not Authorized | Authentication is required before accessing this resource. |
| **403** | Forbidden | The client does not have permission to access this resource. |
| **404** | Page Not Found | The requested page or resource does not exist. |
| **405** | Method Not Allowed | The requested HTTP method is not allowed for this resource (e.g., sending a GET instead of a POST). |
| **500** | Internal Server Error | The server encountered an unexpected error while processing the request. |
| **503** | Service Unavailable | The server is temporarily unavailable due to overload or maintenance. |

### Headers:
- Additional bits of data sent to/from the webserver. NOT strictly required but actually it'd be quite hard to view a modern website without them.
- Common **request headers** sent from client to server:
    - Host: Tells the server which website to serve (told ya it'll be hard without these) cause modern website are more like multiple VMs hosted on the same server, and thus have the same IP.
    - User-Agent: Y'all people's browser. Sent so that the server can send data accordingly. (yk some web elements are only supported by select browsers)
    - Content-Length: When sending the data to a web server such as a form, it tells the web server how much data to expect, this way lost data cases can be identified.
    - Accept-Encoding: Tells the web server what type of compression/encoding the client uses. This is done so that the correct kind of compression can be used on the data for transmission.
    - Cookie: Data sent by the server to help it remeber you tf you are. So that you don't have to log in each time you hit <F5>.
> **Cookies**: So these are interesting, they're set inside your web-client when the server sends them via a 'set-cookie' header. And from then on, every request your browser makes, it sends the cookie along; so that the server knows which pirate is talking.  
> The *need* for cookies was that HTTP in itself is *stateless*, and as a result of that, can't remeber n' store your info persistenly. So cookies are sent on top of HTTP to make the connection kinda *stateful*.  
> Cookies are not authenticators themselves, instead, they're like containers that transport a state between your browser and the server. The state now can be a session ID or a login token or anything. The state decides the purpose, not the cookie.  
> Types of Cookies:  
> **Session Cookies**: These don't have an expiry date, they remain as long as the browser is open and you're browing, i.e- the *session* exist. When you close the browser? BOOM. GONE  
> **Question**: But then why do I not have to log in each time I open THM?
> Cause, Type-2: **Persistent Cookies**: The server sends them with either an expiry date or max-age header. They're stored in your browser's local storage, thus when you open and close  and open chromium again, you're still logged in.  
> **Third Party Cookies**:  Now, these are the ones that actually track you across websites. How? Well, suppose there are three different sites, all using google ads, your browser will send a `GET` request for the address for the `ad-banner.png`, google will respond with that AND a session-id cookie which your browser now stores, and any other site you visit which has google ads will get a `GET` request with the session-id cookie as well, so google will know what websites you browser AND IN WHAT ORDER (feels reason enough to install Graphene doesn't it?).  
> **Attack Vector**: **Session Hijacking**: The persistent feature of the session-id cookies make them conveninet, and thus -- at the same time -- make them very valuable as well. Cause anyone who manages to get your persistent cookie doesn't need any password or TFA OTP then, they can just spoof being you, cause the session-id is the only thing proving to the server that it's you. That is why cookies are sent **only** over HTTPS.
- Common **response headers**:
    - Set-Cookie: Information sent back to the client to store? **TODO**
    - Content-Type: Tells the client what type of data is being returned. Helps the browser know how to process it.
    - Content-Encoding: Again, what method of compression has been used to the data being transmitted so that the browser knows how to uncompress/decode it.

---

## How Websites Work
### How Websites Work
- Request-Response model
- Two ends front and back

### HTML
- Definiton as the building block of a website.
- Strucutre, head n' body | Basic Tags

### Javascript
- Javascript as a programmin language to make static sites into interactive web apps.
- Onclick functions to change innerHTML

### Sesitive Data Exposure
- Note on not to leave any sensitive data on the frontend 'cause anyone can view the source. Increases, attack surface.

### HTML Injection
- Just like SQL injection, but this time to HTML
- DO NOT TRUST USER INPUT

---

## Other Components
### Load Blanancers:
As the name suggests, they balance the load on the server, usually when you connect to a website with a load balancer, your request has to go through it, and when it gets the request it *smartly* forwards the request to one of many servers using alogrithms such as:
> Round Robin: Sends to each server in turn.  
> Weighted: Sends to the least busy server.

These guys are also responsible for performing periodic checks on each server (health checks) to make sure they're running.  
More than that, load balancers can also enforce security rules, like seeing that one certain IP has been sending 12000 SYN flags per minute, they'll drop those packets or for somene trying to Ddos you, they might have rules like only 100 connections/second/IP.

### CDN (Content Delivery Networks):
A CDN works like a traffic routing service. Suppose you setup a website with cloudflare as the CDN. After that you won't have your files hosted on just one server (though they're still 'hosted' on one actually). The CDN provided would have thousands of distribution servers across the globe, and whenever somone from Hokkaido tries to visit your website, the request would not have to travel all the way to Tallin, the CDN's local distribution server might be in Tokyo or even Hokkaido itself. It'll fetch the data from the source and will serve it in the response to the request, and then the stored data will live with a TTL. And when that TTL expires, it'll just fetch again. This way there's no need to update the codebase every time on 12000 servers, only the source server needs to update.

### Database:
- [Almost] every website requires a way of storing data. That's where databases come in. Webservers can communicate with them to fetch/update/manipulate data. There are many kinds like MySQL|MongoDB|Postres each serve a different purpose.

### WAF (Web Application Firewall):
- Defined as firewalls for the web server.
- Monitors traffic and controls the flow.
- Watches out for known attack methods, and drops traffic or blocks IPs w.r.t. the situaion.

### How Webservers Work
- Defined what a web server is, just a computer listening for incoming connections and then uses HTTP to serve the files to IP addresses requesting them.
- Listed common hosting servers like 'Apache', 'Ngnix' and 'IIS' and that commonly the files are stored in `/var/www/html` for linux based servers and in `C:\inetpub\wwwroot\` for windows based servers like `IIS`

**vHosts**
- When the same webserver hosts multiple websites in its local storage, and as per the client's request, serves a specific one.
- vHosts is just a text config file. (yup, lol)
- No limit to possible number of vHosts.

**Static V/S Dynamic Content**
- Static: Never Changes     | Changes made at the backend
- Dynamic : Changes         | reflect at the frontend

**Scripting & Backend Languages**
- Languages like Python, PHP, JS interact with the database, make changes, update fields on the backend which affects the frontend.  
- The user only sees the frontend 'cause the backend lies at the server.

---

## Inside A Computer System:

### Inside A Computer System:
- Puts down the idea that nearly every computer system is made up of the same building blocks.
- Components Explained:
    - Motherboard: Connects & holds almost all components
    - CPU: Brain of the computer, carries out instructions
    - RAM: Volatile fast memory
    - Storage: HDD--> Old/Cheaper per GB | SSD--> New/Expensive per GB
    - NIC: The computer's means of comm. with other computers. Variants:[wired/wireless/PCIe]
    - PSU: Takes and distributes power to all components
    - GPU: Picks up info from the OS and programs and outputs visual data to the monitor
    - I/O Devices: Used for sending n' receiving info to/from the computer

### What Happens When You Press The Power Button
- You press it
    - This turns the PSU from inactive to active state where it allow free flow of electricty to ever component as per the need. Which means everything has power already, they're just dumb 'cause they don't have no idea what to do with it.

- CPU steps in
    - The CPU is one component which is not dumb (it's literally the brain bro)
    - The CPU has "reset instructions" which tell it exactly what to do at startup: *Execute the code on the BIOS chip (the firmware)*

- The Firm-ware
    - The code that's now being executed from the BIOS/UEFI chip performs a 'POST' (Power On Self Test)
    - POST is done to check that all necessary components are plugged in and working, if not, it raises an alarm.
    - Once POST is done, then UEFI searches for a bootable storage medium. It can be anything that stores data as long as your PC supports booting from it.
    - This is when -- if you haven't set up auto boot -- you'll see the display with UEFI asking you to choose a boot medium.
    - After successfully finding a bootable medium, UEFI starts the bootloader.

- Bootloader
    - Bootloader is the software which controls and oversees the execution and running of your kernel up until the point where it hands things to it.
    - Once started, the bootloader then loads the OS kernel from the storage medium to the RAM.
    - The CPU then begins executing the kernel (and no, we're not servering the kernel's head from its body using a machette, it's CODE EXECUTION) and thus the kernel initalizes drivers, memory management n' everything and when it reaches the user space programs, the display appears, asking your fat fingers to log tf in.

---

## Computer Types
- Computer Types: Laptops, desktops, workstations, servers | Different machines => Different goals.
- Note on embedded computers & IoT devices:
    - Embedded: Often never connect to the internet and work for a lifetime locally on the host machine.
    - IoT: ('cmon man, there's fucking INTERNET in the name) connect to the internet to share data & receive commands.

---

## Client-Server Basics:
- Explanations in a connection:
- Request  -> Protocol -> DNS -> Response

### Web Communications In Practice:
- Launched a VM instance and inspected a locally hosted website with firefox dev tools and saw the request and response.

---

## Virtualisation Basics
- Virtualisation as a concept to reduce cost and increase efficiency.
- Hyprevisor as a VM manager.
- Container as a standalone box with enough config and resources for just the application it's meant to be shipped with. Runs on top of already existing kernel.

---

## Cloud Computing Fundamentals

### Cloud Computing Overview
- It's not like someone stole the cloud hardening spary from Doraemon and is now hoarding up server racks on them, In as nutshell, all cloud computing means is that you rent someone else's hardware over the internet to use for yourself (not to mention that the someone can be a $2.6 Billion MNC).
- You'd be surprised to know that even the big corp. like Spotify and Netflix use cloud servers.
**But Why? Don't they have enough money**
- Well, actually... yeah. They have money but they don't have "we need 12000 servers for a day so let's rent out a building and then buy some servers" kind of money. So they use cloud based servers like AWS (Amazon Web services), that way they can *instantly* increase/decrease how many servers you are using and thus pay only for what you use, no maintainence (physical) and no need for a 3 inch steel wall room to protect your proprietary servers, cause remeber the AWS is **HUGE** so basically you're enjoying scalable servers, paying as you go, and MNC level security. See the comfort?

---

## Operating Systems Basics: Introduction
### The Invisible Manager
An operating system is like an all-the-time running manager. It is what sits directly between your and your apps and the computer's hardware.

**System Privilege Levels**
Different componenents of the system operate at different levels.
- **Kernel Space**: Privilidged, and locked down core of the OS with unrestricted access to computing power and memory and the hardware.
- **User Space**: All standard apps live and run here, and so do you. Apps are delibratley restriced access to hardware directly as a security configuration of the OS. That's why when the apps need to interact with the hardware, the make a *'system call'* to the kernel, and then the kernel works on their behalf to execute that task.

The user **never** interacts with anything in the kernel space, and that's by design.   

Even when you run something as the root user, or with `sudo`, you are still in the userspace, because, even will all of its powers, root is still invariably a user.

**Driver**: Drivers are kernel space entities, they facilitate the execution of the tasks the kernel needs to be done as a response of system calls. The flow is something like this:
```txt
ping --(system call)-> Kernel --(Wifi driver)-> NIC
```

**OS Duties**
Yup, unlike you, the OS has duties which it fulfills quite fucking gracefully such as:

**Process Management**: A process is the running instance of a program. When you start a program, the kernel creates a process for it, and creating/terminating/handling the process then becomes a task which the kernel is responsible for. (Mind you the system calls are still made by the application that the process is created for)

**Memory Management**: The OS is also responsible for handling the computer's memory, and allocating multiple programs from the total pool of installed memory so that they can have access to it (through it).

e.g- Suppose you open firefox, initially it may ask for 24MB of memory, but that's not all, firefox works on **dynamic memory**, so it keeps asking the kernel for more and more memory as per the growing needs and as per the tasks you'd be performing with it.  
The other case would be of **pre-allocated memory**. In programs such as VirtualBox which need huge amounts of memory pre-allocated for its VMs, the program asks the kernel for all of that memory, and the kernel reserves it for it, and thus that much memory is not allocated to any other process which asks for more. (The hyprevisor's own functioning memory is still dynamic though.)

Another thing the kernel does is **abstraction**. Every program's memory is isolated from all other running programs at that moment. Only the kernel is the one with access to all of the different programs' memories.
> Kind of like a switch in a LAN, all the inter-device but intra-LAN connections gotta pass through it, but here, the kernel ain't allowing no one to take a peek.

**Filesystem Management**: The OS is also responsible for file handling. I.e- creating, updating, re-writing, setting up permissions for different files. All of that in addition to keeping the record of where the contents of each file are located on disk is one of the duties of the kernel.
  
**User Management**: The OS is also responsible for creating, authenticating and letting multiple users use the computer at once while keeping the credentials of each one inaccessible to all the others. Same goes for each user's processes and programs.

**Device Management**: And then we have this. Remember how we talked about drivers in the kernel space? That is a part of device management.  
The kernel manages executing the system call rquests because of the available drivers. If there's no drivers installed, system call ain't gonna achieve no shit.


**OS Security**:
Before any other add-on, the kernel implements security by itself (and bro is pretty good at it as well), such as:
1. Isolating processes
2. Authentication
3. Permission Management
4. Filesystem protection 
5. And much more...

### OS Interaction & Landscape
**OS Interfaces**
- Definition of GUI n' CLI, precision & control difference while working on either.
- Different OS types like desktop/server/mobile (while computers), each for a different reason: to achieve a specific task.

---

## Windows Basics:
- History of MS-DOS {<-- Kinda like a tty, you are in a black box and you type stuff, box goes brrrrrrrrr}
- Other windows' features like basic heirarchy of the file structure, root at `C:\`, different slases "backshlashes"

### Configurin' n' Securing:
- Windows firewall, task manager, security, updating/installing/uninstalling stuff.

---

## Linux(The GOAT) CLI basics:
- Basic Commands
- Navigation
- `find`, `uname`, `df` syntax

---


## Windows CLI Basics:
The command prompt, often called `cmd`, is a text based interface where in stead of clicking icons, you enter text based commands telling the computer exactly what you want to do. Quite like our linux TTY and emulators but in a different language.

**Basic Commands**:
`cd`: Navigation & shows you where you are
`dir`: List contents of the current directory
`dir \a`: Show hidden files like `ls -a`
`dir \s <file>`: Searches for the given file in all folders and subfolders from `.`
`type`: Same as unix `cat`
`whoami`: Similar, but here it also gives you the hostname
`hostname`: Gives you just the hostname
`systeminfo`: Kinda like `uname` but way too verbose
`ipconfig`: Quite like our legacy `ifconfig`

---

## OS Security:
**C**onfidentiality  
**I**ntegrity    
**A**vailability
- The need for an OS to be secure.

### Common examples of security:
3 weaknesses targeted by most attackers:
    - Auth and weak passwords
    - Weak file permission {principle of least privilege}
    - Malicious programs {trojans/ransomware/worms}

### Practical Example of OS security:
Attack machine **BABY**;
> Enumerated the target, found `ssh`, used it to log in (username and password was given), moved horizontally between users, half guided (was given the username and was told to figure out the password from "weak passwords", did, moved vertically (found root creds in `.bash_history`), got flag

---

## Data Representation:
### Representing Colours
**our first 8 colours**:
So, initially we only had 8 colours, not in nature jack, in computers. Because deriving from the base 3 colours {red|green|blue} and having them either in an **on** or **off** state {0|1}, that'll give us (2)<sup>3</sup>3 = 8 possible combinations, or in other words: *3 independent 2-bit channels:*
|R|G|B|Colour|
|---|---|---|---|
|1|1|1|White|
|1|1|0|Yellow|
|1|0|1|Magenta|
|0|1|1|Cyan|
|0|0|1|Blue|
|1|0|0|Red|
|0|1|0|Green|
|0|0|0|Black|

But 8 were not quite enough for the human spectrum, so, we thought: in stead of 3 independent 2 bit channel, what if we have 3 independent 8 bit channels. That ways:  
R <=> 1 Byte : 8 Bits => 2<sup>8</sup> = 256  
G <=> 1 Byte : 8 Bits => 2<sup>8</sup> = 256  
B <=> 1 Byte : 8 Bits => 2<sup>8</sup> = 256  

=> (256)<sup>3</sup> = 16,777,216

Now, **THAT** is a lot of colours. But the number didn't come out of nowhere, even with increased number of colours, it lets us represent each possible colour easily in *hexadecimal*.  
As each HEX character corresponds to a nibble, a total of 6 hex characters could now easily represent any 24-bit colour.
> NOTE: That should tell you a very important thing. *Representation is not data. Data is data*. Cause: (FF0000)<sub>16</sub> (255.0.0)<sub>10</sub> (100)<sub>2</sub> all represent the same **Red**.

### Numbers from Decimal to Hex
A note on different number systems with different bases like:  
Base 2 : Binary : 0|1  
Base 8 : Octal  : 0|1|2|3|4|5|6|7  
Base 10: Decimal: 0|1|2|3|4|5|6|7|8|9  
Base 16: Hexa-D : 0|1|2|3|4|5|6|7|8|9|A|B|C|D|E|F  

But why introduce characters in HEX?
> Cause without them, representation becomes ambigious.  
I.e- FFFFFF is white, cause character is distinct.
151515151515 is absolute garbage in that context cause who's gonna be determining whether that is 15151|5151515 or what other shit, so we solved the problem with assigning characters to double digit numbers.

---

## Data Encoding
### ASCII (7-bit)
- American Standard code for info interchange
- Used as a common medium of english interpretation on computers
- ASCII letters
- ISO/IEC for other western and European countries

### Unicode
- Unicdoe as the universal character encoding standard
- Assigns unique code points to each character, is surprisingly capable of not fucking up.
- Ease of use cause now you don't have to check what encoding you are writing and pray that the sender uses the same.

> Different UTFs simply are ways of storing unicode code points in bytes, UTF-8 goes intelligent and give anywhere from 1 to 4 bytes depending on character complexity. UTF-16 is a little less intelligent and uses only either 2 bytes or 4 bytes for each characater, more complex ones get 4, and the rest get 2. And then we have...  
UTF-32, bro says "fuck y'all" and given 4 bytes to everything.

---

## Python: A Simple Demo {Massacare}
- Building a random number generator and then a guessing game out of it to teach python basics. Syntax and other stuff.

--- 

## Javascript: A Simple Demo
- `const`: immutable | `let`: mutable
- `parseint()` for character to number conversion

---

## Database SQL Basics
- Column: Type of info | Row: One full record
- Definition of a query and basic queries

---

## The CIA triad
Cybersec focuses on protecting the three key aspects of the digital world:  
**C**onfidentiality  
**I**ntegrity    
**A**vailability

### Understanding the CIA triad:
**Confidentiality**:
- Sensitive data can only be accessed by authorized individuals.  

**Integrity**:
- Ensures that unauthorized individuals can't mutate data which can often happen in the transition phase, but conceptually can happen anywhere, (yeah, even yo grandma's backyard.)

**Availability**:
- Ensures data & services are available to authorized users when **needed**

### The Security Mindset
Interactive exercise to drag-n-drop boxes to classify into one out of *C.I.A*

---

## Cryptography Concepts
### Hiding Info - Symmetric Encryption
- Uses two keys(actually two copies of one key), one at the sender's position and one at the reciever's
- Thus both need a copy of the key
- Fast and efficient
- Key sharing problem

### Sharing Keys Safely - Asymmetric Encryption
- Works on public/private key model
- More secure <=> More expensive
- Solved key sharing problem

#### Legacy RSA-style Key Transport
Client generates a symmetric key, encrypts with the server's public key, sends data to the server, server decrypts with private key and stores the symmteric key.  
Both then use the symmetric key for bilateral data transmission.

#### Modern Witchcraft
Server first authenticates it really is `captainsrum.org`  
Server has a private secret.  
Client has a private secret.  
Both derive some info from their own sets of secrets and that derived data is transmitted publically towards the other.  
Using the other's publically shared data, both use that with their private secrets to generate a symmetric session key. How did that sound like? **Abracadabra!!**
