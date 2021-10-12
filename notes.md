# Lesson 1: The Internet

## Networks
Devices connected to communicate or exchange data

### LAN (Local Area Network)
Multiple computers/devices connected via a **switch** (or another network bridging device).
- WLAN (Wireless LAN) can be used to substitute cables.

### Inter-network
**Routers** are used to connect switches or LANs.
This is the gateway to the other LANs or computers via the internet

## Protocols
Systems of rules governing the exchange or transmission of data.
Allows different manufacturers, hardware, and software to depend on one another for consistency across the board.

## Two Models:

### OSI Model:
A model focused on the functions of each layer.
Layers:
**P**lease **D**o **N**ot  **T**hrow **S**ausage **P**izza **A**way
- Application
- Presentation
- Session
- Transport
- Network
- Data Link
- Physical

### Internet Protocol Suite (TCP/IP)
A model focused on the cope of communication (LAN, Inter-network, etc)
TCP/IP roughly maps to summarize layers of the OSI Model
- Appliation (Application, Presentation, Session, Transport)
- Tansport (Session, Transport)
- Internet (Network)
- Link (Data Link)

### Protocol Data Units: amount or block of data transferred over a network
Different names in different layers:
- Link Layer => *frame*
- Internet => *packet*
- Transport => *segment* (TCP) / *datagram* (UDP)
Usually consists of:
- Header
- Payload
- Trailer/Footer (sometimes)
Header and Trailer are Used to provide protocol-specific metadata about PDU
Payload is relative to the layer: The payload and header of one layer may be the total payload of the next.
This works like encapsulation or API's where the implementation isn't necessary, just the requirements for interaction between layers

## Physical Network
Cables, wires, waves, etc. concerned with transfer of bits and signals.

### Methods of Transmission
Electrical signals are used for Ethernet cables.
- Pros: Cheap
- Cons: Latency over distance and Interference prone
Light Signals: Used for long-distance transmission (i.e. fiber optic or laser)
- Pros: No Interference, instantaneous
- Cons: Expensive
Radio Waves: Wifi, Mobile, Satellite
- Pros: Wireless
- Cons: Short Radius

### Latency:
Latency is the *time* for data to get point-to-point.
Total Latency is the sum of the following parts:
- Propagation Delay: time for message to travel from sender to receiver. Calculated as the ratio of distance to speed.
- Transmission Delay: Time to navigate Links (intersections)
- Processing Delay: Time for data to get processed between links
- Queuing Delay: Router bottleneck problem.  Router will buffer data by putting it in the queue.
Other Latency Terms
- Last-mile latency: Delays in getting from ISP to your home network (often slower than core of network travel)
- Rount-Trip Time (RTT): Signal to be sent to point of acknolwdgement or response.

### Bandwidth: 
Bandwidth is the *amount* of data sent in a particular unit of time (second)
Bottlenecks: the bandwidth of a connection is limited to the lowest point in the overall connection.


## Link / Data Link Layer
Where the physical layer creates connections, the data link layer determins how they use the connection.
The primary elements of this layer are:
- Identification of the devices on the physical network
- Moving data over the physical network between devices Hosts (Computers/Servers), switches, and routers
Ethernet protocol (governed by IEEE) is most common and is comprised of **framing** and **addressing**

### Ethernet Frames
Frames are a PDU which encapsulates data from the Internet/Network layer above.
Ethernet Frames ad logical structure to the binary data at the Physical Layer by defining which bits are teh data payload and which are metdat used in the process of transporting the frame.
Sent prior to frame as synchroniaztion measure.
- Preamble: 7 Bytes (56 bits) notifying a device to expect frame data
- SFD (Start of Frame Delimiter): 1 byte (8 bits) identifies the start point of the data
Parts of the Frame:
- Mac Addresses: six bytes (48 bits) long each sent to notify the Destination (first) and Source (second)
- Length: two bytes (16 bits) which indicates the size of the payload
- Service Access Points: DSAP (Destination) and SSAP (Source), one byte each, identify Network Protocol used for the payload.
- Control: one byte informing communication mode for the frame
- Data Payload: Data from 42-1497 bytes containing the entire PDU from the layer above
- Frame Check Sequence (FCS): 4 bytes containing a checksum for the frame using cyclic redundancy check. FCS simply drops frames which do not match, it is up to higher levels to handle these situations.
Interframe Gap: Brief pause between frames; contributes to Transmission delay

### Mac Addresses
Assigned to every Network-Enabled Device wen manufactured (aka physical or burned-in address)
six two digit hexadecimal numbers (e.g. 00:40:96:9d:68:0a)
A switch routes MAC Addresses only to the specific intended device using a Port to MAC Table.
MAC Limitations at scale:
- Physical not logical
- Flat, not hierarchical

## Internet / Network Layer
Protocols which facilitate communication between hosts on different networks
Internet Protocol (IP): IPv4 & IPv6
- Routing via IP addressing
- Encapsulation of data into packets

### Packets
PDU in IP is a Packet comprised of a Header and a Data Payload (the PDU from Transport layer)
Important header elements:
- Version: IPv4 or IPv6
- ID, Flags, Fragmentation offset: Used for reassembly if Transport layer PDU was fragmented.
- TTL (Time to Live): Maximum number of hops before being dropped to avoid endlss re-routing. Decremented each time a router forwards.
- Protocol: TCP, UDP
- Checksum: Error Checking.  Also, IP doesn't help with lost packets. 
- Source Address: 32-bit IP address of source
- Destination address: 32-bit IP address of Destination

### IP Addresses
Logical addresses assigned as devices join a network.
IPv4 uses 32bit addresses with 4 sections.  
All routers have routing tables for the local network in order to get packets closer to its destination
IPv6 uses 128bit addresses

