# The Internet
At the simplest level of abstraction, the internet is a network of networks.
Individual computers connect to one another using cables, wires, and waves, which communicate binary information
based on a set of protocols

# OSI Model
Comprised of 7 different layers based on Function:
- Application (Application / Presentation / Session)
  - HTTP: HOw to Access data via the WWW
- Transport (Application => Router => Application)
  - Multiplexing/Demultiplexing: Transmitting multiple signals on single channel via network ports
  - Ports are used to identify specific applications on hosts called communication end points
  - Sockets: IP Address combined with Port creates end-to-end communication
  - TCP: Connection-driven providing reliability at the cost of additional latency
    - Sends data in **Segments** containing:
      - Source/Dest POrts
      - Meta-Data for in-order delivery, data-loss handling, duplication handling, and error detection
    - Connection started with 3-way handshake: syn, syn/ack, ack.
    - Overcomes Latency using Flow Control and Congestion Avoidance
    - Drawbacks: Overhead latency and Head of Line Blocking
  - UDP: Connectionless foregoing reliability for speed
    - Deleviry and order are not guaranteed
    - NO Congestion Avoidance, flow-control, or connection state tracking provided
    - **Datagrams** as PDU contain source, destination, length, and checksum.
    - Great as a starting place (template) or phone/video services
- Network (Computer => Router => Computer)
  - Inter-Network: Protocols to connect hosts on different networks
  - IP Addresses have logical structure and are used to identify devices on a network.
  - Routers accept **packets** which have the following Data:
    - IP Version
    - Reassembly data (in-order delivery)
    - Data Payload Protocol (TCP/UDP)
    - IP Address Source & Destination
    - Checksum: Data dropped if checksum does not match
- Data Link (Computer => Switch)
  - Mostly used to get information from host to Switch/Router (LAN level)
  - Protocols like Ethernet provide structure to binary information transfer using Frames. 
  - Frames Contain the following:
    - MAC Addressing: Used to Identify physical computers on local network.
    - Other Metadata: Length of Transmission, synchronization of clocks
    - Payload: PDU from Network Layer
    - Checksum: Data dropped if checksum does not match
- Physical (Physical Infrastructure)
  - Individual computers connect to one another using cables, wires, and waves, to send binary information

# TCP/IP (Internet Protocol Suite) Model
Comprised of 4 different layers based on Scope:
- Application (Application, Presentation, Session, Transport)
- Tansport (Session, Transport)
- Internet (Network)
- Link (Data Link)

# PDUs
This layered approach allows for abstraction from each layer beneath, as each layer has specific 
endpoints and protocols with which they interact with one another.
Generally, the interaction involves a PDU (Protocol Data Unit) which is how data at the top level of abstraction
can be passed down to lower layers, where the necessary contextual data for the layer below is appended and passed down.

# Physical Layer
The Physical layer is the actual wires, cables, and waves which carry binary information in the form of electricity, waves, or light. 
Each method has its own tradeoffs: 
- Electicity trasmission is cheap but prone to interference and degradation.  
- Light is expensive, but does not suffer from the weaknesses of electricity.  
- Waves are great for mobility, but only up to a small radius before degrading.

## Latency
Latency is the amount of time for data to travel.  There are multiple types of latency:
- Propagation delay: Time to travel from sender to receiver on straight line
- Transmission Delay: Additional time navigating the network (intersections)
- Processing Delay: Additional time of processing (Switches/Routers/Hosts)
- Queuing Delay: Additional time for buffering at an overcrowded router
Honorable Mention:
- Last-Mile Latency: Referring to the final trip from ISP to local network
- Round-Trip Time: Time to reach destination and receive response.

## Bandwidth
Bandwidth is the amount of data sent in a set amount of time (usually seconds)
Total bandwidth is limited to the network's bottleneck.

# Data Link Layer
The physical layer provides the infrastructure, but the Data Link layer provides the logical structure via protocols like Ethernet.
Ethernet sends data from computers while also adding metadata in the PDU in the form of headers. These are called Frames.
- Protocols like Ethernet provide structure to binary information transfer using Frames. 
- Frames Contain the following:
  - MAC Addressing: Used to Identify physical computers on local network.
  - Other Metadata: Length of Transmission, synchronization of clocks
  - Payload: PDU from Network Layer
  - Checksum: Data dropped if checksum does not match

# Network Layer

## IP Addresses
IP Addresses have two forms:
IPv4: 32-bit address
IPv6: 128-bit address

IP Addresses are unique identifiers prescripted by the network to identify a device.
Routers read IP Addresses within the IP Packets in order to route to the appropriate network.
Ports are a way that a particular application 

# Transport Layer
Port Numbers allow hosts on different networks to connect to specific applications.

## TCP

### Three Way Handshake
The Three Way Handshake:`SYN` and `ACK` flags (Send, Acknowledge) are used to establish a connection
1. Sender sends `SYN`
2. Receiver responts with `SYN` and `ACK`
3. Sender sends `ACK`
Once final `ACK` is received, connection is established.

### Flow Control 
Uses WINDOW header field to communicate how much data may be sent at a time to avoid overwhelming the receiver

### Congestion Avoidance
Uses data loss as feedback and changes transmission window to avoid congesting the network.

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

# HTTP
Applications submit HTTP requests for every piece of data it needs to load a website
HTTP requests are sent in plain text and contain headers for metadata

## Request Response Cycle
1. URL entered in Address Bar
2. Browser creates HTTP Request, packages and sends to your device's network interface
3. Device either uses the IP in its DNS Cash or submits a DNS Request to obtain it.
4. Packaged HTTP request goes over the internet to find teh IP Address
5. REmote server accepts request and sends a response back to network interface
6. Interface hands to browser and browser displays

Or more simply: Clients sent HTTP requests, and servers send back HTTP Responses
HTTP is **stateless* as each request/response is completely independent of previous ones.
Applications which create state (logged in) are created despite HTTP's stateless transfer of information.

## DNS
DNS (Domain Name System) is a distributed system which stores Key/Value table with URI as Key and IP address as key.

## URLs

### Components of URLs
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
When to Encode:
1. No corresponding character in ASCII Character set
2. Character is unsave because it is it used for encoding other characters. `%#<>{}[]~`` and more
3. Characters reserved in URL scheme `/?:@&`

Save Characters => `$-_.+!'{}"`

## HTTP Request/Response Components

### `GET` and `POST` methods and when to choose each

### HTTP Requests:

### HTTP Responses:

## Status Codes with examples
Headers included in HTTP Responses

### Status Code Categories
- Informational responses (100–199)
- Successful responses (200–299)
- Redirection messages (300–399)
- Client error responses (400–499)
- Server error responses (500–599)

### Common Status Codes
- 200: OK
- 201: OK: new resource was created. Typically, this is the status code that is sent after a POST/PUT request.
- 301: Permanent Redirect
- 302: Temporary Redirect
- 304: The is status code used for browser caching. If the response has not been modified, the client/user can continue to use the same response/cached version
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

# Security

## Security risks in HTTP

### Mitigating HTTP Security Risks

## TLS Services

## Client-Server Model of Web Interactions

### Role of HTTP within Client-Server Model