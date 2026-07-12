# Computer Internals

## Startup
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

## Operating Systems Basics
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
