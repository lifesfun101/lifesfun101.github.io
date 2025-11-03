---
title: "Pentest Primer: Networking Protocols Practical Demo"
date: 2024-12-01 00:00:00 -0500
categories: [Tutorials, Pentest Primer]
tags: [pentest-primer, wireshark, packet-analysis, network-security, tools]
---

Welcome to this hands-on demonstration where we dive into essential network protocols and their practical applications in ethical hacking using Wireshark and command-line utilities.

Networking is the backbone of the internet, and understanding how protocols like TCP, UDP, HTTP, HTTPS, DNS, ICMP, and SSH work is critical for anyone stepping into the world of cybersecurity and penetration testing. These protocols dictate how devices communicate, share information, and secure data across networks.

In this lesson, we explore not only how these protocols function theoretically but also demonstrate their behavior using real-world tools. By the end of this session, you'll see how these protocols are leveraged in everyday network communication and, more importantly, how they're applied in ethical hacking scenarios.

{% include youtube.html id="robk-uS1jNM" %}

What is Wireshark?
---
Wireshark is a free, open-source packet analyzer that captures and displays network traffic in real-time. It's one of the most essential tools for network analysis, troubleshooting, and security testing. Wireshark allows you to see exactly what's happening on your network at a microscopic level.

For penetration testers and security professionals, Wireshark provides invaluable capabilities:

- **Packet Capture**: Intercept and log network traffic passing through a network interface
- **Protocol Analysis**: Decode and interpret hundreds of network protocols
- **Traffic Inspection**: Examine packet contents, headers, and payloads
- **Anomaly Detection**: Identify unusual patterns or suspicious activity
- **Credential Discovery**: Detect unencrypted credentials transmitted over the network
- **Forensic Analysis**: Investigate security incidents and reconstruct network events

Understanding TCP with Wireshark
---
The Transmission Control Protocol (TCP) is a connection-oriented protocol that provides reliable, ordered, and error-checked delivery of data. Wireshark excels at visualizing TCP communications.

**TCP Three-Way Handshake**: When analyzing TCP connections in Wireshark, you can observe the famous three-way handshake:

1. **SYN**: Client sends a synchronize packet to the server
2. **SYN-ACK**: Server responds with synchronize-acknowledgment
3. **ACK**: Client sends acknowledgment to establish the connection

Understanding this handshake is crucial for:
- Identifying connection establishment patterns
- Detecting SYN flood attacks
- Troubleshooting connectivity issues
- Understanding TCP port scanning techniques

**TCP Flags and States**: Wireshark displays TCP flags (SYN, ACK, FIN, RST, PSH, URG) that indicate the state and purpose of each packet. Learning to interpret these flags helps identify normal vs. suspicious traffic patterns.

UDP Traffic Analysis
---
Unlike TCP, User Datagram Protocol (UDP) is connectionless and doesn't establish formal connections. UDP prioritizes speed over reliability, making it ideal for time-sensitive applications like streaming, gaming, and DNS queries.

In Wireshark, UDP traffic appears simpler than TCP since there's no handshake or acknowledgment mechanism. However, analyzing UDP traffic is essential for:

- DNS query and response analysis
- VoIP and video streaming troubleshooting
- Detecting UDP-based attacks and scans
- Identifying covert channels

HTTP and HTTPS Analysis
---
HTTP (Hypertext Transfer Protocol) is the foundation of web communication. In Wireshark, HTTP traffic is easy to analyze because it's unencrypted text-based protocol.

**Analyzing HTTP Requests**: Wireshark can display:
- Request methods (GET, POST, PUT, DELETE)
- URLs and query parameters
- Headers (User-Agent, Cookies, Referrer)
- Request and response bodies
- Status codes (200, 404, 500, etc.)

**Security Implications**: Capturing HTTP traffic with Wireshark demonstrates why HTTPS is essential. You can literally read usernames, passwords, and sensitive data transmitted over HTTP in plain text.

**HTTPS Traffic**: While HTTPS traffic is encrypted, Wireshark can still reveal valuable information:
- TLS/SSL handshake process
- Certificate information
- Cipher suites negotiated
- Traffic volume and patterns
- Metadata (even though content is encrypted)

DNS Protocol Analysis
---
Domain Name System (DNS) translates human-readable domain names into IP addresses. DNS typically uses UDP port 53 for queries, though TCP is used for zone transfers.

**DNS in Wireshark**: You can observe:
- DNS queries (A, AAAA, MX, TXT records)
- DNS responses and resolved IP addresses
- Query timing and response times
- DNS cache poisoning attempts
- DNS tunneling and covert channels

Understanding DNS traffic is crucial for detecting:
- DNS spoofing and cache poisoning
- Data exfiltration via DNS tunneling
- Command and control communications
- Malware beaconing behavior

ICMP Protocol and Ping
---
Internet Control Message Protocol (ICMP) is used for diagnostic purposes, most commonly through the ping command. ICMP doesn't carry application data but provides feedback about network conditions.

**ICMP in Wireshark**: You can capture and analyze:
- Echo requests (ping) and replies
- Destination unreachable messages
- Time exceeded messages (traceroute)
- Redirect messages

**Security Considerations**: ICMP is commonly used for:
- Network reconnaissance and host discovery
- Ping sweeps to map networks
- Traceroute to discover network paths
- Covert channels for data exfiltration
- Denial of service (ping flood) attacks

SSH Protocol Analysis
---
Secure Shell (SSH) provides encrypted remote access to systems. While SSH traffic is encrypted and you can't see the commands or data being transmitted, Wireshark can still reveal important information:

- Connection establishment
- SSH version negotiation
- Authentication attempts (success/failure)
- Traffic volume and timing
- Session duration
- Connection source and destination

This information helps identify:
- SSH brute force attempts
- Unusual SSH activity
- Potential lateral movement
- Backdoor SSH sessions

Practical Applications in Ethical Hacking
---
Wireshark is an indispensable tool throughout the penetration testing lifecycle:

**Reconnaissance**: Passive network monitoring to identify hosts, services, and protocols in use

**Vulnerability Assessment**: Detecting unencrypted protocols, weak ciphers, and insecure configurations

**Exploitation**: Capturing credentials during man-in-the-middle attacks

**Post-Exploitation**: Monitoring network traffic to avoid detection or identify defensive measures

**Reporting**: Providing evidence of vulnerabilities and successful exploitation

Wireshark Capture Filters
---
Effective use of Wireshark requires understanding capture and display filters to focus on relevant traffic:

- `tcp.port == 80` - Capture HTTP traffic
- `dns` - Capture only DNS traffic
- `ip.addr == 192.168.1.100` - Capture traffic to/from specific IP
- `tcp.flags.syn == 1` - Capture SYN packets (connection attempts)

Best Practices for Using Wireshark
---
When using Wireshark for penetration testing or security analysis:

- Always obtain proper authorization before capturing network traffic
- Be aware of legal and privacy implications
- Use appropriate filters to manage large captures
- Save captures for later analysis and evidence
- Understand the protocols you're analyzing
- Practice on lab environments before production networks

Getting Started with Wireshark
---
To begin learning Wireshark:

1. Install Wireshark and set up a lab environment (Kali Linux VM recommended)
2. Start with simple captures (ping, web browsing, DNS lookups)
3. Learn to apply filters to isolate specific traffic
4. Practice analyzing the protocols covered in this lesson
5. Gradually work up to more complex traffic analysis scenarios

Wireshark is a skill that improves with practice. The more time you spend analyzing network traffic, the better you'll become at identifying patterns, anomalies, and security issues.
