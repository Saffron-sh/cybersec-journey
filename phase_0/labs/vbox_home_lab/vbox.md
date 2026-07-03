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

#TODO
