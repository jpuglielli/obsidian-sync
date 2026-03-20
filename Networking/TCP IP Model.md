Model for how information is passed over the internet. Modern standard which won over OSI model. 
- 4 layers, each only talks to the layer directly below and above it
#### 1. Link Layer
Get a data frame from one machine on a network to another machine on the same network
- **MAC Addresses** are the identities at this layer. Communication between machines on the same local network is addressed using MAC addresses, not IPs.
- **ARP (Address Resolution Protocol)** bridges the Link and Internet layers. When the Internet layer says "send this to 10.0.0.5," ARP maps that IP to the MAC address of the machine on the local network. Results are cached in an ARP table.
- **Switches** operate at this layer. A switch maintains a lookup table mapping MAC addresses to physical ports, and forwards frames only to the correct port.
- **Ethernet Frames** are the unit of data at this layer. A frame wraps data from the layer above with a header containing: source MAC, destination MAC, and a type field indicating the protocol inside (usually IPv4 or IPv6).
**Commands:**
- `arp -a` — show the ARP table (IP → MAC mappings)
- `ip link show` — show network interfaces and their status
- `ip neighbor` — show ARP/neighbor table entries
#### 2. Internet Layer

#### 3. Transport Layer
##### 4. Application Layer
