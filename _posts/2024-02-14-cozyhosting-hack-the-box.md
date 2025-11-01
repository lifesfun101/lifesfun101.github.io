---
layout: post
title: "CozyHosting - Hack The Box"
date: 2024-02-14
---

This walkthrough covers CozyHosting, a Hack The Box machine that involves web application enumeration, exploiting Apache OFBiz authentication bypass vulnerabilities, database enumeration, hash cracking, and Linux privilege escalation.

CozyHosting demonstrates the exploitation of recent vulnerabilities in enterprise software and emphasizes the importance of thorough enumeration in both web applications and backend databases. This machine provides valuable experience with real-world CVE exploitation and credential harvesting techniques.

Web Application Enumeration
---
The initial reconnaissance phase focuses on enumerating a web application to identify the underlying technology and potential attack vectors. Proper enumeration reveals critical information about the application's structure, accessible endpoints, and authentication mechanisms.

Initial Foothold and Reverse Shell
---
The initial access is achieved through exploiting vulnerabilities in the web application. Once a foothold is established, obtaining a proper reverse shell allows for interactive command execution and further enumeration of the compromised system.

Apache OFBiz Authentication Bypass
---
Apache OFBiz is an open-source enterprise resource planning (ERP) system commonly used for business applications. Two critical authentication bypass vulnerabilities were discovered in late 2023 (CVE-2023-49070 and CVE-2023-51467) that allow attackers to bypass authentication mechanisms and potentially achieve remote code execution.

These vulnerabilities demonstrate how even widely-used enterprise software can contain critical security flaws. Organizations running Apache OFBiz should ensure they are running patched versions and monitor for suspicious authentication attempts.

System Enumeration with LinPEAS
---
After gaining initial access, thorough enumeration is performed using LinPEAS (Linux Privilege Escalation Awesome Script). This automated enumeration tool helps identify potential privilege escalation vectors, misconfigurations, and sensitive information stored on the system.

Writable Executable Enumeration
---
Part of the enumeration process involves identifying executables that may have improper permissions or configurations. Finding writable binaries or scripts can sometimes lead to privilege escalation opportunities if these files are executed by privileged users or processes.

Internal Port Discovery
---
During enumeration, an internal web port is discovered that is not accessible from external networks. This highlights the importance of comprehensive port scanning and service enumeration on compromised systems, as internal services often have weaker security controls than externally-facing applications.

Apache Derby Database Enumeration
---
The system is found to be running Apache Derby, a Java-based relational database often embedded in Java applications. Derby databases can contain valuable information including user credentials, configuration details, and application data. Understanding how to locate and extract data from Derby databases is essential for thorough post-exploitation enumeration.

Credential Recovery and Hash Cracking
---
Through database enumeration, password hashes are discovered and extracted. These hashes require identification and cracking to obtain clear text credentials. This phase demonstrates the importance of:

- Using strong, unique passwords that resist hash cracking
- Implementing proper password hashing algorithms with salting
- Regularly rotating credentials
- Monitoring for unauthorized database access

Privilege Escalation to Root
---
The final stage involves using the recovered credentials or identified misconfigurations to escalate privileges to root. This demonstrates how credential harvesting from databases combined with system misconfigurations can lead to complete system compromise.

Key Learning Points
---
This machine reinforces several important security concepts:
- Stay updated on CVEs affecting enterprise software
- Implement defense-in-depth for internal services
- Properly secure database files and credentials
- Use strong password hashing mechanisms
- Regularly audit systems for misconfigurations
- Monitor internal network traffic for lateral movement

{% include youtube.html id="UCtsUbK9URo" %}
