---
layout: post
title: "Manager - Hack The Box"
date: 2024-03-28
---

{% include youtube.html id="8cTPzczgNa8" %}

This walkthrough covers Manager, a Hack The Box machine that demonstrates Active Directory enumeration, password spraying attacks, and exploitation of Active Directory Certificate Services using the ESC7 privilege escalation technique.

Manager provides comprehensive experience with Windows Active Directory penetration testing, from initial reconnaissance through privilege escalation. This machine emphasizes the importance of thorough enumeration and understanding certificate-based authentication vulnerabilities in enterprise environments.

Port Scanning and Web Enumeration
---
The initial reconnaissance begins with comprehensive port scanning to identify available services. Port 80 enumeration reveals a web application that provides initial information about the target environment. Understanding the services exposed by a domain controller is crucial for planning the attack path.

SMB and RPC Enumeration
---
SMB (Server Message Block) and RPC (Remote Procedure Call) are fundamental Windows protocols that often reveal valuable information about Active Directory environments. Enumeration of these services can disclose:

- Domain name and structure
- User account information
- Share permissions and accessible resources
- Group memberships and policies
- Domain controller details

Proper enumeration of SMB and RPC services is essential for understanding the Active Directory landscape and identifying potential attack vectors.

LDAP Enumeration
---
Lightweight Directory Access Protocol (LDAP) is the core protocol used by Active Directory for directory services. Using tools like ldapsearch, attackers can query the directory to extract detailed information about:

- User accounts and attributes
- Computer objects
- Group memberships
- Organizational structure
- Service accounts and SPNs (Service Principal Names)

LDAP enumeration often provides the foundation for more targeted attacks against Active Directory environments.

Username Enumeration with Kerbrute
---
Kerbrute is a tool that leverages Kerberos pre-authentication to enumerate valid usernames in an Active Directory environment. This technique is particularly useful because:

- It doesn't require authentication
- It generates minimal logs compared to other enumeration methods
- It's fast and efficient for validating username lists
- It can help identify naming conventions used in the organization

Once valid usernames are identified, they can be used in password spraying or other authentication attacks.

Password Spraying
---
Password spraying is a technique where a small number of commonly used passwords are tried against many user accounts. This approach is preferred over traditional brute force because:

- It avoids account lockouts by staying below the lockout threshold
- It targets common weak passwords across the organization
- It's more likely to succeed in environments with weak password policies

Password spraying demonstrates why organizations need strong password policies and account lockout mechanisms that consider both per-account and organization-wide attack patterns.

Initial Foothold
---
After successful password spraying, valid credentials provide initial access to the Active Directory environment. This foothold enables further enumeration of the domain and identification of privilege escalation paths.

Active Directory Certificate Services Exploitation
---
The privilege escalation path involves exploiting misconfigured Active Directory Certificate Services (AD CS). AD CS is a Windows Server role that provides public key infrastructure (PKI) functionality, including certificate issuance and management.

ESC7 Privilege Escalation
---
ESC7 is one of several Active Directory Certificate Services escalation techniques documented by security researchers. This technique exploits misconfigurations in certificate authority permissions and certificate template settings. The ESC7 attack chain typically involves:

- Identifying vulnerable certificate authority configurations
- Abusing CA permissions to escalate privileges
- Requesting certificates with elevated privileges
- Using these certificates for authentication as privileged users

Tools like Certipy automate the discovery and exploitation of AD CS vulnerabilities, making these attacks accessible to penetration testers and threat actors alike.

Troubleshooting and Clock Skew
---
Active Directory environments rely heavily on time synchronization for Kerberos authentication. Clock skew issues occur when the time difference between client and server exceeds the allowed threshold (typically 5 minutes). Understanding how to identify and resolve clock skew problems is essential for successful Active Directory exploitation.

Key Takeaways
---
This machine reinforces important Active Directory security concepts:

- Implement strong password policies to prevent password spraying
- Regularly audit Active Directory Certificate Services configurations
- Monitor for unusual certificate requests and CA permission changes
- Maintain proper time synchronization across the domain
- Apply principle of least privilege to certificate templates and CA permissions
- Implement network segmentation to limit lateral movement

