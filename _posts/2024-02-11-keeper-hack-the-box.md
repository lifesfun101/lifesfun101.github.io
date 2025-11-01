---
layout: post
title: "Keeper - Hack The Box"
date: 2024-02-11
---

This walkthrough covers Keeper, an easy-level machine from Hack The Box that demonstrates the exploitation of default credentials, a recent KeePass vulnerability (CVE-2023-32784), and PuTTY key-based privilege escalation.

Keeper provides an excellent learning opportunity for understanding how default credentials, password manager vulnerabilities, and SSH key misconfigurations can be combined to compromise a system. This machine emphasizes the importance of changing default credentials and staying updated on recent security vulnerabilities.

{% include youtube.html id="b7B4b2jotiw" %}

Web Application Enumeration
---
The initial phase involves enumerating a web application to identify the underlying technology. Proper enumeration reveals that the application is running Request Tracker, a popular open-source ticket tracking system. Understanding the software in use is crucial for identifying potential security weaknesses and default configurations.

Request Tracker and Default Credentials
---
Request Tracker is an enterprise-grade issue tracking system commonly used by IT departments and help desks. Like many web applications, Request Tracker ships with default administrative credentials that should be changed during installation. Unfortunately, administrators sometimes overlook this basic security practice, leaving systems vulnerable to unauthorized access.

Default credentials represent one of the most common and easily preventable security vulnerabilities. Organizations should:
- Change all default passwords immediately after installation
- Use strong, unique passwords for administrative accounts
- Implement password policies that prevent the use of common or default passwords
- Regularly audit systems for default credentials

Initial Foothold via SSH
---
After obtaining credentials from the Request Tracker application, these credentials are successfully reused to gain SSH access to the system. This demonstrates the danger of password reuse and the importance of implementing unique credentials for different services and privilege levels.

KeePass Password Manager Vulnerability
---
Upon gaining initial access, a suspicious zip file is discovered on the system. This archive contains a KeePass database dump file, which leads to a critical discovery. KeePass is a widely-used, free, and open-source password manager that stores credentials in an encrypted database protected by a master password.

CVE-2023-32784: KeePass Master Password Recovery
---
In May 2023, a serious vulnerability was discovered in KeePass that allows attackers to recover the clear text master password from a memory dump. This vulnerability affects KeePass versions prior to 2.54 and demonstrates that even security-focused applications can have critical flaws.

The vulnerability works by extracting remnants of the master password from process memory. Even though KeePass attempts to protect the master password in memory, the way it handles password entry character-by-character leaves traces that can be reconstructed. This highlights the challenges of securely handling sensitive data in memory and the importance of timely patching.

Privilege Escalation via PuTTY Keys
---
After successfully recovering the master password from the KeePass dump, the password database reveals PuTTY private key files. PuTTY is a popular SSH client for Windows that uses its own proprietary format (.ppk) for storing SSH keys.

The privilege escalation involves converting the PuTTY format key to OpenSSH format and using it to authenticate as a privileged user. This demonstrates:
- How SSH keys can be as sensitive as passwords
- The importance of protecting private keys with proper permissions
- How credential stores can become single points of failure if compromised

Key Takeaways
---
This machine reinforces several important security concepts:
- Always change default credentials on all systems and applications
- Keep password managers and security tools updated with the latest patches
- Protect SSH keys with the same rigor as passwords
- Be aware that memory dumps can expose sensitive information
- Implement defense-in-depth strategies rather than relying on single security controls

