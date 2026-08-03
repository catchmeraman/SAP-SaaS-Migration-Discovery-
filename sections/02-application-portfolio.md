# 02 — Application Portfolio

> 🔴 **HIGH PRIORITY** — Inventory all 300+ apps with identity, criticality, and hosting details.
> **22 Questions** | Pillar: Operational Excellence, Cost Optimization

---

## Purpose

Build a complete, validated inventory of every application in scope. Each app gets classified by type, criticality, hosting model, and ownership. This is the foundation for all subsequent sections.

---

## Questions

*Ask per application or application group:*

| # | Question | WA Pillar | Notes |
|---|----------|-----------|-------|
| 1 | What is the application name and vendor (if applicable)? | OE | Canonical name, avoid aliases |
| 2 | What type is it? (Enterprise SaaS / Productivity SaaS / Self-Hosted / Platform SaaS / Managed Hosting / Legacy) | OE | Use standard taxonomy |
| 3 | What business function does it support? (Finance, HR, Sales, Engineering, Operations) | OE | Business alignment |
| 4 | What is the business criticality? (Critical / High / Medium / Low) | Reliability | Drives RTO/RPO and wave priority |
| 5 | How many users rely on this application? (total + concurrent peak) | Perf | Sizing and licensing |
| 6 | Where is it currently hosted? (Vendor cloud / On-prem DC / Colo / MSP / Hybrid / Unknown) | OE | Migration method depends on this |
| 7 | Who is the technical owner / admin? | OE | Named individual |
| 8 | Who is the business owner? | OE | Named individual |
| 9 | What is the current version / release? Is it supported? | Reliability | End-of-life risk |
| 10 | Is the application actively maintained by the vendor? | Reliability | Abandoned = high risk |
| 11 | What is the contract end date? (for SaaS/managed) | Cost | Drives urgency |
| 12 | What is the total annual cost? (license + support + infra + people) | Cost | TCO baseline |
| 13 | What data does this application store? (PII, financial, health, IP, none) | Security | Data classification |
| 14 | What integrations does this application have? (list upstream + downstream) | Perf | Dependency mapping |
| 15 | Is SSO/SAML/OIDC configured? Which IdP? | Security | Identity posture |
| 16 | What is the current availability SLA? (from vendor or internal) | Reliability | Baseline expectation |
| 17 | What is the RTO/RPO requirement? | Reliability | Recovery expectations |
| 18 | Is there existing documentation? (architecture diagram, runbook, DR plan) | OE | Documentation gap |
| 19 | What environments exist? (Prod only? Dev/Test/UAT?) | OE | Non-prod footprint |
| 20 | Has this app been migrated before? What was the experience? | OE | Lessons learned |
| 21 | Are there any known issues / tech debt with this application? | OE | Risk register input |
| 22 | What is the preliminary migration strategy? (Retain / Rehost / Replatform / Refactor / Replace / Retire / Relocate) | OE | Initial 7R assignment |

---

## How to Use

1. Start with your CMDB / asset register export
2. Enrich with data from vendor admin consoles, finance (invoices), and stakeholder interviews
3. Fill one row per application in a master spreadsheet
4. Flag any apps where answers are "Unknown" — these need deeper discovery
5. Use criticality + cost + contract end date to prioritize which apps get deep-dived first

---

## Application Type Quick Reference

| Type | Hosting | You Manage Infra? | Examples |
|------|---------|-------------------|----------|
| Enterprise SaaS | Vendor cloud | No | SAP S/4, Salesforce, Workday |
| Productivity SaaS | Vendor cloud | No | Jira, Slack, M365 |
| Self-Hosted | Your DC / colo | Yes | SAP ECC, custom apps |
| Platform SaaS | Vendor (often on AWS) | No | Datadog, Snowflake |
| Managed Hosting | MSP datacenter | Shared | Apps at Rackspace, Kyndryl |
| Unknown/Legacy | Unknown | Unknown | Needs discovery |

---

[← Previous: Section 01](./01-initial-discovery.md) | [Back to README](../README.md) | [Next: Section 03 →](./03-infrastructure.md)
