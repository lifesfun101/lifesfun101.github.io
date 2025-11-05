---
title: "GCP API Key Tester: Assessing Security Impact of Leaked Google Cloud API Keys"
date: 2025-11-03 19:30:00 -0500
categories: [Tools, Security]
tags: [gcp, api-security, cloud-security, bash, security-tools, google-cloud, pentesting]
---

## Introduction

Google Cloud Platform (GCP) API keys could be exposed in a varity of ways: through public GitHub repositories, client-side JavaScript code, CI/CD logs, and configuration files. When an API key leaks, organizations face potential cost implications from unauthorized API usage and risk exposure of data accessible through enabled services.

The **GCP API Key Tester** is a bash-based security assessment tool designed to evaluate the impact of leaked API keys. It systematically tests a compromised key against GCP APIs, generating impact reports to help organizations understand their exposure.

This tool is intended for authorized security testing, penetration testing engagements, and demonstrating the severity of API key leaks.

## The Problem: How API Keys Leak

API keys commonly leak through several vectors:

- **Source Code Commits**: Keys accidentally committed to public GitHub, GitLab, or Bitbucket repositories
- **Client-Side Code**: API keys embedded in JavaScript, mobile apps, or browser extensions
- **CI/CD Logs**: Build logs containing environment variables or configuration details
- **Documentation**: API keys left in technical documentation, wikis, or README files
- **Network Traffic**: Keys transmitted over unencrypted connections or captured in browser developer tools

The consequences of leaked API keys include:
- **Cost Impact**: Attackers can exhaust API quotas and rack up significant charges
- **Data Exposure**: Access to Cloud Storage buckets, Firebase databases, etc
- **Service Disruption**: Quota exhaustion causing legitimate services to fail
- **Compliance Risk**: Unauthorized access potentially violating data protection regulations

## Tool Overview

The GCP API Key Tester provides a systematic approach to impact assessment:

**Key Features**:
- Tests **29 Google APIs** across **8 logical categories**
- Non-invasive, read-only testing methodology
- Generates timestamped security impact reports
- Color-coded category headers for clear visual feedback
- Calculates approximate cost impact per API
- Identifies specific restriction types (IP, referrer, API disabled)
- Provides detailed remediation recommendations

**The 8 Categories Tested**:
1. **Google Maps Platform** (8 APIs) - Geocoding, Directions, Places, Static Maps, Distance Matrix, Time Zone, Roads
2. **Google Cloud AI/ML** (7 APIs) - Translation, Vision, Natural Language, Speech-to-Text, Text-to-Speech, Video Intelligence, Gemini
3. **Workspace - LIMITED RISK** (3 APIs) - Drive, Sheets, Calendar (public resources only)
4. **YouTube & Media** (1 API) - YouTube Data API
5. **Firebase & Databases** (2 APIs) - Firebase Realtime Database, Firebase Installations
6. **Google Cloud Infrastructure** (1 API) - Cloud Storage
7. **Search & Discovery** (5 APIs) - Books, Custom Search, PageSpeed Insights, Fonts, Safe Browsing
8. **Other Services** (2 APIs) - Blogger, Civic Information

**Important Note on Workspace APIs**: The tool tests Google Drive, Sheets, and Calendar APIs, which only access **public resources** with API keys. While this limits immediate data exposure risk, this can still prove that the API key is valid.

## Installation & Usage

### Prerequisites

```bash
# Required
curl  # Usually pre-installed on most *nix systems

# Optional (for cost calculations)
bc
```

### Installation

```bash
# Clone the repository
git clone https://github.com/lifesfun101/gcp-api-key-tester.git
cd gcp-api-key-tester

# Make the script executable
chmod +x gcp-api-key-tester.sh
```

### Basic Usage

```bash
./gcp-api-key-tester.sh AIzaSy...your_api_key_here
```

### Example Output

When executed, the tool provides color-coded feedback organized by category:

```
════════════════════════════════════════════════════════════════
GCP API Key Impact Assessment Tool
════════════════════════════════════════════════════════════════

[i] Testing API Key: AIzaSyD...xyz12
[i] Report will be saved to: gcp_api_key_assessment_20251103_143022.txt

════════════════════════════════════════════════════════════════
CATEGORY 1: GOOGLE MAPS PLATFORM (8 APIs)
════════════════════════════════════════════════════════════════

[*] Testing: Google Maps Geocoding API
[✓] VULNERABLE: Google Maps Geocoding API is accessible!

[*] Testing: Google Maps Reverse Geocoding
[✗] Protected: Google Maps Reverse Geocoding not accessible
    → IP restriction in place

...
```

## Understanding the Output

The tool uses color-coding to indicate test results:

### ✅ Accessible (Green)

```
[✓] VULNERABLE: Google Maps Geocoding API is accessible!
```

**Meaning**: The API key can access this service
**Action Required**: High priority - revoke and rotate the key immediately
**Implication**: Attackers can use this API, potentially incurring costs or accessing data

### ❌ Protected (Yellow)

```
[✗] Protected: Gmail API not accessible
    → API key invalid for this service
```

**Meaning**: The API key cannot access this service (good security posture)

**Possible Reasons**:
- API not enabled for this key's project
- IP address restrictions in place
- HTTP referrer restrictions configured
- Service requires OAuth instead of simple API key
- API disabled in the GCP project

The tool identifies and reports the specific restriction type encountered, helping you understand your security configuration.

## Report Generation

Each test run generates a detailed assessment report with a timestamp: `gcp_api_key_assessment_YYYYMMDD_HHMMSS.txt`

**Report Contents**:

1. **Executive Summary**
2. **API Access Results**
3. **Cost Calculations**
4. **Impact Assessment**
5. **Recommendations**
6. **Technical Details**

## Remediation: Securing Your API Keys

Based on [Google Cloud API Key restriction documentation](https://docs.cloud.google.com/api-keys/docs/add-restrictions-api-keys), here's how to properly secure API keys:

### Immediate Actions When a Key is Compromised

1. **Revoke the Key Immediately**
   - Navigate to [Google Cloud Console > APIs & Credentials](https://console.cloud.google.com/apis/credentials)
   - Delete the compromised key

2. **Review Audit Logs**
   - Check Cloud Audit Logs for unauthorized usage
   - Look for unusual API call patterns
   - Identify accessed resources

3. **Generate New Key with Restrictions**
   - Create a replacement key
   - Apply appropriate restrictions before use
   - Update applications with the new key

### Application Restrictions

API keys support four types of **client restrictions** (choose one per key):

#### HTTP Referrers (Website Restrictions)

Limits usage to specific websites:

```
# Allow main domain and subdomains
www.example.com
*.example.com/*

# Restrict to specific paths
www.example.com/path
```

**Best for**: Browser-based applications, client-side JavaScript

#### IP Addresses (Server Restrictions)

Permits calls only from specified server IP addresses:

```
# Single IPv4 address
198.51.100.1

# IPv4 subnet (CIDR notation)
198.51.100.0/24

# IPv6 address
2001:db8::1
```

**Best for**: Server-side applications, cron jobs, backend services

#### Android App Restrictions

Requires package name and SHA-1 certificate fingerprint:

```
Package name: com.example.app
SHA-1 fingerprint: DA:39:A3:EE:5E:6B:4B:0D:32:55:BF:EF:95:60:18:90:AF:D8:07:09
```

**Best for**: Android mobile applications

#### iOS App Restrictions

Requires bundle IDs:

```
com.example.iosapp
com.example.anotherapp
```

**Best for**: iOS mobile applications

### API Restrictions

Limit which Google Cloud APIs can be accessed:

- **Restrict to specific APIs**: Only allow the APIs your application needs
- **Method-level restrictions**: Use wildcards like `Get*` or specify exact method names
- **Principle of least privilege**: Enable only the minimum required APIs

**Example**: If your application only uses Maps Geocoding API, restrict the key to only that service rather than all Maps APIs.

### Defense-in-Depth Approach

Google recommends applying **both** client and API restrictions:

```
✅ Client Restriction: iOS or Android App
AND
✅ API Restriction: Maps Geocoding API only
```

This layered approach ensures that even if one restriction is bypassed, the key remains limited in scope.

### Best Practices

- **Never commit API keys to source control**: Use environment variables or secret management systems
- **Rotate keys regularly**: Implement a key rotation policy (e.g., every 90 days)
- **Monitor API usage**: Set up billing alerts and quota monitoring
- **Use different keys for different environments**: Separate development, staging, and production keys
- **Consider service accounts**: For server-side applications, OAuth 2.0 service accounts provide better security
- **Enable Cloud Audit Logs**: Track all API key usage for security monitoring

## Important Notes & Limitations

### Understanding False Positives

An API showing as "accessible" doesn't always indicate a critical data breach:

- Some APIs are read-only by design (PageSpeed Insights, Fonts API)
- Authorization may still be required for sensitive operations
- Many APIs provide public data only
- API keys have different permission levels depending on project configuration

### OAuth vs API Keys

The GCP API Key Tester focuses on API key testing, but many Google services prefer OAuth:

- **Gmail API**: Typically requires OAuth 2.0, not just API keys
- **Google Drive/Sheets/Calendar**: Only access **public** resources with API keys; OAuth required for private data
- **YouTube Data API**: Some operations require OAuth for private video access

### Tool Limitations

- **Project IDs Required**: Firebase tests are more effective with known project IDs
- **Disabled APIs**: Tests show "not accessible" if the API is disabled in the GCP project
- **Rate Limits**: Aggressive testing may hit quota limits and trigger warnings
- **Public Resources Only**: Workspace APIs (Drive, Sheets, Calendar) only access public data with API keys

### Non-Invasive Testing

The tool performs **read-only, non-invasive** testing:
- No data is modified or deleted
- Script includes 1-second delays to respect rate limits
- Tests use minimal API calls
- All operations are logged for transparency

## Ethical Use & Disclaimer

### Authorized Use Only

This tool is designed for:

✅ Testing your own API keys
✅ Authorized penetration testing engagements
✅ Security assessments with explicit permission
✅ Educational demonstrations in controlled environments
✅ Incident response and impact analysis

### Before Testing

1. **Get Authorization**: Only test keys you own or have explicit permission to test
2. **Document Everything**: Keep records of your assessment and findings
3. **Inform Stakeholders**: Alert relevant parties before testing production keys
4. **Follow Scope**: Respect the boundaries of your engagement or authorization

### Legal Considerations

- Unauthorized testing may violate terms of service
- Accessing systems without permission may violate computer fraud laws
- Always operate within legal and ethical boundaries
- Use for defensive security, authorized pentesting, or research only

**The authors are not responsible for misuse of this tool. Use responsibly.**

## Conclusion

The GCP API Key Tester provides security professionals with a systematic method to assess the impact of leaked Google Cloud API keys. By testing 29 APIs across 8 categories and generating detailed reports, the tool helps organizations understand their exposure and take appropriate remediation steps.

**Key Takeaways**:

- Leaked API keys can provide access to costly services and potentially expose public data
- Proper API key restrictions (both client and API-level) significantly reduce risk
- Regular key rotation and monitoring are essential security practices
- Not all "accessible" APIs represent critical security issues - context matters
- OAuth 2.0 service accounts provide better security than API keys for server-side applications

### Resources

- **GitHub Repository**: [gcp-api-key-tester](https://github.com/lifesfun101/gcp-api-key-tester)
- **Google Cloud API Key Restrictions**: [Official Documentation](https://docs.cloud.google.com/api-keys/docs/add-restrictions-api-keys)
- **GCP Security Best Practices**: [Google Cloud Security](https://cloud.google.com/security)

If you find this tool useful for your security assessments, consider starring the repository on GitHub. Contributions, bug reports, and feedback are welcome!

**Remember**: Secure your API keys, apply restrictions, and monitor usage. Your future self will thank you.
