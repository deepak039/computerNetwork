# Internet and Network Layers

## Outline

- What is the Internet made of?
- Visualize all the steps when you browse a webpage
- Layering abstraction, encapsulation
- Overview of all the layers of the network stack
- Circuit switching vs packet switching
- A brief history of the Internet

## What is the Internet Made Of?

- **Computers connected by links.**
- Multiple links terminate at a switch/router. Switches and routers connect a set of computers to one another.
- Many switches/routers in an organization like IITB connect all the end hosts.
- **Internet Service Providers (ISPs)** connect multiple end-user organizations.
- ISP networks have several routers that provide interconnections and form the **"backbone"** of the Internet.
- Some end hosts also provide useful services and applications (e.g., web and mail servers). So the Internet is mainly **end hosts** and **routers**.

### IP Addresses
- Every machine on the Internet has a unique address called the **IP address**.
- IP addresses are assigned **statically** or **dynamically** (e.g., DHCP). They can be **public** or **private** (like in IITB).

## What Happens When You Open a Web Page?
1. **URL Resolution:** The URL is resolved into the IP address of the web server via DNS.
2. **TCP Connection:** The browser opens a TCP connection to the web server and sends a GET request for the web page.
3. **Server Response:** The server sends some HTML content, which the browser displays.
4. **Additional Requests:** The browser may send multiple GET requests for images and other objects, possibly opening multiple TCP connections.
5. **Routing:** TCP data is routed through several routers from the client's IP to the server's IP.
6. **Final Step:** The IP router next to the web server hands the packet to the server via LAN.
7. **Communication Continues:** Several TCP segments and acknowledgments are exchanged until the full web page is transferred.

## Concept of Layering

- **Layering**: Each layer in the networking stack handles specific functions, communicating with its "peer" layer at the other end using predefined protocols.
- Layers provide **abstraction** or **services** to the layers above them.
- Example: The **application layer** at the client sends a GET request, and the application layer at the server responds with the requested page.

### Key Layers (5 Layers System):
1. **Application Layer**
2. **Transport Layer**
3. **Network Layer**
4. **Link Layer**
5. **Physical Layer**

## Encapsulation and Decapsulation

- Each layer adds **headers** to the data from the layer above it. These headers assist in the layer’s operations and are removed by the peer layer at the other end.

## Overview of Layers

### Application Layer
- Focuses on **application-level semantics**.
- Example: A browser issues GET requests and displays results, while the web server holds pages and serves them.
- **Popular Protocols:** HTTP, SMTP, P2P protocols.

### Transport Layer (TCP/UDP)
- **TCP (Transmission Control Protocol):** Handles the transfer of application data, ensuring reliability and proper order.
- **UDP (User Datagram Protocol):** Provides a simpler, unreliable transport.
- **Key Responsibilities:** Segmentation, flow control, congestion control, retransmission.

### Network Layer (IP Layer)
- **IP Datagrams:** Responsible for routing datagrams to their destination IP addresses.
- **Routing:** Routers use **forwarding tables** to decide the next hop for each datagram.

### Routing Protocols
- **Intradomain Routing:** E.g., OSPF within an organization for shortest path routing.
- **Interdomain Routing (BGP):** Connects different organizations and handles routing policies.

### Link Layer
- **Link Layer Frames:** Deals with local delivery of packets over a LAN, converting IP addresses to MAC addresses using ARP and transmitting data over the LAN.

### Physical Layer
- Transmits actual bits over physical media like copper wires, optical fibers, or wireless radio.

## Circuit Switching vs Packet Switching

- **Circuit Switching:** Reserves resources along the entire path of data flow, ensuring a dedicated circuit. Used in traditional telephone networks.
- **Packet Switching:** Data is broken into packets and forwarded through intermediate nodes using **store-and-forward** techniques. Internet networks use packet switching.

## Statistical Multiplexing vs Time Division Multiplexing (TDM)

- **TDM:** Resources are shared by allocating timeslots, which can result in resource wastage.
- **Statistical Multiplexing:** Resources are shared dynamically, providing better utilization but without guaranteed service quality.

### Problem Example: Multiplexing in Networking
- With statistical multiplexing, more users can be supported compared to TDM, but queuing and delays may occur during peak usage.

## History of Networking

- **1960s:** Concept of packet switching emerged.
- **1970s:** Proprietary networks like ARPANET developed, leading to the creation of top-level store-and-forward gateways and the primitive TCP/IP protocols.
- **1980s:** Refinement of TCP congestion control algorithms.
- **1990s:** Commercialization of the Internet with the rise of ISPs.
- **2000s:** Growth of web applications, stabilization of networks, and convergence with cellular data networks.

## End-to-End Principle

- **Intelligence at End Hosts:** Applications and transport layer functionality (e.g., reliability, security) should reside at the end hosts.
- **Core of the Network:** Should focus on efficient data transportation with minimal processing.

### Evolution of TCP/IP
- **TCP/IP Split:** Initially combined, the TCP and IP layers were later separated to allow flexibility in the transport layer and accommodate different application needs.

## Conclusion

The Internet is a complex, layered system of networks that connects end hosts through routers and switches. It uses IP addressing, encapsulation, and both circuit and packet switching methodologies to ensure reliable and efficient communication. The evolution of protocols and design philosophies such as the end-to-end principle has shaped the modern Internet. 

### Further Reading:
- [The design philosophy of the DARPA Internet protocols](https://www.cl.cam.ac.uk/teaching/2020/Resilience/papers/clark88.pdf) by David Clark

To model the performance of computer networks, we need to consider several key aspects such as throughput, end-to-end delay, packetization, and the differences between circuit switching and packet switching. Additionally, understanding the bandwidth-delay product and insights from queuing theory are essential for evaluating network performance.

### Key Concepts in Network Performance Modeling:

#### 1. **Throughput vs. Delay**
   - **Throughput** refers to how many bits per second are transmitted through the network. It can be thought of as the width of a pipe through which data flows.
   - **End-to-End Delay** represents the total time it takes for a bit to travel from the source to the destination. It’s analogous to the length of the pipe.
   - These two metrics are largely orthogonal: throughput measures capacity, while delay measures latency.

#### 2. **Throughput**
   - **Link Bandwidth** and **Datarate** represent the nominal rate at which data can be transmitted over a link.
   - **Goodput** (or throughput) represents the actual number of useful bits received at the destination, excluding retransmissions and errors.
   - The overall throughput of a connection across multiple links is determined by the slowest link, known as the **bottleneck link**.
   - If the sender exceeds the bottleneck link's capacity, a queue will form at the router connected to that link.

#### 3. **Bottleneck Dynamics**
   - The bottleneck isn’t always the slowest nominal datarate; a high-speed link might still become a bottleneck if multiple flows share it.
   - Protocol-induced delays (e.g., waiting for acknowledgments) can further reduce the average throughput.
   - **Example Problem:**
     - A 125 KB file sent over a network with a bottleneck bandwidth of 1 Mbps would take 1 second to transfer. However, if acknowledgments are required after each 1 KB packet with a 40 ms round trip time (RTT), the effective throughput reduces to 200 kbps, increasing the total transfer time to 5 seconds.

#### 4. **Delay Components**
   - **Transmission Delay:** The time to put all bits of a packet onto the wire (depends on link bandwidth).
   - **Propagation Delay:** The time for bits to travel through the medium (depends on the physical distance and the speed of wave propagation).
   - **Processing Delay:** The time to process the packet at the intermediate nodes (usually small with modern hardware).
   - **Queuing Delay:** The waiting time when packets are queued before transmission. This can vary based on network conditions and traffic.

   **Example:**
   - Consider a network where data travels between A and B with an intermediary switch S, each link 200 meters in length with a speed of 1 Mbps. The total propagation delay per link is 1 microsecond. The total end-to-end delay for a 125-byte packet would be approximately 2020 microseconds.

#### 5. **Packetization**
   - Breaking large data transfers into smaller packets can reduce total end-to-end delay, particularly in store-and-forward networks.
   - **Example:** Sending a 1 MB file over two 1 Mbps links. Sending it as a single large packet would take 16 seconds, while splitting it into 100-byte chunks would reduce the transfer time to 10.001 seconds, even accounting for the overhead of packet headers.

#### 6. **Circuit Switching vs. Packet Switching**
   - **Circuit Switching (CS):** Reserves a dedicated path between the source and destination for the entire duration of the transfer. Setup messages must be sent before data transmission begins.
   - **Packet Switching (PS):** Data is split into packets and routed independently across the network. This method is more efficient for bursty traffic but incurs higher delays due to packetization and routing.
   - **Example Comparison:** Transferring a 1 MB file using CS takes approximately 8.04 seconds, while using PS takes about 10.001 seconds, with more bytes sent in PS due to header overheads.

#### 7. **Queuing Theory**
   - **M/M/1 Queueing System:** Models packet arrivals as a Poisson process, with service times exponentially distributed. The system’s utilization is defined as ρ = λ/μ, where λ is the arrival rate, and μ is the service rate.
   - **Little’s Law:** The average number of customers in the system (N) is related to the arrival rate and average time in the system by N = λT.

   **Example:**
   - When traffic approaches the capacity of the link (i.e., L/R ≈ 1), queuing delays can grow disproportionately, leading to packet losses when queues fill up.

#### 8. **Bandwidth-Delay Product (BDP)**
   - The BDP measures how much data can be "in-flight" in the network at any time. It’s calculated as the product of the bandwidth and the round-trip time (RTT).
   - **Example:** For a 1 Mbps link with a 20 ms RTT, the BDP is 20,000 bits, or 2.5 KB. This means that a sender can have 2.5 packets in flight before receiving an acknowledgment.

#### 9. **Understanding BDP and TCP**
   - BDP helps in optimizing TCP performance by maintaining a window size equal to the BDP to fully utilize the link without waiting for acknowledgments unnecessarily.
   - **Example:** A 1 KB packet on a 1 Mbps link with a 20 ms RTT achieves an average throughput of 400 kbps when waiting for ACKs, demonstrating the importance of managing the in-flight data effectively.

By considering these factors, network performance can be analyzed and optimized for different conditions, such as traffic patterns, delays, and protocols in use.
