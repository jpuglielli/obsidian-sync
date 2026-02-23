## IP Addresses
- An Internet Protocol (IP) address is a logical identifier assigned to each device connected to a network that uses the Internet Protocol for communication
- It serves two main functions: **host identification** and **location addressing**.
- Every interface that needs to communicate on Layer 3 must have an IP address assigned to it.
###### [[#IPv4 Subnets|IPv4]] example:
- 192.168.1.50, it is just a human readable format for a 32 bit binary number `11000000.10101000.00000001.00110010`
## Network interface
A network interface is a point of connection between a device and a network.
- Can be **physical** (network card, ethernet port) or **virtual** (software defined interface like a loopback, docker bridge, veth pair)
- **Common examples:**
	- `eth0` — a physical or primary Ethernet interface
	- `lo` (loopback, `127.0.0.1`) — a virtual interface for a device to talk to itself
	- `docker0` — the virtual bridge interface Docker creates for container networking
	- `veth` pairs — virtual interfaces that connect containers to bridge networks
## Network segment
Section of network where devices can communicate directly via the data link layer (layer 2)
- [[Physical_Datalink (L1-L2)#Ethernet|Ethernet]] & [[Physical_Datalink (L1-L2)#MAC Addresses|MAC addresses]], not IP routing
- Similar idea to [[#Subnets]] but at a lower level.
## Bridge Networks
Connects two [[#Network segment|network segments]] together so that devices on those segments can communicate as if they're on the same network.
##### Virtual bridges
- Do this in software
- When you see `docker0`, that's a Linux bridge that Docker containers plug into via their `veth` pairs.
- See the bridges on any Linux machine by running `ip link show type bridge` or `brctl show`
## CIDR
- Method for allocating IP addresses.
**It is 2 things working together:**
1. **A notation:** Uses the `/n` suffix that tells you how many bits are the network prefix. This replaces the rigid Class A/B/C system with the ability to draw the line anywhere.
2. **An allocation strategy**: Because you can draw the line anywhere, you can give an organization _exactly_ the right-sized block of addresses.
In the [[#IPv4 Subnets IPv4 example|IPv4 example]] above, if we set `/24`, the first 24 bits would be the network space, and the last 8 would be reserved for hosts. Thus we could have 2^8 different machines per network.
- `11000000.10101000.00000001 | 00110010`
## IP Networks
Essentially a way to determine "who is local", and is a way to subdivide (subnet) a larger network
- How can two machines know whether they can talk to each other directly?
- All machines on the same network can communicate directly (see [[Physical_Datalink (L1-L2)#Ethernet|Ethernet]]), otherwise, they have to communicate via a **router**
	- Think of communicating with your friends, if you are in the same room (IP network) you can communicate with all of them. But if you are across the country, you have to send messages via mail (**router**)
	- There is a Network Host split, every machine in the network will have the same network prefix (e.g. `192.168.1.___`), and what follows is what makes them unique within that network
## Subnets
- A subnet is a logical division of an IP network, defined by a **network prefix** and a **host range**, specified using CIDR notation
- The network side of the split, as well as ALL IP addresses within that network.
	- `192.168.1.0/24` defines a subnet
	- That subnet includes the shared prefix (`192.168.1`) **and** all 254 usable host addresses within it (`.1` through `.254`). 
- **Key relationship:** A smaller prefix length → more host bits → larger subnet. A larger prefix length → fewer host bits → smaller subnet.