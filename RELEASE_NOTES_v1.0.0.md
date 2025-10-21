# NEBU SCAN v1.0.0 - Initial Release

## 🎉 First Public Release

NEBU SCAN is a powerful CLI tool for auditing AWS cloud infrastructure. Find cost savings, security issues, and optimization opportunities across your entire AWS account.

## 📦 Downloads

**Choose the binary for your operating system:**

| Platform | Architecture | Download | Size |
|----------|-------------|----------|------|
| Linux | x86_64 | `nebu-linux-amd64` | 149 MB |
| Windows | x86_64 | `nebu-windows-amd64.exe` | 151 MB |
| macOS | Intel (x86_64) | `nebu-darwin-amd64` | 152 MB |
| macOS | Apple Silicon (ARM64) | `nebu-darwin-arm64` | 133 MB |

### Installation

**Linux / macOS:**
```bash
# Download the appropriate binary
# Make it executable
chmod +x nebu-*
# Move to your PATH
sudo mv nebu-* /usr/local/bin/nebu
```

**Windows:**
```powershell
# Download nebu-windows-amd64.exe
# Rename to nebu.exe and add to your PATH
```

## ✨ What's Included

### Cost Waste Optimization
Analyze **50+ AWS services** for cost-saving opportunities:
- **Compute:** EC2, ECS, EKS, Lambda, EMR
- **Storage:** EBS, S3, EFS, FSx
- **Database:** RDS, DynamoDB, ElastiCache, Redshift, DocumentDB
- **Networking:** NAT Gateway, Load Balancers, VPC, Transit Gateway
- **Data & Analytics:** Glue, Kinesis
- **AI/ML:** SageMaker
- And many more...

### Security Audits
Security scanning for **4 core AWS services:**
- **IAM:** 15+ security checks (admin access, MFA, inactive credentials, overly permissive policies)
- **S3:** 10+ security checks (public buckets, encryption, versioning, access logging)
- **EC2:** 8+ security checks (public instances, security groups, IMDSv2, unencrypted volumes)
- **RDS:** 12+ security checks (public databases, encryption, backups, multi-AZ)

## 🚀 Key Features

- **Multi-Region Scanning** - Scan all AWS regions or target specific ones
- **Reserved Instance Awareness** - Optionally exclude resources covered by RI/Savings Plans
- **Business Impact Scoring** - Prioritize opportunities by implementation complexity
- **Detailed Remediation** - Step-by-step guides for each optimization
- **Interactive Mode** - User-friendly prompts for easy scanning
- **Privacy-First** - Anonymous telemetry (opt-in), no sensitive data collected
- **Read-Only Operations** - Safe to use in production, never modifies infrastructure

## 📖 Quick Start

### Prerequisites
- AWS CLI configured with credentials (`aws configure` or AWS SSO)
- Read-only IAM permissions (AWS managed policy `ReadOnlyAccess` recommended)

### Your First Scan

**Interactive Mode (Easiest):**
```bash
nebu
```

**Direct Commands:**
```bash
# Scan for cost waste (all regions, exclude RI/SP covered resources)
nebu aws waste your-profile exclude

# Scan for cost waste (specific region)
nebu aws waste your-profile exclude us-east-1

# Security audit (all regions)
nebu aws security your-profile

# Security audit (specific region)
nebu aws security your-profile us-west-2
```

### Example Output

**Waste Optimization:**
```
SUMMARY
  Total Monthly Waste:     $4,250.50
  Opportunities Found:     318
  Regions Scanned:         17

BY BUSINESS IMPACT
  ● CRITICAL:    25 opportunities → $1,850.25/month
  ● HIGH:        48 opportunities → $1,420.75/month
  ● MEDIUM:      156 opportunities → $820.50/month
  ● LOW:         89 opportunities → $159.00/month
```

**Security Audit:**
```
SUMMARY
  Regions Scanned:    17
  Total Findings:     672

FINDINGS BY SEVERITY
  ● CRITICAL:    59 findings
  ● HIGH:        126 findings
  ● MEDIUM:      336 findings
  ● LOW:         151 findings
```

## 🔒 Privacy & Telemetry

NEBU collects **anonymous usage data** to improve the tool (opt-in by default).

**What we collect:**
- Anonymous installation ID (random UUID)
- CLI version, OS, architecture
- Scan duration and regions count
- Number of findings by severity
- Total savings amount

**What we DON'T collect:**
- Cloud account IDs or credentials
- Resource names, IDs, ARNs, or identifiers
- IP addresses or personal information
- Any sensitive data

**Manage telemetry:**
```bash
nebu telemetry status   # Check status
nebu telemetry enable   # Enable
nebu telemetry disable  # Disable
```

## 📚 Documentation

Full documentation available in `README.md` including:
- Complete command reference
- Supported services list
- Understanding scan results
- Troubleshooting guide
- FAQ

For telemetry integration details, see `TELEMETRY_PAYLOAD.md`.

## 🌐 NEBU Cloud (Optional)

Want historical tracking, team sharing, and compliance reports?

Create a free account at **[report.thenebu.com/register](https://report.thenebu.com/register)**

Features:
- Track savings and security posture over time
- Share reports with your team
- Export compliance reports (SOC2, HIPAA, PCI-DSS)
- Get personalized recommendations
- Benchmark against similar companies

## 🛠️ Technical Details

**Built with:**
- Go 1.21+
- AWS SDK for Go v2
- Binary size optimization with `-ldflags="-s -w"` and `-trimpath`

**Build flags used:**
- Strip debug symbols (`-s`)
- Strip DWARF symbols (`-w`)
- Remove file system paths (`-trimpath`)

## 💬 Support

Need help?
- **Email:** support@thenebu.com
- **Website:** https://thenebu.com
- **Issues:** Create an issue on GitHub

## 📜 License

**Proprietary Software** - All rights reserved.

This is closed-source software provided as compiled binaries only. You may not redistribute, modify, reverse engineer, or create derivative works.

Copyright © 2025 NEBU

---

## 🏷️ GitHub Release Tags

Use these tags when creating the GitHub release:

```
v1.0.0
aws
cloud-cost-optimization
finops
cloud-security
aws-cli
cost-optimization
security-audit
infrastructure-audit
cloud-audit
devops
sre
cloud-management
aws-tools
```

## 📝 GitHub Release Short Description

```
NEBU v1.0.0 - AWS Cloud Auditing CLI. Find cost savings and security issues across 50+ AWS services. Scan for waste, detect misconfigurations, get actionable recommendations. Read-only, safe for production.
```

---

**Ready to optimize your AWS infrastructure?** Download the binary for your platform above and run your first scan in under 2 minutes!

---

**GitHub Repository:** https://github.com/THE-NEBU/NEBU-SCAN
