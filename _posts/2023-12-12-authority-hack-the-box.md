---
layout: post
title: "Authority - Hack The Box"
date: 2023-12-12
---

This walkthrough covers Authority, a medium-level Windows machine from Hack The Box that involves SMB information disclosure, hash cracking, LDAP clear text credentials, and exploiting a configured Active Directory Certificate Authority.

Authority presents a realistic Active Directory environment that requires a combination of enumeration skills, credential harvesting, and knowledge of certificate-based authentication vulnerabilities. This machine is an excellent opportunity to practice Windows pentesting techniques and understand common misconfigurations in enterprise environments.

Initial Enumeration
---
The walkthrough begins with comprehensive service enumeration to identify potential attack vectors. Initial scanning reveals multiple services running on the target, with SMB being a key focus area. Proper enumeration of SMB shares and accessible resources is crucial for discovering the information disclosure vulnerability that leads to initial access.

SMB Information Disclosure
---
Through careful enumeration of SMB shares, sensitive files are discovered that contain valuable information. This phase demonstrates the importance of thoroughly investigating all accessible resources, as even seemingly innocuous files can contain credentials or configuration details that aid in further exploitation.

Credential Harvesting
---
The machine involves working with Ansible configuration files that contain password hashes. These hashes require cracking to obtain clear text credentials. Additionally, LDAP enumeration reveals clear text credentials stored in the directory service, highlighting common security misconfigurations in Active Directory environments.

Active Directory Certificate Services Exploitation
---
The privilege escalation path involves exploiting a misconfigured Active Directory Certificate Authority. Using tools like Certipy, the walkthrough demonstrates how to enumerate certificate templates, identify vulnerable configurations, and leverage certificate-based authentication to escalate privileges. This technique showcases a modern attack vector that is increasingly relevant in enterprise penetration testing.

Key Techniques Covered
---
- SMB share enumeration and file discovery
- Password hash extraction and cracking
- LDAP enumeration and credential extraction
- Responder for credential capture
- Active Directory Certificate Services exploitation
- Certificate-based authentication abuse

This machine provides valuable hands-on experience with Windows Active Directory environments and demonstrates the importance of proper security configurations for certificate services, credential storage, and file share permissions.

{% include youtube.html id="5xf1hZ2p_kg" %}
