---
layout: post
title: "Bizness - Hack The Box"
date: 2024-06-05
---

{% include youtube.html id="O8-cyFIzcI8" %}

This walkthrough covers Bizness, a Hack The Box machine that demonstrates exploitation of Apache OFBiz authentication bypass vulnerabilities (CVE-2023-49070 and CVE-2023-51467), Derby database enumeration, and privilege escalation through credential recovery.

Bizness showcases real-world exploitation of enterprise resource planning (ERP) software vulnerabilities and reinforces the importance of patching critical CVEs in business-critical applications. This machine provides hands-on experience with modern web application vulnerabilities and database forensics.

Web Application Enumeration
---
The initial phase involves thorough enumeration of the web application to identify the technology stack, accessible endpoints, and potential vulnerabilities. Discovering that the application runs Apache OFBiz immediately indicates potential attack vectors, especially given the recent disclosure of critical authentication bypass vulnerabilities.

Apache OFBiz Vulnerabilities
---
Apache OFBiz is a popular open-source ERP system used by organizations for managing business processes. In late 2023, two critical vulnerabilities were disclosed that allow unauthenticated attackers to bypass authentication mechanisms:

- **CVE-2023-49070**: Pre-authentication remote code execution vulnerability
- **CVE-2023-51467**: Authentication bypass leading to potential code execution

These vulnerabilities affect unpatched versions of Apache OFBiz and demonstrate the critical importance of maintaining up-to-date software, especially for internet-facing business applications.

Initial Foothold and Code Execution
---
Exploiting the OFBiz authentication bypass provides initial code execution on the target system. This demonstrates how a single vulnerability in a web application can lead to complete system compromise. Organizations running Apache OFBiz should prioritize patching these CVEs and implementing web application firewalls as compensating controls.

Establishing a Reverse Shell
---
After achieving initial code execution, establishing a proper reverse shell provides interactive access to the compromised system. This enables more efficient enumeration and lateral movement within the environment.

Post-Exploitation Enumeration
---
Comprehensive enumeration using automated tools like LinPEAS helps identify privilege escalation vectors, sensitive files, and misconfigurations. Key areas of focus include:

- Writable executables and scripts
- SUID/SGID binaries
- Scheduled tasks and cron jobs
- Configuration files containing credentials
- Database files and connection strings
- Internal services and ports

Internal Service Discovery
---
During enumeration, internal web ports and services are discovered that are not accessible from external networks. These internal services often have weaker security controls and may contain additional vulnerabilities or sensitive information. Understanding the full scope of services running on a compromised system is essential for complete assessment.

Apache Derby Database Analysis
---
Apache OFBiz uses Apache Derby as its embedded database. Derby stores application data including user credentials, business records, and configuration information. Locating and analyzing Derby database files requires understanding:

- Derby file structure and storage locations
- Database schema and table relationships
- Methods for extracting data from Derby databases
- Password storage mechanisms and hashing algorithms

Password Hash Recovery
---
Through careful enumeration of Derby database files and application data directories, password hashes are discovered. These hashes represent encrypted credentials used by the application. The hash recovery process involves:

- Identifying the hashing algorithm used
- Extracting hash values from database or configuration files
- Using hash cracking tools to recover plaintext passwords
- Leveraging hashlib or similar libraries to verify hash formats

This phase emphasizes the importance of using strong, modern hashing algorithms with proper salting and key derivation functions.

Privilege Escalation
---
The recovered credentials or identified system misconfigurations provide a path to root access. This final escalation demonstrates how multiple vulnerabilities and misconfigurations can be chained together for complete system compromise.

Security Recommendations
---
Organizations using Apache OFBiz or similar enterprise applications should:

- Immediately apply security patches for CVE-2023-49070 and CVE-2023-51467
- Implement network segmentation to isolate business-critical applications
- Use strong, unique passwords with modern hashing algorithms
- Regularly audit application and database configurations
- Monitor for unusual authentication attempts and database access patterns
- Implement defense-in-depth security controls

