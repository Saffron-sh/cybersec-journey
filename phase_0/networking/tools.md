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

### DIG
- Quite literally lets you DIG into Authoritative Name Serves of a domain
- Used to query DNS records and retrieve detailed information about domain names, mail servers, name servers, and other DNS data.
- Shows comprehensive query information including flags, query time, server responses, and answer sections.
- Commands are:
```bash
dig example.com                    # Query the A record for a domain
dig example.com +short             # Show only the answer section (simplified)
dig example.com MX                 # Query mail server records
dig example.com NS                 # Query name server records
dig example.com CNAME              # Query alias records
dig example.com SOA                # Query start of authority records
dig @8.8.8.8 example.com           # Query using a specific DNS server
dig +trace example.com             # Show the full DNS resolution path
dig -x 192.168.1.1                 # Perform reverse DNS lookup
dig example.com ANY                # Query all record types for a domain
```
##### Common Combinations:
```bash
dig example.com +short MX          # Get mail servers in short format
dig example.com +nocmd +nocomments +noheader +nostats  # Minimal output
dig @ns1.example.com example.com   # Query a specific authoritative nameserver
dig +trace +short example.com      # Show trace path in short, readable format
dig example.com +noall +answer     # Display only the answer section
dig example.com +noall +authority  # Display only the authority section
```
