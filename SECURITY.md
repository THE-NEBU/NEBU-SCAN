# Security Policy

## Supported Versions

| Version | Supported          |
| ------- | ------------------ |
| 1.0.x   | :white_check_mark: |

## Reporting a Vulnerability

We take security seriously at NEBU. If you discover a security vulnerability, please follow these steps:

### 1. **DO NOT** Create a Public Issue

Please **DO NOT** open a public GitHub issue for security vulnerabilities.

### 2. Report Privately

Send security reports to: **security@thenebu.com**

Include in your report:
- Description of the vulnerability
- Steps to reproduce
- Potential impact
- Any suggested fixes (optional)

### 3. Response Timeline

- **Initial Response**: Within 48 hours
- **Status Update**: Within 7 days
- **Fix Timeline**: Varies based on severity (we'll keep you updated)

### 4. Disclosure Policy

- We follow **responsible disclosure**
- We'll work with you to understand and fix the issue
- We'll credit you in the release notes (unless you prefer to remain anonymous)
- We'll publish a security advisory after the fix is released

## Security Best Practices for Users

1. **Use Read-Only IAM Permissions**
   - Grant only `ReadOnlyAccess` policy
   - Never use admin credentials

2. **Keep NEBU SCAN Updated**
   - Download latest version from official GitHub releases
   - Check for security updates regularly

3. **Verify Binary Integrity**
   - Download only from official GitHub releases
   - Check file sizes match release notes

4. **Review Telemetry Settings**
   - Use `nebu telemetry status` to review what's collected
   - Disable if needed: `nebu telemetry disable`

5. **Network Security**
   - NEBU SCAN only connects to: `https://api.thenebu.com`
   - All communication is encrypted (HTTPS/TLS)

## Security Features

- ✅ **Read-Only Operations**: Never modifies infrastructure
- ✅ **No Credential Storage**: AWS credentials handled by AWS SDK only
- ✅ **Encrypted Communication**: All network traffic uses HTTPS/TLS
- ✅ **Anonymous Telemetry**: No account IDs or resource identifiers collected
- ✅ **Minimal Dependencies**: Only trusted, well-maintained libraries

## Contact

- Security Issues: dev@thenebu.com
- General Support: dev@thenebu.com
- Website: https://thenebu.com
