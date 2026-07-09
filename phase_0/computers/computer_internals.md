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
