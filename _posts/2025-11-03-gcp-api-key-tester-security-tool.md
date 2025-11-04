---
title: "GCP API Key Tester: Assessing the Impact of Leaked Google Cloud API Keys"
date: 2025-11-03 16:30:00 -0500
categories: [Tools, Security]
tags: [gcp, api-security, cloud-security, pentest, security-tools, bash, google-cloud]
---

## Overview

I recently developed a security tool to demonstrate the potential impact of leaked Google Cloud Platform (GCP) API keys. The **GCP API Key Tester** is a bash script that systematically tests a compromised API key against 39 different Google APIs to assess what an attacker could access or abuse.

This tool is designed for security assessments, penetration testing, and demonstrating the importance of proper API key management in cloud environments.

## The Problem: Leaked API Keys

API keys are often accidentally exposed through:
- **Source code commits** in public repositories (GitHub, GitLab)
- **Client-side code** (JavaScript, mobile apps)
- **CI/CD logs** and build artifacts
- **Documentation** and configuration files
- **Browser developer tools** and network traffic

A single leaked GCP API key can potentially provide access to:
- Maps and geocoding services (cost implications)
- Cloud storage buckets (data exposure)
- Email and contacts via Gmail/People APIs (CRITICAL)
- Firebase databases and configurations
- AI services like Gemini (expensive abuse)
- Analytics and search console data

## What the Tool Does

The GCP API Key Tester automates the assessment process by:

1. **Validating** the API key format (AIza... pattern)
2. **Testing** 39 different Google APIs systematically
3. **Categorizing** results by accessibility and restrictions
4. **Calculating** potential cost impact and severity
5. **Generating** a detailed assessment report
6. **Providing** remediation recommendations

### APIs Tested

The tool tests across multiple categories:

**Google Maps Platform** (8 APIs)
- Geocoding API (forward and reverse)
- Static Maps API
- Distance Matrix API
- Time Zone API
- Roads API

**AI & Machine Learning**
- Cloud Speech-to-Text
- Cloud Text-to-Speech
- Gemini API (Vertex AI)
- Cloud Video Intelligence

**Data Access** (HIGH RISK)
- Gmail API
- Google Drive API
- Google Sheets API
- Google Calendar API
- People API (Contacts)
- Google Photos Library API

**Cloud Infrastructure**
- Cloud Storage (buckets)
- Cloud Firestore
- Firebase Realtime Database
- Firebase Remote Config
- Cloud Functions
- Cloud Build
- Cloud Pub/Sub

**Search & Analytics**
- Custom Search API
- PageSpeed Insights
- Google Analytics Reporting
- Search Console API
- Safe Browsing API

**And more** - 39 APIs total

## Installation & Usage

### Prerequisites

```bash
# Required: curl (usually pre-installed)
# Optional: bc (for cost calculations)
```

### Installation

```bash
# Clone the repository
git clone https://github.com/lifesfun101/gcp-api-key-tester.git
cd gcp-api-key-tester

# Make executable
chmod +x gcp-api-key-tester.sh
```

### Basic Usage

```bash
./gcp-api-key-tester.sh <API_KEY>
```

### Example

```bash
./gcp-api-key-tester.sh AIzaSyD-example-key-9qBGGQ
```

## Understanding the Output

The tool provides color-coded feedback:

- **GREEN ✓**: API is accessible (VULNERABLE)
- **YELLOW ✗**: API is protected/restricted
- **RED**: High severity findings

### Sample Output

```
╔════════════════════════════════════════════════════════════════╗
║        GCP API Key Impact Assessment Tool                     ║
╚════════════════════════════════════════════════════════════════╝

[*] Testing: Google Maps Geocoding API
[✓] VULNERABLE: Google Maps Geocoding API is accessible!

[*] Testing: Gmail API v1
[✗] Protected: Gmail API not accessible
    → API not enabled for this key

Total APIs Tested: 39
Accessible APIs: 5
Severity: HIGH
```

### Generated Report

The tool creates a timestamped report file (`gcp_api_key_assessment_YYYYMMDD_HHMMSS.txt`) containing:

- Executive summary
- Detailed API access results
- Cost impact analysis
- Data exposure risks
- Remediation recommendations
- Technical details

## Security Impact Assessment

### Cost Impact

If an attacker gains access to APIs with per-request costs:
- **Gemini API**: $0.00025 per request → $250 per 1M requests
- **Video Intelligence**: $0.10 per minute → Very expensive for large-scale abuse
- **Speech-to-Text**: $0.006 per 15 seconds
- **Maps APIs**: $0.005 per request → Can rack up significant costs

### Data Exposure

The most critical APIs for data exposure:
- **Gmail API**: Access to all email content
- **People API**: Entire contact list, phone numbers, emails
- **Google Photos**: Private photo albums
- **Drive/Sheets**: Business documents and spreadsheets
- **Calendar**: Meeting information and schedules

### Compliance Risks

Unauthorized access to customer data via compromised API keys can lead to:
- GDPR violations
- Data breach notifications
- Reputation damage
- Legal liability

## Real-World Scenario

During a recent security assessment, I discovered a leaked API key in a public GitHub repository. Using this tool, I found:

- ✅ **5 APIs accessible** (Maps, Geocoding, PageSpeed, Fonts, Books)
- ❌ **34 APIs protected** (properly restricted)
- **Severity**: MEDIUM
- **Impact**: Potential for cost abuse via Maps APIs

**Recommendation**: Even though sensitive APIs were protected, the key was immediately revoked and rotated with proper restrictions applied.

## Mitigation Strategies

### Immediate Actions

If you discover a leaked API key:

1. **Revoke immediately** at [Google Cloud Console](https://console.cloud.google.com/apis/credentials)
2. **Review audit logs** for unauthorized usage
3. **Generate new key** with proper restrictions
4. **Check billing** for unexpected charges

### Long-Term Security

**API Key Restrictions** (Configure in GCP Console):
```
✓ IP address restrictions (whitelist known IPs)
✓ HTTP referrer restrictions (for web apps)
✓ API service restrictions (limit to required APIs only)
✓ Rate limiting and quotas
```

**Best Practices**:
- ❌ Never commit API keys to source control
- ✅ Use environment variables for credentials
- ✅ Implement key rotation policies
- ✅ Use Cloud Secret Manager for production
- ✅ Set up billing alerts and quotas
- ✅ Enable Cloud Audit Logs
- ✅ Monitor API usage for anomalies

**For Web Applications**:
```javascript
// BAD - Exposed in client-side code
const API_KEY = "AIzaSyD...";

// GOOD - Use backend proxy
fetch('/api/maps/geocode?address=...')
  .then(response => response.json());
```

## Tool Architecture

The script uses a modular approach:

```bash
test_api() {
    local api_name="$1"
    local curl_cmd="$2"
    local success_pattern="$3"
    local cost_per_request="$4"
    local description="$5"

    # Execute test, check response, log results
}
```

Each API test:
1. Executes curl request with timeout
2. Checks response for success patterns
3. Identifies specific restriction types
4. Logs detailed results to report
5. Updates statistics and severity

## Ethical Considerations

**Important**: This tool is for authorized security testing only:

✅ **Authorized Use**:
- Testing your own API keys
- Penetration testing with explicit permission
- Security assessments and audits
- Educational demonstrations

❌ **Unauthorized Use**:
- Testing keys without authorization
- Malicious API abuse
- Data exfiltration
- Cost exploitation

## Future Enhancements

Planned features:
- OAuth 2.0 service account testing
- Automated key enumeration from GitHub
- Integration with security scanning pipelines
- Cloud Functions deployment option
- JSON output format for automation

## Conclusion

The GCP API Key Tester demonstrates why proper API key management is critical in cloud security. A single leaked key can provide access to sensitive data, rack up significant costs, and create compliance risks.

**Key Takeaways**:
1. Always restrict API keys to minimum required services
2. Implement IP/referrer restrictions where possible
3. Use backend proxies instead of exposing keys client-side
4. Monitor API usage and set up billing alerts
5. Rotate keys regularly and immediately upon suspected compromise

## Resources

- **GitHub Repository**: [gcp-api-key-tester](https://github.com/lifesfun101/gcp-api-key-tester)
- **GCP API Key Best Practices**: [Google Cloud Documentation](https://cloud.google.com/docs/authentication/api-keys)
- **API Key Restrictions**: [Cloud Console Guide](https://cloud.google.com/docs/authentication/api-keys#adding_restrictions)

---

**Disclaimer**: This tool is provided for educational and authorized security testing purposes only. Always obtain proper authorization before testing API keys. The author is not responsible for misuse of this tool.

If you find this tool useful, consider giving it a star on GitHub! Contributions and feedback are welcome.
