---
layout: post
title: "Pentest Primer: Basic Networking Concepts: IPs, Subnets & Devices"
date: 2024-11-16
---

{% include youtube.html id="WdqiOAg0tGk" %}

This guide explores the fundamentals of networking with a particular emphasis on its role in ethical hacking and cybersecurity. Whether you're new to networking or looking to strengthen your foundation, this tutorial covers the essential concepts without overwhelming technical complexity.

Understanding networking is crucial for anyone pursuing a career in cybersecurity or penetration testing. Networks form the backbone of modern computing, and security professionals must understand how devices communicate to effectively identify and exploit vulnerabilities.

What is Networking?
---
Networking, at its core, is the practice of connecting computers and devices to share resources and information. Its significance lies in its ability to facilitate communication between these devices. Think of a network as a digital community where devices collaborate to make information sharing efficient and accessible.

When we talk about networking, we're describing how devices communicate with each other. It's not about complicated algorithms; it's about ensuring your computer can effectively communicate with other devices, whether that's a friend's tablet, a web server across the internet, or a printer in your office.

The fundamental purpose of networking is to enable resource sharing, including:
- Files and documents
- Printers and other peripherals
- Internet connectivity
- Applications and services
- Computing power and storage

Network Devices
---
In the world of networking, we use various devices to facilitate communication and ensure data reaches its intended destination. Think of these devices as digital traffic directors, managing the flow of information across the network.

**Routers**: Devices that connect different networks together and route traffic between them. Your home router connects your local network to the internet, making decisions about where data should go. In enterprise environments, routers handle complex routing decisions across vast networks.

**Switches**: Devices that connect multiple devices within the same network. Switches operate at the data link layer and use MAC addresses to forward data to the correct device. They're more intelligent than hubs, sending data only to the intended recipient rather than broadcasting to all connected devices.

**Firewalls**: Security devices that monitor and control network traffic based on predetermined security rules. Firewalls act as barriers between trusted internal networks and untrusted external networks like the internet.

**Access Points**: Devices that allow wireless devices to connect to a wired network. They extend network connectivity without requiring physical cables.

Understanding these devices is essential for penetration testing, as each represents potential attack vectors or security controls that must be assessed.

IP Addresses
---
IP (Internet Protocol) addresses are fundamental to networking. They serve as unique identifiers for devices on a network, similar to how street addresses identify houses in a city.

**IPv4 Addresses**: The most common IP address format, consisting of four numbers separated by periods (e.g., 192.168.1.100). Each number ranges from 0 to 255, providing approximately 4.3 billion unique addresses.

**IPv6 Addresses**: The newer IP address format designed to address IPv4 exhaustion. IPv6 uses 128-bit addresses represented in hexadecimal notation, providing an astronomical number of unique addresses.

**Public vs. Private IP Addresses**:
- **Public IPs**: Addresses that are routable on the internet and must be globally unique
- **Private IPs**: Addresses reserved for internal networks, not directly accessible from the internet. Common private ranges include 192.168.0.0/16, 172.16.0.0/12, and 10.0.0.0/8

**Static vs. Dynamic IPs**:
- **Static**: Manually configured addresses that don't change
- **Dynamic**: Automatically assigned addresses using DHCP that may change over time

Subnetting
---
Subnetting is the practice of dividing a larger network into smaller, more manageable sub-networks (subnets). This serves several important purposes:

**Improved Organization**: Subnetting allows network administrators to logically organize devices based on department, function, or security requirements.

**Enhanced Security**: By separating networks into subnets, organizations can implement access controls and security policies between different network segments. For example, keeping the finance department's subnet separate from the guest WiFi network.

**Optimized Performance**: Smaller broadcast domains reduce unnecessary network traffic, improving overall performance.

**Efficient IP Address Allocation**: Subnetting allows for more efficient use of available IP address space by allocating appropriately sized address blocks.

Understanding subnetting is crucial for penetration testers because:
- It reveals the network architecture and segmentation strategy
- Identifies potential targets and trust boundaries
- Helps plan lateral movement during penetration testing
- Reveals security controls and network policies

Subnet Masks and CIDR Notation
---
Subnet masks determine which portion of an IP address represents the network and which portion represents the host. Common subnet masks include:

- 255.255.255.0 (/24): Provides 254 usable host addresses
- 255.255.0.0 (/16): Provides approximately 65,000 usable host addresses
- 255.255.255.128 (/25): Provides 126 usable host addresses

CIDR (Classless Inter-Domain Routing) notation provides a compact way to represent IP addresses and their associated subnet masks, using a slash followed by the number of network bits (e.g., 192.168.1.0/24).

Networking in Ethical Hacking
---
Understanding networking fundamentals is essential for ethical hacking because:

**Reconnaissance**: Network scanning and enumeration reveal the structure and devices present in a target environment.

**Lateral Movement**: After gaining initial access, understanding network segmentation helps identify paths to move between systems.

**Man-in-the-Middle Attacks**: Understanding how devices communicate enables interception and manipulation of network traffic.

**Network-Based Exploitation**: Many attacks target network protocols and services.

**Defensive Understanding**: To secure networks, you must understand how they function and where vulnerabilities exist.

This foundational knowledge prepares you to understand more advanced networking concepts and security testing techniques covered in subsequent lessons.

