---
layout: post
title: "Pentest Primer: Are Hackers Targeting You? Analyze SSH Logs Like a Pro!"
date: 2025-03-12
---

Discover how to transform raw log data into actionable security insights using powerful Linux command-line tools. This lesson demonstrates how to identify potential cyber threats by analyzing SSH log files for suspicious activities and failed login attempts.

SSH (Secure Shell) is one of the most common attack vectors for systems exposed to the internet. Understanding how to analyze SSH logs effectively is crucial for detecting brute-force attacks, password spraying, and other malicious authentication attempts.

{% include youtube.html id="C406yf-AVaE" %}

Why SSH Log Analysis Matters
---
SSH logs contain valuable information about who is attempting to access your systems, whether those attempts succeed or fail, and patterns that might indicate malicious activity.

**Common Attack Patterns in SSH Logs**:

- **Brute Force Attacks**: Repeated failed login attempts from the same IP address
- **Password Spraying**: Testing common passwords across multiple accounts
- **Account Enumeration**: Attempts to identify valid usernames
- **Successful Compromises**: Successful logins from unexpected locations
- **Lateral Movement**: SSH connections from compromised internal systems

**Benefits of Log Analysis**:

- Early detection of attack attempts
- Identification of compromised accounts
- Understanding attack patterns and methods
- Evidence collection for incident response
- Proactive security hardening based on observed threats

Understanding SSH Authentication Logs
---
SSH authentication activities are typically logged to system authentication logs:

- **Linux**: `/var/log/auth.log` (Debian/Ubuntu) or `/var/log/secure` (RedHat/CentOS)
- **Log Content**: Username, source IP, authentication method, success/failure status, timestamp

**Common Log Entries**:

- `Failed password for invalid user`: Attempt to authenticate as non-existent user
- `Failed password for [username]`: Failed authentication for valid user
- `Accepted password for [username]`: Successful password authentication
- `Accepted publickey for [username]`: Successful key-based authentication
- `Connection closed by`: Connection terminated

Essential Commands for Log Analysis
---
Analyzing logs effectively requires combining several Linux commands to filter, extract, and analyze data.

**cat (Concatenate)**: Display file contents
- Usage: `cat /var/log/auth.log`
- Displays the entire log file
- Often used as first step in command pipelines
- Can combine multiple log files: `cat /var/log/auth.log /var/log/auth.log.1`

**awk (Text Processing)**: Extract and manipulate structured data
- Usage: `awk '{print $1, $2}' file`
- Powerful for field-based extraction
- Can filter based on conditions
- Performs calculations and transformations
- Example: `awk '/Failed password/ {print $1, $2, $3, $11}' /var/log/auth.log`

**grep (Pattern Matching)**: Filter lines matching patterns
- Usage: `grep "pattern" file`
- Find specific events in logs
- Case-insensitive with `-i` option
- Invert match with `-v` option
- Example: `grep "Failed password" /var/log/auth.log`

**cut (Field Extraction)**: Extract specific fields from delimited text
- Usage: `cut -d' ' -f1-3 file`
- Useful for extracting specific columns
- Works with various delimiters
- Example: `cut -d' ' -f1-3,9,11` extracts timestamp and key info

**sed (Stream Editor)**: Text transformation and substitution
- Usage: `sed 's/old/new/g' file`
- Replace patterns in text
- Delete specific lines or patterns
- Extract specific portions of text
- Example: `sed 's/invalid user //g'` removes "invalid user" text

**tr (Translate)**: Character-level transformations
- Usage: `tr 'a-z' 'A-Z'`
- Convert case
- Delete specific characters
- Squeeze repeated characters
- Example: `tr -d ':'` removes colons

Analyzing Failed Login Attempts
---
Failed login attempts are often the first indicator of attack activity.

**Finding All Failed Password Attempts**:
```bash
grep "Failed password" /var/log/auth.log
```

This shows all failed authentication attempts in the log.

**Extracting IP Addresses from Failed Attempts**:
```bash
grep "Failed password" /var/log/auth.log | awk '{print $11}'
```

This extracts just the IP addresses attempting failed logins.

**Counting Failed Attempts by IP**:
```bash
grep "Failed password" /var/log/auth.log | awk '{print $11}' | sort | uniq -c | sort -nr
```

This command:
1. Finds failed password attempts
2. Extracts IP addresses
3. Sorts them
4. Counts occurrences
5. Sorts by count (most attempts first)

**Finding Targeted Usernames**:
```bash
grep "Failed password for" /var/log/auth.log | awk '{print $9}' | sort | uniq -c | sort -nr
```

Shows which usernames attackers are targeting most frequently.

Detecting Password Spraying Attacks
---
Password spraying involves testing a small number of common passwords against many user accounts to avoid triggering account lockouts.

**Identifying Password Spraying Patterns**:
```bash
grep "Failed password" /var/log/auth.log | awk '{print $1,$2,$3,$9,$11}' | sort -k5
```

This shows failed attempts sorted by source IP, helping identify IPs targeting multiple accounts.

**Analyzing Attack Timeline**:
```bash
grep "Failed password" /var/log/auth.log | awk '{print $1,$2,$3}' | uniq -c
```

Shows temporal patterns of attack attempts, revealing coordinated attack windows.

**Finding IPs Targeting Multiple Accounts**:
```bash
awk '/Failed password/ {print $9,$11}' /var/log/auth.log | sort | uniq | cut -d' ' -f2 | sort | uniq -c | sort -nr
```

Identifies source IPs attempting authentication against multiple different usernames - a clear indicator of password spraying.

Detecting Brute Force Attacks
---
Brute force attacks involve repeated attempts against specific accounts.

**Counting Failed Attempts Per User**:
```bash
grep "Failed password for" /var/log/auth.log | awk '{print $9}' | sort | uniq -c | sort -nr | head -20
```

Shows the top 20 most-targeted usernames.

**Finding High-Volume Attack Sources**:
```bash
grep "Failed password" /var/log/auth.log | awk '{print $11}' | sort | uniq -c | sort -nr | head -20
```

Lists IP addresses with most failed attempts.

**Analyzing Specific Attacker**:
```bash
grep "Failed password" /var/log/auth.log | grep "192.168.1.100" | wc -l
```

Counts total failed attempts from specific IP address.

Advanced Log Analysis Techniques
---
**Extracting Clean IP Lists for Blocking**:
```bash
grep "Failed password" /var/log/auth.log | awk '{print $11}' | sort | uniq -c | sort -nr | awk '$1 > 10 {print $2}' > ips_to_block.txt
```

Creates a list of IPs with more than 10 failed attempts for adding to firewall block lists.

**Removing Unnecessary Text for Cleaner Output**:
```bash
grep "Failed password" /var/log/auth.log | sed 's/invalid user //g' | sed 's/ from//g'
```

Uses sed to remove repetitive text for more readable output.

**Time-Based Analysis**:
```bash
grep "Failed password" /var/log/auth.log | awk '{print $1,$2,$3}' | cut -d':' -f1 | uniq -c
```

Groups failed attempts by hour to identify attack patterns throughout the day.

Saving Analysis Results
---
Analysis results should be saved for documentation and further investigation.

**Saving to File**:
```bash
grep "Failed password" /var/log/auth.log | awk '{print $11}' | sort | uniq -c | sort -nr > failed_login_summary.txt
```

The `>` operator redirects output to a file (overwrites existing file).

**Appending to File**:
```bash
grep "Failed password" /var/log/auth.log | awk '{print $11}' | sort | uniq -c | sort -nr >> security_analysis.txt
```

The `>>` operator appends output to existing file.

Identifying Successful Compromises
---
While failed attempts are concerning, successful logins from unexpected sources are critical.

**Finding Successful Logins**:
```bash
grep "Accepted password" /var/log/auth.log
```

Shows all successful password-based authentications.

**Identifying Unusual Successful Logins**:
```bash
grep "Accepted password" /var/log/auth.log | awk '{print $9,$11}'
```

Shows username and source IP for successful logins to identify unexpected access.

**Comparing with Known Good IPs**:
Create a list of legitimate IP addresses and compare successful logins against this whitelist to identify potentially compromised accounts.

Practical Security Applications
---
**For Defenders**: Use these techniques to:
- Monitor for ongoing attacks in real-time
- Generate lists of malicious IPs for firewall rules
- Identify compromised accounts
- Document security incidents
- Support incident response investigations

**For Penetration Testers**: Understand these techniques to:
- Evade detection during authorized testing
- Clean up traces after testing (with permission)
- Demonstrate impact of successful attacks
- Recommend defensive monitoring improvements
- Test whether defensive logging is working correctly

Automating Log Analysis
---
Manual log analysis works for ad-hoc investigations, but consider automation for ongoing monitoring:

**Bash Scripts**: Create scripts that run these analyses periodically and alert on suspicious patterns

**Cron Jobs**: Schedule regular log analysis and reporting

**Log Management Systems**: Tools like ELK Stack, Splunk, or Graylog provide powerful log aggregation and analysis

**SIEM Solutions**: Security Information and Event Management systems automate threat detection

Best Practices for SSH Security
---
Beyond log analysis, implement these security measures:

- **Disable Password Authentication**: Use key-based authentication only
- **Implement Fail2ban**: Automatically block IPs with repeated failed attempts
- **Change Default SSH Port**: Reduce automated scan attempts (security through obscurity, not primary defense)
- **Use Strong Passwords**: If password auth must be enabled, enforce strong password policies
- **Implement Account Lockouts**: Limit failed attempt tolerance
- **Restrict SSH Access**: Use firewall rules to limit source IPs
- **Enable Two-Factor Authentication**: Add additional authentication layer
- **Regular Log Review**: Make log analysis part of routine security operations
- **Keep SSH Updated**: Apply security patches promptly

Practice Resources
---
To practice SSH log analysis:

- Download sample auth.log file from: https://github.com/elastic/examples/blob/master/Machine%20Learning/Security%20Analytics%20Recipes/suspicious_login_activity/data/auth.log
- Set up honeypot SSH servers to collect real attack data
- Use your own systems' logs (with appropriate privacy considerations)
- Join cybersecurity training platforms that provide log analysis challenges

Conclusion
---
SSH log analysis is a fundamental defensive skill that combines Linux command-line proficiency with security awareness. The techniques covered in this lesson transform raw log data into actionable intelligence for detecting and responding to threats.

Mastering these analysis techniques provides both defensive capabilities for protecting systems and offensive understanding for penetration testing. Whether defending networks or testing their security, SSH log analysis remains an essential skill in the cybersecurity toolkit.
