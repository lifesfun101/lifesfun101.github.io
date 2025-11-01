---
layout: post
title: "Pentest Primer: Networking Ports"
date: 2024-12-06
---

Ports in computer networking are critical to facilitating efficient data exchange within networks, enabling communication between devices via specific channels. Understanding ports is fundamental for anyone pursuing cybersecurity or penetration testing.

Think of IP addresses as street addresses for buildings, and ports as specific apartment numbers within those buildings. While the IP address gets network traffic to the right device, the port number directs that traffic to the correct application or service running on that device.

{% include youtube.html id="OUIqmpzBVZE" %}

Understanding Ports
---
A network port is a virtual communication endpoint that identifies a specific process or service on a device. Ports allow a single device with one IP address to run multiple network services simultaneously.

**Port Numbers**: Ports are identified by numbers ranging from 0 to 65535. These port numbers are standardized to ensure that different devices and applications can communicate predictably across networks.

**How Ports Work**: When data arrives at a device, the port number tells the operating system which application should receive the data. For example:

- Web browser traffic goes to port 80 (HTTP) or 443 (HTTPS)
- Email traffic might use port 25 (SMTP), 110 (POP3), or 143 (IMAP)
- File transfer uses port 21 (FTP) or 22 (SFTP)
- Remote access uses port 22 (SSH) or 3389 (RDP)

**Protocols and Ports**: Ports work in conjunction with transport layer protocols (TCP or UDP). The same port number can be used by both TCP and UDP, but they're treated as separate ports. For example, DNS typically uses UDP port 53 for queries but TCP port 53 for zone transfers.

Types of Ports
---
Port numbers are divided into three main categories based on their usage and assignment:

**Well-Known Ports (0-1023)**: Reserved for common services and protocols. These ports are standardized by the Internet Assigned Numbers Authority (IANA) and used by system processes and widely-used applications.

Common well-known ports include:
- **Port 20/21**: FTP (File Transfer Protocol) - Unencrypted file transfer
- **Port 22**: SSH (Secure Shell) - Encrypted remote access
- **Port 23**: Telnet - Unencrypted remote access (insecure, avoid)
- **Port 25**: SMTP (Simple Mail Transfer Protocol) - Email transmission
- **Port 53**: DNS (Domain Name System) - Name resolution
- **Port 80**: HTTP (Hypertext Transfer Protocol) - Unencrypted web traffic
- **Port 110**: POP3 (Post Office Protocol) - Email retrieval
- **Port 143**: IMAP (Internet Message Access Protocol) - Email access
- **Port 443**: HTTPS (HTTP Secure) - Encrypted web traffic
- **Port 445**: SMB (Server Message Block) - Windows file sharing
- **Port 3306**: MySQL - Database server
- **Port 3389**: RDP (Remote Desktop Protocol) - Windows remote desktop

**Registered Ports (1024-49151)**: Assigned to specific services by IANA but not as strictly controlled as well-known ports. Organizations can register ports for their applications.

Examples include:
- **Port 1433**: Microsoft SQL Server
- **Port 1521**: Oracle Database
- **Port 3000**: Node.js development server (common default)
- **Port 5432**: PostgreSQL database
- **Port 8080**: Alternative HTTP port (often used for proxy servers or development)
- **Port 8443**: Alternative HTTPS port

**Dynamic/Private Ports (49152-65535)**: Also called ephemeral ports, these are used temporarily by client applications when connecting to servers. The operating system automatically assigns these ports for outbound connections.

When you browse a website, your browser doesn't use port 80 or 443 (those are for the server). Instead, your computer assigns a random high-numbered port for your side of the connection.

Port Security
---
Understanding port security is crucial for both penetration testing and defending networks against attacks. Ports represent potential entry points into systems, and improperly secured ports are among the most common vulnerabilities exploited by attackers.

**Open vs. Closed vs. Filtered Ports**:

- **Open Ports**: Actively accepting connections. Necessary for services to function but increase attack surface.
- **Closed Ports**: Not accepting connections but responding to probes. The service isn't running.
- **Filtered Ports**: A firewall or security device is blocking access. No response is received.

**Common Port-Related Vulnerabilities**:

**Unnecessary Open Ports**: Services running that aren't needed create unnecessary risk. Every open port is a potential attack vector.

**Outdated Services**: Running old versions of services on standard ports can expose systems to known vulnerabilities.

**Default Configurations**: Many services ship with insecure default settings that should be changed.

**Unencrypted Protocols**: Services like FTP (21), Telnet (23), and HTTP (80) transmit data in clear text, exposing credentials and sensitive information.

Port Security Best Practices
---
Securing network ports is essential for maintaining a strong security posture:

**Principle of Least Privilege**: Only open ports that are absolutely necessary for required services. Close or disable all others.

**Firewalls**: Implement network and host-based firewalls to restrict which ports are accessible and from where. Firewalls act as gatekeepers, controlling traffic based on port numbers, IP addresses, and protocols.

**Access Control Lists (ACLs)**: Configure ACLs on routers and switches to control which IP addresses can access specific ports. For example, administrative ports (like SSH on port 22) should only be accessible from trusted networks or IP addresses.

**Change Default Ports**: While security through obscurity shouldn't be your only defense, changing services from default ports can reduce automated attack success. For example, running SSH on a non-standard port reduces brute-force attempts from automated bots.

**Encryption**: Use encrypted versions of protocols whenever possible:
- HTTPS (443) instead of HTTP (80)
- SSH (22) instead of Telnet (23)
- SFTP (22) instead of FTP (21)
- IMAPS (993) instead of IMAP (143)

**Regular Port Scans**: Periodically scan your own systems to identify which ports are open and verify that only necessary services are exposed.

**Monitoring and Logging**: Monitor connection attempts to unusual ports or patterns that might indicate port scanning or attacks.

Ports in Penetration Testing
---
Understanding ports is fundamental to penetration testing for several reasons:

**Port Scanning**: One of the first steps in penetration testing is discovering which ports are open on target systems. Tools like Nmap systematically probe port ranges to identify accessible services.

**Service Identification**: Once ports are identified, determining which services and versions are running helps identify potential vulnerabilities.

**Exploit Selection**: Many exploits target specific services running on specific ports. Knowing that port 445 (SMB) is open immediately suggests certain attack vectors.

**Firewall Testing**: Understanding how firewalls filter ports helps testers identify ways to bypass or evade security controls.

**Lateral Movement**: After compromising one system, understanding internal port configurations helps identify paths to other systems.

Common Port Scanning Techniques
---
Port scanning is a core reconnaissance technique in penetration testing:

**TCP Connect Scan**: Completes full TCP three-way handshake. Most reliable but easily detected and logged.

**SYN Scan**: Sends SYN packets but doesn't complete handshake. Faster and stealthier than connect scans.

**UDP Scan**: Probes UDP ports. Slower and less reliable than TCP scanning but necessary for comprehensive discovery.

**Service Version Detection**: After identifying open ports, sends probes to determine which services and versions are running.

Why Ports Matter for Cybersecurity
---
Ports are among the most fundamental concepts in networking security:

- Every open port is a potential attack surface
- Many attacks specifically target services on well-known ports
- Misconfigurations in port security lead to unauthorized access
- Port-based filtering is a primary access control mechanism
- Understanding ports is prerequisite to using penetration testing tools

Whether you're attacking or defending networks, comprehensive knowledge of ports, their functions, and their security implications is essential for success in cybersecurity.
