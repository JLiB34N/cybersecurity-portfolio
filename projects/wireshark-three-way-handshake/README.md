# wireshark-3-way-handshake

## 1. Objective
The objective of this project is to understand how a TCP connection is established by capturing and analysing the TCP three-way handshake using Wireshark.

The experiment focuses on identifying the three main stages of TCP connection establishment: SYN, SYN-ACK and ACK.

Through this exercise, I aim to understand how a client and server establish communication, how TCP sequence and acknowledgement numbers work, and how network traffic analysis can help cybersecurity professionals understand network communication and identify potential security threats.

## 2. Environment and Tools
- Operating System: Windows 11
- Packet Analysis Software: Wireshark
- Packet Capture Driver: Npcap
- Additional Tools: Python, Windows Command Prompt, curl
- Lab Environment: Localhost (127.0.0.1)
- Server: Python HTTP Server
- Server Port: 8000

## 3. Methodology
The following steps were performed to capture and analyse the TCP three-way handshake.

### Step 1: Start a Local Python Web Server
Opened Windows Command Prompt and executed the following command: <b>py -m http.server 8000 --bind 127.0.0.1</b>

This command starts a simple HTTP server listening on port 8000.

The server is bound to the loopback IP address 127.0.0.1, meaning it only accepts connections from the local machine.

### Step 2: Start Wireshark
Opened Wireshark and located the Adapter for Loopback Traffic Capture interface.

Double-clicked the interface to begin capturing network traffic.

### Step 3: Generate an HTTP Request
Opened another Command Prompt window and executed: <b>curl http://127.0.0.1:8000</b>

This command sends an HTTP request to the local Python web server.

The request triggers a TCP connection, allowing Wireshark to capture the three-way handshake.

### Step 4: Filter the Captured Packets
Returned to Wireshark and entered the following display filter: <b>tcp.port == 8000</b>

This filter displays TCP packets associated with port 8000, making it easier to identify the connection establishment process.

### Step 5: Analyse the TCP Three-Way Handshake
Examined the captured packets and identified the three stages of the TCP handshake:
1. SYN
2. SYN-ACK
3. ACK

The source and destination ports, TCP flags, sequence numbers and acknowledgement numbers were examined to understand how the connection was established.

The experiment was conducted on a local Windows machine. The Python HTTP server acted as the server, while curl acted as the client generating HTTP requests.

Wireshark was used to capture and analyse the network packets exchanged between the client and server through the loopback interface.

## 4. Results and Findings
The experiment successfully captured the TCP three-way handshake between the client and the local Python web server.

Both the client and server used the same IP address, 127.0.0.1, because the communication occurred within the same computer through the loopback interface.

However, they used different port numbers:
- Client port: 65251
- Server port: 8000

The client used an ephemeral port (65251) to communicate with the server, while the Python HTTP server listened on port 8000.

1. SYN (Synchronisation)
The first packet was sent from the client to the server.
- Source Port: 65251
- Destination Port: 8000
- TCP Flag: SYN
- Relative Sequence Number: 0
- Relative Acknowledgement Number: 0

The SYN flag indicates that the client wants to establish a TCP connection with the server.

The client sends an initial sequence number to begin the connection establishment process.

The acknowledgement number is 0 in Wireshark's relative sequence number display because the client has not yet received a sequence number from the server to acknowledge.

2. SYN-ACK (Synchronisation-Acknowledgement)
The second packet was sent from the server back to the client.
- Source Port: 8000
- Destination Port: 65251
- TCP Flags: SYN, ACK
- Relative Sequence Number: 0
- Relative Acknowledgement Number: 1

The server responds with SYN-ACK to indicate that it has received the client's connection request and is willing to establish the connection.

The acknowledgement number increases to 1 because the server acknowledges the client's SYN packet.

The server also sends its own initial sequence number to the client.

3. ACK (Acknowledgement)
The third packet was sent from the client to the server.
- Source Port: 65251
- Destination Port: 8000
- TCP Flag: ACK
- Relative Sequence Number: 1
- Relative Acknowledgement Number: 1

The client sends an ACK packet to acknowledge the server's SYN-ACK response.

The relative sequence number increases to 1 because the SYN flag consumes one sequence number.

The acknowledgement number also becomes 1 to acknowledge the server's initial sequence number.

After this exchange, the TCP three-way handshake is complete, and both sides can begin exchanging application data.

Screenshots of the experiment are available in the evidence/ directory.

## 5. Security Implications
Although the TCP three-way handshake is essential for establishing reliable communication, attackers can exploit this mechanism through a SYN flood attack, which is a type of Denial-of-Service (DoS) attack.

In a SYN flood attack, an attacker sends a large number of SYN packets to a target server without completing the TCP three-way handshake.

The server responds to each SYN request with a SYN-ACK packet and waits for the final ACK from the client.

If many connections remain incomplete, the server may consume resources maintaining these pending connection requests. This can prevent legitimate users from establishing connections.

Some defensive measures against SYN flood attacks include:
1. SYN Cookies: Help reduce resource consumption associated with incomplete TCP connections.
2. Firewall Configuration: Apply appropriate filtering and connection-handling rules.
3. Rate Limiting: Limit excessive connection requests from suspicious sources.
4. Network Monitoring: Use network analysis tools such as Wireshark to identify unusual patterns of repeated SYN packets.

This experiment helped me understand how a normal TCP connection is established and how attackers may abuse the same mechanism to disrupt network availability.

## 6. Challenges Encountered
No major technical difficulties were encountered during this experiment.

The local Python web server was successfully started, and Wireshark was able to capture the TCP packets generated by the curl request.

## 7. Improvements and Future Work
Although this experiment successfully demonstrated the TCP three-way handshake, there are several areas that could be explored further.

For future work, I plan to:
1. Capture and analyse TCP connection termination using FIN and ACK packets.
2. Investigate TCP connection resets (RST) and understand when they occur.
3. Compare successful TCP connections with unsuccessful connection attempts.
4. Investigate SYN flood attacks in an isolated lab environment to understand their effects on network availability.
5. Explore how firewalls and other defensive mechanisms detect and mitigate suspicious TCP connection patterns.

These additional experiments would provide a deeper understanding of TCP communication and network security.

## 8. Conclusion
This project provided practical experience in capturing and analysing TCP traffic using Wireshark.

By creating a local Python web server and generating an HTTP request using curl, I successfully captured the TCP three-way handshake.

I identified the three stages of connection establishment: SYN, SYN-ACK and ACK. I also learned how source ports, destination ports, sequence numbers and acknowledgement numbers are used during TCP communication.

One important lesson from this experiment is that understanding normal network behaviour is essential for identifying abnormal activities. The TCP three-way handshake is not only fundamental to network communication but can also be abused through attacks such as SYN flooding.

Overall, this experiment improved my understanding of TCP connection establishment and demonstrated how Wireshark can be used as a practical tool for network traffic analysis and cybersecurity investigation.

