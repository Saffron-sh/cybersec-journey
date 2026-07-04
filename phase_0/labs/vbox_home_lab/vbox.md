# Virtual Box {The Warden}
Aight, you dumbasses, today we cross from the harbour (theory) to the pirate ship (practical) class. 'Cause knowledge without a place for experiment is like owning a ship and then keeping it in the harbour.  
Every future topic that can be tested & experimented, *will be* tested and experimented, but we'll be using the virtualbox lab as our plunder simulator 'cause I'd rather not the kind's navy issue a bounty poster in my name.

Aight, enough fucking around, going technical:
**Virtualbox is a Type-2 Hypervisor**, and nah, am not speaking alien tech, a hypervisor is a kind of manager that sits on top of your host OS, and manages the virtual machine instances, allocates `vCPU`s, `RAM` and other resources. (This was for type-2)  
Type-1 is the kind of hypervisors that work without needing a host OS, talks directly to the hardware and is normally used in enterprise greade servers and/or cloud tech. 'Cause ain't no one launching steam on their cloud VPS.

## Installation
Am using arch, you'd be using something else, so look the command up for you, for my fellas, it is:
```bash
sudo pacman -S virtualbox
```
Now, you don't actually just launch it after that, you gotta: download, compile, and load vbox kernel modules if you wanna launch VMs. The reason being that you need the kernel modules for vbox to be able to talk to your kernel to let virualization happen.
```bash
sudo pacman -S virtualbox-host-dkms dkms #for arch
```
The DKMS stands for **Dynamic Kernel Module Support**, it detects when your kernel gets an update and automatically re-compiles the host modules for that specific kernel.  
> If you're not on arch though, you wouldn't really be worrying about constant kernel updates, not that am complaining though. ;)

And actually, if you're not on arch, you might have to compile the modules yourself with:
```bash
sudo dkms install vboxhost/<version> -k <kernel_version>
```
**BUT**, if you *are* on arch, the modules will be compiled at the time of installation, so you're golden.

Now, downloaded, compiled, but they're still sitting ducks in your filesystem, you still gotta load them into your kernel. Do that with:
```bash
sudo modprobe vboxdrv       # Drivers
sudo modprobe vboxnetflt    # Network Filter
sudo modprobe vboxnetadp    # Network Adapter
```
#TODO
Now, one thing to keep in mind, this *loading* is temporary, and these modules will be un-loaded the moment you shut your computer down, cause they were loaded into your current running kernel, and at boot, a fresh kernel will be there, and if you're like me, you can just load them each time manually for *control*.  
Or if you wanna automate it so that they're loaded to the kernel on boot, you can add them to your `/etc/modules-load.d` directory inside a file `vboxdrv.conf` and it'll tell systemd to load them at boot:
```bash
sudo echo "vboxdrv" >> /etc/modules-load.d/vboxdrv.conf
sudo echo "vboxnetflt" >> /etc/modules-load.d/vboxdrv.conf
sudo echo "vboxnetadp" >> /etc/modules-load.d/vboxdrv.conf
```

## Networks in VirutalBox
Now that you have loaded them modules you would like to configure your network so that the machines can talk to each other (or not, depends on you).  
Vbox has many network configurations, but fret not, as each one comes with its own uses.

### NAT Network:
Nope, not the normal `NAT` on your router but if you use this, then vbox creates its own virtual 'router' and then acts as the default gateway for every machine on the virutal `LAN`, this holds for every VM that is configured to use the same NAT network.  
All of the machines on the same NAT network can ping each other in the normal case, and will be on the same subnet (unless of course, you decide to configure the NAT network yourself). So, logically, it is quite like a normal `LAN`, but here it's a `vLAN`.
**Representation**
```txt
Internet <-> Home Router <-> Host Machine
                                ⇡
              |-    VirutalBox virutal Router
  NAT Network |                 ^
    vLAN      |                 |
              |            -----⇆-----
              |            |    |    |
              |-          VM1  VM2  VM3
```

### Host Only Adapter
Now, just as the name gives out, when you set any VM to host-only adapter, it can talk to the host and the host can talk to it (yup, the connection is mutual you hopeless romantic), and VirtualBox creates a network interface on the Host-OS as well, usually something like `vboxnet0`. So, it becomes another virtual LAN (with the Host OS joining in this time), vritualbox DHCP does assign both the Host and the Guest OS and IP, but there's no internet, cause there is no gateway connected.
**Representation**
```txt
Internet <--> Home Router <---
                   🡩         |
                   x         |
                   |         |
                   -----Host Machine
                             🡩
                             |
                     VM1🡨----⇆----🡪VM3
                             |
                             🡫
                            VM2
```

### Internal Network
Again, the name explains a lot, it's an internal network between all the machines set to use the same internal network. The virutalbox virtual DHCP assigns IP addresses, or you can do static ones yourself, creats a virtal LAN for them, but ain't no one talking to anyone outside the virutal software. Not even the host OS.
**Representation**
```txt
Internet <-> Home Router <-> Host Machine
                                 🡩
                                 x
                                 |
                              Vbox LAN
                                 🡫
                                 |
                         VM1🡨----⇆----🡪VM3
                                 |
                                 🡫
                                VM2                               
```

### Bridged Adapter:
This guy is the simplest, it quite literally acts as a bridge for the Virutal Machines and lets them connect directly to the actual router/network that the Host-OS is connected to. No `DHCP` no `LAN`, all the VMs exist in the same `AN`as the host OS, as if they were actual computers connected to the router.
**Representation**
```txt
Internet <--> Home Router      
               ⇅   ⇅   ⇅
              Host Machine
               🡩   🡩   🡩
               |   |   |
              Virtual-BOX
            • ↲    ↓    ↳•
            🡫      🡫     🡫
           VM1    VM2   VM3
```

### NAT Adapter
**NOPE**, this is different from the NAT network. Because there, it was a `vLAN` shared by every VM on the same NAT network. Contrary to that, here we every VM set to have a `NAT network` will have its own separate 'bubble' complete with a virtual router and all.  

You might be wondering that if that's the thing it does, why does it exist at all? Cause honestly, NAT network does the thing quite gracefully. And even if you only have one VM, NAT Network suffices.
> Well, tbh, you're not wrong, NAT Network is sufficient to mange even one VM as well, but it really comes down to simplicity of the setup required. The NAT network requires explicit setting up, naming, and configuring a DHCP manually if you wanna go that way, whereas the NAT adapter... just works, quite like plug and play, you don't need to set anything up. That's one reason as to why it is the default network configuration of every VM upon creation in VirutalBox.


### The Netwokr of OUR Home Lab:
Here, look at the diagaram: (remember lads, a picutre is a 1000 words, even an ASCII one)
```txt
Internet <--> Home Router <--> Host Computer
                                    ⮁
                                VirtualBox
                                    ⮁
                              (NAT Adapter)
                  (Internal)        ⮁
Metasploitable🡨---(  Net   )--🡪Kali Linux
```
Reason being: we don't want our intentionally vulnerable machine to be exposed to the internet.

---

## Snapshots {Savious}
Now, even before you launch your Virutal Machines, you better know about **snapshots**, these are like save points in the games. Once you take a snapshot of any VM in that specific state in the moment you took the snapshot, it is saved. And in cause you be plunderin' in future, and you end up breaking any of your VMs, you can restore to the state you took a snapshot at, an when you restore it, the current state of the machine will be overwritten, and you'll have the old, stable state. Kinda like a time machine, isn't it, LOL.
> You might want to take a look at the broken state before you have it overwritten to take a look at the situation from the point of view of the target that is being exploited. Not about sympathy (nothing wrong with it though) but for understanding and documentation.

---

## Useful Commands:
Having configured everything, here are some commands which will come in handy if you wanna play with your virtual machines from the command line, or even launch them in headless mode (no GUI):
```bash
# General
$ VBoxManage list vms # Show all installed VMs
$ VBoxManage list runningvms # Show running VMs

# Launching and Powering Off
$ VBoxManage startvm "<vm_name>" --type <headless|GUI> # Launches the VM in the mode you specified
$ VBoxManage controlvm "<vm_name>" poweroff # Stops the VM gracefully
$ VBoxManage showvminfo "<vm_name>" # Get information about a specific VM

# Snapshots
$ VBoxManage snapshot "<vm_name>" list # List all existing snapshots for that VM
$ VBoxManage snapshot "<vm_name>" take "<snapshot_name>" --description "<snapshot_description>" # Takes a snapshot of the VM in that state
$ VBoxManage snapshot "<vm_name>" restore  "<snapshot_name>" # Restores the VM to that specific snapshot
$ VBoxManage snapshot "<vm_name>" delete "<snapshot_name>"   # Delete that specific snapshot
```
