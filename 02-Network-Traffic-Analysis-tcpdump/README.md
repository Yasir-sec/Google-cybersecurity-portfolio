# Network Traffic Analysis and Packet Inspection with tcpdump

## 1. Project Overview
This project examines low-level network traffic patterns and protocol behavior captured using the command-line packet analyzer tcpdump. The primary focus is verifying key phases of network communication: Domain Name System (DNS) query resolution, the Transmission Control Protocol (TCP) three-way handshake sequence, and application-layer Hypertext Transfer Protocol (HTTP) transmission flows.

---

## 2. Environment & Protocols Inspected
* *Capture Tool:* tcpdump
* *Transport & Network Layers:* IPv4, TCP, UDP
* *Application Protocols:* DNS (UDP 53), HTTP (TCP 80)
* *Target Services:* yummyrecipesforme.com, greatrecipesforme.com

---

## 3. Packet Inspection & Technical Breakdown

### A. DNS Resolution (UDP Port 53)
* *Query:* The local host (your.machine.52444) submitted standard DNS lookup queries (A?) to dns.google.domain requesting destination IP records for target domains.
* *Response:* The DNS resolver returned valid A records (e.g., mapping to IP 203.0.113.22), providing the destination address required to initiate transport connections.

### B. TCP Three-Way Handshake Verification
The packet trace clearly establishes the stateful connection lifecycle between client and web server:

1. *SYN (Flags [S]):*
   * Log Entry: your.machine.36086 > yummyrecipesforme.com.http: Flags [S], seq 2873951608
   * Analysis: The client initiates the connection by synchronizing sequence numbers and advertising window buffer capabilities.
2. *SYN-ACK (Flags [S.]):*
   * Log Entry: yummyrecipesforme.com.http > your.machine.36086: Flags [S.], seq 3984334959, ack 2873951609
   * Analysis: The web server acknowledges the client's sequence number (ack = seq + 1) and responds with its own initial sequence number.
3. *ACK (Flags [.]):*
   * Log Entry: your.machine.36086 > yummyrecipesforme.com.http: Flags [.], ack 1
   * Analysis: The client confirms receipt of the server's synchronization flag, finalizing the three-way handshake and placing the session into an ESTABLISHED state.

### C. Application-Layer Interaction (HTTP)
* *Data Transmission (Flags [P.]):*
  * Immediately following connection setup, the client transmits a PSH flag containing an HTTP: GET / HTTP/1.1 request across port 80.
  * Subsequent packets demonstrate continuous payload delivery, sequence incrementation, and server acknowledgments.

---

## 4. Log Inspection Snippet

```text

14:18:32.192571 IP your.machine.52444 > dns.google.domain: 35084+ A? yummyrecipesforme.com. (24)
14:18:32.204388 IP dns.google.domain > your.machine.52444: 35084 1/0/0 A 203.0.113.22 (40)

14:18:36.786501 IP your.machine.36086 > yummyrecipesforme.com.http: Flags [S], seq 2873951608, win 65495...
14:18:36.786517 IP yummyrecipesforme.com.http > your.machine.36086: Flags [S.], seq 3984334959, ack 2873951609...
14:18:36.786529 IP your.machine.36086 > yummyrecipesforme.com.http: Flags [.], ack 1, win 512...
14:18:36.786589 IP your.machine.36086 > yummyrecipesforme.com.http: Flags [P.], seq 1:74, ack 1: HTTP: GET / HTTP/1.1

```

## 5. Security & Defensive Takeaways
* Reconnaissance & Threat Detection: Understanding normal TCP flag progression is vital for Security Operations Center (SOC) analysts. Analyzing flag    anomalies helps distinguish legitimate traffic from port scans (such as SYN stealth scans) or SYN flood Denial-of-Service attempts.** Encrypted vs.  Unencrypted Exposure: Cleartext protocols (HTTP on port 80) expose application headers and payload data directly to packet sniffers. Network perimeters must enforce TLS/HTTPS encryption across enterprise environments.
* Command-Line Incident Triage: Mastery of ⁠tcpdump⁠ allows rapid packet troubleshooting and incident triage directly on edge devices, firewalls, and Linux servers without requiring a GUI.
## 6. Project Artifacts
*📄**Full Raw Capture File:**[View PDF Report](02-Network-Traffic-Analysis-tcpdump/tcpdump _traffic _analysis.pdf)
