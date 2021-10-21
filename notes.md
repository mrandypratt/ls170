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
Protocols are systems of rules governing the exchange or transmission of data.
They allow different manufacturers, hardware, and software to depend on one another for consistency across the board.

## Two Models:

### OSI Model:
A model focused on the functions of each layer.

Layers: **P**lease **D**o **N**ot  **T**hrow **S**ausage **P**izza **A**way
- Application
- Presentation
- Session
- Transport
- Network
- Data Link
- Physical

### Internet Protocol Suite (TCP/IP) Model
A model focused on the scope of communication (LAN, Inter-network, etc).

#### Comparison to OSI Model:
TCP/IP roughly maps to summarize layers of the OSI Model
- Application (Application, Presentation, Session, Transport)
- Tansport (Session, Transport)
- Internet (Network)
- Link (Data Link)

## Layer Interactions
Protocol Data Units: amount or block of data transferred over a network

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
#### Electrical signals are used for Ethernet cables.
- Pros: Cheap
- Cons: Latency over distance and Interference prone

#### Light Signals: Used for long-distance transmission (i.e. fiber optic or laser)
- Pros: No Interference, instantaneous
- Cons: Expensive

#### Radio Waves: Wifi, Mobile, Satellite
- Pros: Wireless
- Cons: Short Radius

### Latency:
Latency is the *time* for data to get point-to-point.
Total Latency is the sum of the following parts:
- Propagation Delay: time for message to travel from sender to receiver. Calculated as the ratio of distance to speed.
- Transmission Delay: Time to navigate Links (intersections)
- Processing Delay: Time for data to get processed between links
- Queuing Delay: Router bottleneck problem.  Router will buffer data by putting it in the queue.
#### Other Latency Terms
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
Frames are a PDU which encapsulates data from the Internet/Network layer above which add logical structure to the binary data at the Physical Layer by defining which bits are the data payload and which are metdata used in the process of transporting the frame.

#### Sent prior to frame as synchroniaztion measure.
- Preamble: 7 Bytes (56 bits) notifying a device to expect frame data
- SFD (Start of Frame Delimiter): 1 byte (8 bits) identifies the start point of the data

#### Parts of the Frame:
- Mac Addresses: six bytes (48 bits) long each sent to notify the Destination (first) and Source (second)
- Length: two bytes (16 bits) which indicates the size of the payload
- Service Access Points: DSAP (Destination) and SSAP (Source), one byte each, identify Network Protocol used for the payload.
- Control: one byte informing communication mode for the frame
- Data Payload: Data from 42-1497 bytes containing the entire PDU from the layer above
- Frame Check Sequence (FCS): 4 bytes containing a checksum for the frame using cyclic redundancy check. FCS simply drops frames which do not match, it is up to higher levels to handle these situations.
- Interframe Gap: Brief pause between frames; contributes to Transmission delay

### Mac Addresses
Assigned to every Network-Enabled Device when manufactured (aka physical or burned-in address)
six two digit hexadecimal numbers (e.g. 00:40:96:9d:68:0a). 
A switch routes MAC Addresses only to the specific intended device using a Port to MAC Table.

#### MAC Limitations at scale:
- Physical not logical
- Flat, not hierarchical

## Internet / Network Layer
Protocols which facilitate communication between hosts on different networks.

#### Internet Protocol (IP): IPv4 & IPv6
- Routing via IP addressing
- Encapsulation of data into packets

### Packets
PDU in IP is a Packet comprised of a Header and a Data Payload (the PDU from Transport layer)

#### Important header elements:
- Version: IPv4 or IPv6
- ID, Flags, Fragmentation offset: Used for reassembly if Transport layer PDU was fragmented.
- TTL (Time to Live): Maximum number of hops before being dropped to avoid endlss re-routing. Decremented each time a router forwards.
- Protocol: TCP, UDP
- Checksum: Error Checking.  Also, IP doesn't help with lost packets. 
- Source Address: 32-bit IP address of source
- Destination address: 32-bit IP address of Destination

### IP Addresses
Logical addresses assigned as devices join a network.
All routers have routing tables for the local network in order to get packets closer to its destination

#### IP Versions
IPv4 uses 32bit addresses with 4 sections.  
IPv6 uses 128bit addresses

# Lesson 2: The Transport Layer

IP layer lacks:
- Direct Connection between application
- Reliable network communication

## Multiplexing & Demultiplexing
Different applications/processes are distinct channels on a host machine
IP provides a single channel between hosts which have multiple channels whcih need to communicate.
Multiplexing is transmitting multiple signals over a single channel
Demultiplexing is the reverse

## Ports
Network Ports is an identifier for a process running on a host. It is an integer 0-65535
- 0-1023: used for common network services (HTTP = 80, FTP = 20&21, SMTP 25)
- 1024-49151: Registered ports of private entites (Microsoft, IBM, etc).  Can be Ephemeral ports on client side.
- 49152-65535: Dynamic ports: can be used for customized services or allocation as aphemeral ports

Transport layer PDU contains the source and destination port numbers.
The combination of IP address and port number creates end-to-end communciation between applications on different machines, called a communication end-point, generally referred to as a socket.
Socket: combination of IP Address and Port Number (e.g. 216.3.128.12:8080)
`netstat` is a utiity in the terminal which returns network connections (aka sockets)

## Sockets
- UNIX Socket: communication b/t local process on same machine
- Internet Sockets: inter-process communication between networked processes

**Connections** are created when a specific socket is only listening to one host
Connection-less: one socket object which listens to all hosts

Connectionless system:
- `listen()` method awaits incoming messages
- process and respond to messages as they arrive

Connection-oriented system:
- `listen()` method awaits incoming messges
- Arrving message is received and a new socket object is immediately created
- The socket object tracks the IP and Port of the process and host source and destination (the four-tuple)
- New messages which match an existing four-tuple get routed accordingly
- New Messages which do not match an existing four-tuple get instantiated.

## TCP (Transmission Control Protocol)
Since Ethernet and IP checksums do not handle lost/mixed-up data, they are unreliable communication channels
TCP provides Reliability on top of these unreliable channels: Data integrity, de-duplication, in-order delivery, and retransmission of lost data.
TCP impacts performance due to the requirements imposed by reliability standards.
TCP also provides data encapsulstaion and multiplexing via TCP Segments.

### TCP Segments
**Segments** are the PDU of TCP with Header and Payload.
Header Fields (non-exhaustive):
- Source Port
- Destination Port
- Sequence Number & Acknowledgement Number: Used together to provide reliability function (in-order delivery, data-loss handling, and duplication handling)
- Checksum: Error Detaction
- Window Size: The amount of data willing to be accepted (can be re-established at any point in communication)
- Flag Fields: Boolean values which signal urgency as well as establish, end, and manage state in a TCP connection.


### TCP Connections
The Three Way Handshake:`SYN` and `ACK` flags (Send, Acknowledge) are used to establish a connection
1. Sender sends `SYN`
2. Receiver responts with `SYN` and `ACK`
3. Sender sends `ACK`
Once final `ACK` is received, connection is established.

### States
- CLOSED - State of Client prior to `SYN`
- LISTEN - State of Server before request
- SYN-SENT - State of Client from dending of `SYN` sent until `SYN` `ACK` and `ACK` is sent
- SYN-RECIEVED - State when Server has sent `SYN` `ACK` and awaits final `ACK`
- ESTABLISHED - State when Ack send (Client) or Ack Received (Server)

### Overcoming Latency
1. Flow Control: Prevents client from overwhelming Server with too much info at once
2. Congestion Avoidance: TCP uses algorithms to understand when routers are buffering, since data is usually lost in these situations and indicates that the network is crowded.  TCP will start to send and request smaller window sizes as needed.

### Drawbacks
1. Overhead Latency in establishing a connection
2. Head Of Line (HOL) blocking: a result of in-order delivery is that the front packet becomes the bottleneck.


## UDP (User Datagram Protocol)
Connectionless transfer

### Datagram
**Datagrams** are the PDU of UDP. containing a Header and a Payload.
Header Fields:
- Source Port
- Destination Port
- Length
- Checksum: Optional with IPv4, but necessary for IPv6 which has no Checksum.

### UDP Cons
UDP Does not:
- guarantee message delivery
- guarantee order
- provide congestion aviodance or flow-control
- provide connection state tracking

### UDP Pros
Much faster and simpler
Serves as a template for software engineers who want to build only the services they require.
Example Case: Phone/Video services where dropped packets do not need to be retransmitted.

# Lesson 3: Intro to HTTP

## The Application Layer

OSI => Session, Presentation & Application layers

## The Web
The Intenet is a network of networks: the infrastructure in terms of physical network and low-level protocols
World Wide Web (Web) is a service accessed via the internet which is an information system of resources which
are navigable by URL (Uniform Resource Locators)
Made Possible by combining the following:
- HTML (Hypertext Markup Language): Hot to structure resources in Hypertext browsers using tags to assotiate text with sources
- URI (Uniform Resource Identifier): String of characters to identify a source (part of an addressing system)
- HTTP (Hypertext Transfer Protocol): Uniform way to transfer resources between applications.

## Overview of HTTP
HTTP is a message format for how machines transfer hypertext documents
Request Response Protocol: client makes a request to a server and waits for a response
Each device has a public IP Address and Port numbers through which to communicate.
DNS (Domain Name System): A distributed database which maps URIs to IP Addresses

Internet Workflow:
1. URL entered in Address Bar
2. Browser creates HTTP Request, packages and sends to your device's network interface
3. Device either uses the IP in its DNS Cash or submits a DNS Request to obtain it.
4. Packaged HTTP request goes over the internet to find the IP Address
5. Remote server accepts request and sends a response back to network interface
6. Interface hands to browser and browser displays

Or more simply: Clients sent HTTP requests, and servers send back HTTP Responses
HTTP is **stateless* as each request/response is completely independent of previous ones.
Applications which create state (logged in) are created despite HTTP's stateless transfer of information.

## URL
5 Parts:
1. Scheme (aka Protocol): `http`, `ftp`, `mailto` `git`.  The first part before the `://`
2. Host: `www.example.com`. Tells client where teh resources is hosted/located
3. Port (Optional): `:88`. Only required to override the default port
4. Path (Optional): `/home/`. The resource being requested from host.
5. Query String (Optional): `?item=book`. Made of query parameters (name/value pairs) used to send data to the server.

Default Port: `80` used for HTTP requests

### Query Strings
Components
- `?` => reserved character marking the start
- `search=ruby` => parameter name/value pair
- `&` => reserved character to add more parameters

Limitations:
- URL (including Query String) limited at 2048 characters
- Visible in URL: not usable for sensitive information
- Spaces & Special Characters must be encoded

Encoding: Using a `%` followed by the ASCII code of reserved Characters

## Status Codes
- 200: Found and Sucessfully returned
- 302: Found but moved.  Redirect based on re-routed URL in `location` response header.  This code is also used to redirect when trying to access a page without authentication or setting up first.
- 404: Not Found
- 500: Internal Server Error

## State

### Sessions
When a server sends a unique token (called a **session identifier** to the client, and the client appends this token as part of the request for identification.
Passing Session IDs creates a sense of persistent connection
Steps for using Session ID's
1. Every request must be inspected for the session identification
2. Server must check validity of session ID. Servers have rules on how to store and handle session id & expiration
3. Server must retrieve session data based on session id
4. Session must recreate application state from session data to send in response back to client.

### Cookies
Data sent from the server and stored in the client containing session information.
Cookies are set by the `set cookie` response heaer and will be sent to the server to identify you in future requests.

### AJAX
Asynchronous JavaScript and XML: Allows browsers to issue requests and process responses *without full page refresh*
`callback`s are executed to handle the asynchronous responses and can refresh the page as the responses are returned.

## Security

### HTTPS (Secure HTTP)
HTTP sends strings for request/responses, susceptible to *packet sniffing* comprimising your session id.
HTTPS encrypts each request/response using TLS (Transport Layer Security) encryption
TLS uses certificates to communicat with servers and exchange security keys before encryption

### Same-Origin Policy
Origin is the URL Scheme, Hostname, and Port. 
- `http` => `https` is not allowed (different scheme)
CORS (Cross-origin Resource Sharing) allows normally restricted interactions by adding new HTTP headers

### Avoiding Session Hijacking
- Resetting Sessions: Successful login invalidates old session id
- Expiration time
- HTTPS across app

### Cross-Site Scripting (XSS)
Injection of HTML or JavaScript
Preventative Measures:
- Sanitize input (disallow HTML & JS)
- Escape all user input data when displaying.

## Server Components
1. Web Server: Responds to static asset requests: files, images, css, JS
2. Application Server: Application/Business Logic (i.e. server-side code when deployed) which handles request handling
3. Data Store: Persistant data

## Request Response Requirements

### Request
Required:
- Request-Line:
  - Request method: GET, HEAD, POST, etc.
  - Request-URI
- Headers
Optional
- Body

### Response
Required:
- Status-Line: 
  - HTTP Version
  - Status-Code: 200 / 400 / 404 Etc
  - Request-URI
Optional
- Headers
- Body

# Lesson 5: Transport Layer Security (TLS)

HTTP Requests and Responses are essentially insecure since they transfer plain text.
TLS is used in conjunction with TCP as the Transport Layer protocol
TLS is considered a Session layer Protocol in OSI. 

TLS Security Services:
1. Encryption
2. Authentication
3. Integrity

## Encryption
TLS Protocol adds Security to HTTP Communications Using Symmetric and Asymmetric Key Encryption.
- Initial Key Encryption is asymmetrically entrypted
- All subsequent communications are symmetrically encrypted.

Symmetric Key Encryption: Both parties have a copy of the same key to encrypt and decrypt the message.
Asymmetric Key Encryption: Uses a Public to encrypt messages and Private Key to decrypt messages
- Asymetric encryption works one way

### TLS Handshake
TLS Handshake is the process by which client and server exchange encryption keys.
TLS Handshake must be performed before secure data exchange can begin.

### TLS Step-By-Step (From end of TCP Handshake `ACK`)
1. Client Initiation: `ClientHello`
  - `ClientHello` specifies Max version of TLS protocol and List of Cipher Suites supported by client
2. Server Response: `ServerHello`, Certificate, `ServerHelloDone`
  - `ServerHello` Sets Protocol Version and Selects Cipher Suite
  - Certificate sent Containing Public Key
  - `ServerHelloDone` indicates completion
3. Client Response & Completion: `ClientKeyExchange`, `ChangeCipherSpec`, `Finished`
  - RSA Example:
    - 'pre-master secret' encrypted using server's public key sent to server
    - Server recieves and decrypts using private key
    - Both Client and Server use 'pre-master secret' to generate symmetric key
    - `ClientKeyExchange` and `ChangeCipherSpec` flag tells server to begin using symmetric key
    - `Finished` flag indicates client has completed TLS Handshake
4. Server Response & Completion: `ChangeCipherSpec` and `Finished` flags.

Latency is now 3 round trips for TCP and 4 round trips for TLS.

### Cipher Suites
Cipher Suite is the agreed upon set of algorithms used by client and server during secure message exchange

### Certificates
TLS Authentication verifies the identity during message exchange by exchanging Digital Certificates in the TLS Handshake
- Certificates are signed by a Certificate Authority
- Chain of Trust is rooted in a small group of highly trusted Root CAs

Certificate Authorities (CAs) issues certificates by:
1. Verifying the requesting party's identity matches their asseriton.
2. Digitally signing the certificate by encrypting data with CA's own private key.

To view Certificate in browser, click padlock icon and click on "Certificate" and "Details".
- Often follows the following Chain of Trust: Site Certificate => Intermediate CA Certificate => Root CA's Certificate

## TLS Integrity
TLS Integrity checks whether a message has been altered or interfered with in transit using Message Authentication Code (MAC)
TLS PDU: 
- Headers:
  - Content Type
  - TLS Version
  - Length
- Payload
- Trailer:
  - MAC
  - Padding

### Message Authentication Code (MAC)
1. Sender creates a *digest* of payload.
  - Digest is a small amount of data derived from the actual data using a hashing algorithm with a pre-agreed hash value.
  - The hashing information was determined in the TLS Handshake Cipher Suite Negotiation.
2. Sender encrypts payload using symmetric key and sent PDU
3. Receiver decrypts payload using symmetric key and create a digest of the payload using the same algorithm and hash value
  - If digests match, integrity is confirmed.

# Lesson 6: The Evolution of Network Technologies

## HTTP Versions

### HTTP/0.9
- Released 1991: the original version
- Simple: referred as 'One line protocol' with no headers or request body, just the method and path.
- No Status Codes, Meta-data, non `GET` methods

### HTTP/1.0
Mass internet adoption from 1991-1996 for the following reasons: 
- First commercial browser (Netscape Navigator 1.0)
- Dial-Up Connections
- W3C and HTTP-WG formed to document and standardize web technology development

Improvements:
- `POST` and `HEAD` added.
- Request-URI could be relative path or absolute URI.
- HTTP Versions added to request line
- Status Lines/Codes added to Responsees
- HTTP Headers for request/response handling meta-data

### HTTP/1.1
Released 1997/Updated 1999: Adoption of JS (1995) and CSS (1996) exposes ineffieiencies and ambiguties
- Connection Re-use: No separate TCP handshake for each request
- Pipelining Requests: Client can send multiple requests before receiving a response
- Cashe-Control
- New Methods: `PUT`, `DELETE`, `TRACE`, `OPTIONS`

### HTTP/2
Standardized in 2015: Improves inefficiencies
- Multiplexing implemented to replace pipelining
- Compression of headers

### HTTP/3
QUIC.  In the works.


## Optimizations
Document-Aware Optimization: Browser leverages networking integrated with parsing techniquest to identify and prioritize fetchin resources (prioritizing expensive resources first to effiently load a page)
Speculative Optimizations: Pre-resolving DNS names, pre-rendering pages to frequently visited sites, open TCP connection when hovering over a link.

### Optimization Techniques:
- Reduce Bloat: Only sending what is necessary (JS, CSS, HTML) and limiting excessive requests
- Data Compression:
  - gzip for text (HTML/CSS/JS)
  - minification: remove unnecessary/redundant data like white-space, comments, formatting, unused code, shorter variable/function names
- Reusing TCP Connections (keepalive):
  - Standard as of HTTP/1.1 as long as application and server configurations support.
  - HTTP/1.0 can be used by adding 'Connection: Keep-Alive' Header
- DNS Optimizations:
  - Limit number of host-names needed for loading a page or locally download external resources on the hosting server.
  - Premium Services: Amazon Route 53 and DNS Made Easy boast faster services than GoDaddy or NameCheap.
- Caching (Server-Side):
  - Short Term Memory for recently accessed content

## Browser Networking APIs
Real-Time Data Synchronization (i.e. automatic page refreshes like notifications) are not well-managed by request-response cycle.
Functionality is achieved through a variety of APIs:

### XHR (XMLHttpRequest)
XML = Extensible Markup Language: used for encoding documents
SHR can also use JSON, HTML, or plain text.
XHR Manages requests/responses programmatically and asynchronously.
XHR is a key component of AJAX (Asynchronous JS and XML), allowing webpages to be re-rendered without re-loading the entire page.
Polling: issuing periodic requests to check for updated (lots of unnecessary request/responses)
Long-Polling: Client requests, but server keeps connection idle and sends response only when update is available.

### SSE (Server-Sent Events)
Enables efficient server-to-client streaming of text-based event data.
Client keeps single, long-lived TCP connection for messages pushed from server

Trade Offs:
- Only works with client-server model
- Does not allow request streaming (client to server) on same connection
- Streaming support is designed for UTF-8 data, not binary streaming.

### WebSocket
The only transport allowing client and server a persistent TCP connection using bidirectional communication (server => client && client => server)
Trade Offs:
- Application must handle state management, compression, caching (otherwise provided by browser)

## P2P (Peer-to-peer)
Instead of client-server, all computers are nodes

### Pros
- No server set-up and maintenance
- Resiliency: distributed compute resources (good for File Sharing)
- Reduced latency by removal of middle-man (server) (good for Real Time Communication like voice or video calls)

### Cons
- Discovery: Other nodes on network are not dependably available as a dedicated server would be.
  - Flooding: sending message an dforwarding to find a node which can server
  - Distributed Hash Table (DHT): table of key-value pairs acting as DNS for nodes
  - Hybrid: Central Server for node discovery
    - Client-Server for discovery
    - P2P once connected
- Other: Connnection negotioation/extablishment, security, performance, scaling.

### WebRTC (Real-time communication)
Collection of Standards, Protocols, and APIs which allows browser to act as P2P node
- UDP at Transport Layer
- Additional Protocols to manage UDP unreliability:
  - Session Establishment & Maintenance (STUN, TURN, ICE)
  - Security (DTLS)
  - Reliability: Congestion and Flow Control  (SRTP, SCTP)
  - 





