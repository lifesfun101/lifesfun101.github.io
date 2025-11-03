---
title: "Visual - Hack The Box"
date: 2024-01-27 00:00:00 -0500
categories: [Pentesting, HackTheBox]
tags: [htb, hackthebox, enumeration, web-exploitation, privilege-escalation]
---

This walkthrough covers Visual, a Hack The Box machine that involves exploiting a .NET C# project build process, achieving code execution through Visual Studio build events, and performing privilege escalation using service exploitation and potato-based privilege escalation techniques.

Visual presents a unique attack scenario that demonstrates how development tools and build processes can be weaponized to achieve remote code execution. This machine provides valuable experience with Windows exploitation techniques and emphasizes the security implications of automated build systems.

{% include youtube.html id="V5lMuqxCTYw" %}

Web Application Enumeration
---
The initial reconnaissance phase involves enumerating a web application that appears to be related to software development or build automation. Understanding the application's purpose and functionality is crucial for identifying the attack vector that leads to initial access.

.NET Project Exploitation
---
The primary exploitation path involves creating a malicious .NET C# project that will be processed by the target system. This demonstrates how build automation systems can be dangerous when they process untrusted code or projects. By carefully crafting a C# project with malicious build events, it's possible to achieve code execution when the project is compiled.

Visual Studio Build Events
---
Build events in Visual Studio allow developers to run custom commands before, during, or after the build process. While this feature is useful for legitimate development workflows, it can be abused to execute arbitrary commands on the build server. This walkthrough demonstrates how pre-build or post-build events can be leveraged to establish a reverse shell connection.

Git Repository Setup
---
The exploitation chain requires setting up a Git repository containing the malicious project. Understanding how Git works over HTTP and how build systems fetch and process repositories is essential for successful exploitation. This phase highlights the importance of validating and sandboxing code from external sources.

Initial Foothold and Enumeration
---
After achieving initial access through the malicious build event, thorough enumeration of the Windows system is performed using tools like WinPEAS. This enumeration phase identifies potential privilege escalation vectors, including misconfigured services and exploitable Windows features.

Windows Privilege Escalation
---
The privilege escalation process involves two stages. The first escalation leverages Apache service misconfiguration, while the second employs potato-based privilege escalation techniques. Potato exploits take advantage of Windows token impersonation features and NTLM relay to escalate from a service account to SYSTEM privileges.

Understanding Potato Exploits
---
Potato-family exploits (including HotPotato, RottenPotato, JuicyPotato, and PetitPotato) represent a class of privilege escalation techniques that abuse Windows authentication and impersonation features. These techniques demonstrate the complexity of Windows privilege models and the importance of proper service account configuration.

This machine provides excellent hands-on experience with:
- .NET project structure and build processes
- Visual Studio build event abuse
- Git repository exploitation
- Windows service enumeration and exploitation
- Advanced Windows privilege escalation techniques

