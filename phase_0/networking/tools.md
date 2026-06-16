# Networking Tools
## Usage, explanation, and commands

### SS: Socket Statistics
- Lets you inspect sockets currently known to the kernel.
- (Loosley)Lets you see the statistics of every port on the machine. (potentially every, cause the closed ones aren't viewed by default)
- Commands are:
```bash
ss -t          # Display TCP sockets
ss -u          # Display UDP sockets
ss -n          # Show numerical addresses (no DNS resolution)
ss -l          # Display listening sockets only
ss -p          # Show process information associated with each socket
ss -a          # Display all sockets (listening and non-listening)
```
##### Common Combinations:
```bash
ss -tln        # TCP listening sockets with numerical addresses
ss -tlnp       # TCP listening sockets with process info (requires root)
ss -uan        # UDP sockets with numerical addresses
ss -tna        # All TCP sockets with numerical addresses
ss -pln        # All listening sockets with process info
```
