# 04 — Security & Compliance

> 🔴 **HIGH PRIORITY** — Identity, data protection, and compliance frameworks.
> **25 Questions** | Pillar: Security

---

## Purpose

Assess the security posture, identity configuration, data protection mechanisms, and compliance requirements for each application. This section applies to **all application types** — SaaS, self-hosted, and legacy.

---

## Questions

| # | Question | WA Pillar | Applies To |
|---|----------|-----------|------------|
| 1 | What authentication method is used? (SAML, OIDC, local accounts, AD/LDAP, API key) | Security | All |
| 2 | Is MFA enforced? For all users or admins only? | Security | All |
| 3 | What Identity Provider (IdP) is used? (Okta, Azure AD, AWS IAM Identity Center, Ping, OneLogin) | Security | All |
| 4 | Is user provisioning/deprovisioning automated? (SCIM, JIT, manual) | Security | All |
| 5 | What authorization model is used? (RBAC, ABAC, custom, none) | Security | All |
| 6 | How many admin / privileged accounts exist? Are they audited? | Security | All |
| 7 | Is data encrypted at rest? What mechanism? (AES-256, vendor-managed, customer KMS) | Security | All |
| 8 | Is data encrypted in transit? What TLS version? | Security | All |
| 9 | Who manages encryption keys? (Vendor, customer-managed KMS, HSM, shared) | Security | All |
| 10 | What compliance certifications does the vendor hold? (SOC2, ISO 27001, FedRAMP) | Security | SaaS |
| 11 | What compliance frameworks must this application satisfy? (GDPR, HIPAA, PCI-DSS, SOX) | Security | All |
| 12 | Where is data physically stored? (specific region / country) | Security | All |
| 13 | Are there data residency or sovereignty requirements? | Security | All |
| 14 | Is there a Data Processing Agreement (DPA) with the vendor? | Security | SaaS |
| 15 | What audit logging exists? Where are logs stored? Retention period? | Security | All |
| 16 | Is there a vulnerability management process for this app? (scanning, patching cadence) | Security | Self-Hosted |
| 17 | When was the last penetration test or security assessment? Findings? | Security | All |
| 18 | Is there network segmentation / VPC isolation for this app? | Security | Self-Hosted |
| 19 | What IP allowlisting or firewall rules exist? | Security | All |
| 20 | How are secrets and credentials managed? (Secrets Manager, Vault, hardcoded, config files) | Security | Self-Hosted |
| 21 | Is there a SIEM integration? (Splunk, Sentinel, CloudTrail, GuardDuty) | Security | All |
| 22 | What is the incident response process for security events in this app? | Security | All |
| 23 | Are data classification labels applied to this app's data? | Security | All |
| 24 | Is PII/PHI data masked or tokenized in non-production environments? | Security | All |
| 25 | Is there a documented shared responsibility model with the vendor? | Security | SaaS |

---

## How to Use

1. Start with your **IdP dashboard** — which apps have SSO configured vs. local auth only?
2. Cross-reference with **compliance team** — which apps are in scope for SOC2/HIPAA/GDPR audits?
3. For self-hosted apps: review firewall rules, encryption settings, patching history
4. For SaaS: request vendor security documentation (SOC2 report, DPA, data residency statement)
5. Flag any app with **local-only auth + no MFA + regulated data** as HIGH RISK

---

## Common Security Gaps

| Gap | Risk Level | Remediation |
|-----|-----------|-------------|
| No SSO configured | High | Integrate with IdP before migration |
| No MFA for admins | Critical | Enable immediately |
| Data in non-compliant region | High | Plan data migration or vendor negotiation |
| Hardcoded credentials | Critical | Move to Secrets Manager |
| No audit logging | High | Enable before migration |
| No encryption at rest | High | Enable or plan for encrypted target |
| Stale admin accounts | Medium | Audit and remove |

---

[← Previous: Section 03](./03-infrastructure.md) | [Back to README](../README.md) | [Next: Section 05 →](./05-operations.md)
