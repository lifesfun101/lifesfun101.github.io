---
layout: post
title: "Sau - Hack The Box"
date: 2023-12-15
---

{% include youtube.html id="pQBR_WEkEFY" %}

This walkthrough covers Sau, a Hack The Box machine that demonstrates Server-Side Request Forgery (SSRF) vulnerabilities and privilege escalation techniques.

Sau provides an excellent learning opportunity for understanding how SSRF vulnerabilities can be exploited to access internal services and achieve remote code execution. This machine emphasizes the importance of proper input validation and network segmentation in web applications.

Web Application Enumeration
---
The initial phase involves comprehensive enumeration of the web application to identify potential attack vectors. Understanding the application's functionality and architecture is essential for discovering the SSRF vulnerability that serves as the entry point.

Server-Side Request Forgery (SSRF)
---
The primary vulnerability in this machine is an SSRF flaw that allows attackers to make the server perform requests on their behalf. SSRF vulnerabilities can be particularly dangerous as they enable access to internal services that are not directly exposed to the internet. By leveraging this vulnerability, it's possible to interact with services running on localhost or internal network segments.

What is SSRF?
---
Server-Side Request Forgery occurs when a web application fetches a remote resource without properly validating user-supplied URLs. Attackers can abuse this functionality to:

- Access internal services and APIs that should not be publicly accessible
- Bypass firewall rules and access controls
- Interact with cloud metadata services to retrieve sensitive information
- Perform port scanning of internal networks
- Chain with other vulnerabilities for greater impact

Exploitation Chain
---
The exploitation process involves identifying the SSRF vulnerability, using it to access a second internal web application, and then exploiting that application to gain initial foothold on the system. This demonstrates how multiple vulnerabilities can be chained together to achieve code execution.

Privilege Escalation
---
Once initial access is obtained, the privilege escalation path is relatively straightforward, demonstrating common Linux misconfigurations that can lead to root access. Understanding these privilege escalation techniques is essential for both offensive and defensive security practitioners.

This machine serves as a practical introduction to SSRF vulnerabilities and reinforces the importance of proper input validation, network segmentation, and least privilege principles in web application security.

