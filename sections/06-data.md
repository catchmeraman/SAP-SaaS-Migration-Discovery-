# 06 — Data

> 🔴 **HIGH PRIORITY** — Volume, portability, classification, and data residency.
> **20 Questions** | Pillar: Security, Reliability, Performance Efficiency

---

## Purpose

Understand the data landscape for each application — what data exists, how much, where it lives, how sensitive it is, and how portable it is. This is critical for migration planning, compliance, and cost estimation.

---

## Questions

| # | Question | WA Pillar | Applies To |
|---|----------|-----------|------------|
| 1 | What is the total data volume? (DB + files + attachments + logs) | Perf | All |
| 2 | What is the data growth rate? (GB/month or TB/year) | Perf | All |
| 3 | What data classification applies? (Public, Internal, Confidential, Restricted) | Security | All |
| 4 | Does this application store PII? (names, emails, SSN, addresses) | Security | All |
| 5 | Does this application store regulated data? (PHI, PCI, financial) | Security | All |
| 6 | Where is data physically stored today? (country, region, datacenter) | Security | All |
| 7 | Are there data residency or sovereignty requirements? (must stay in country X) | Security | All |
| 8 | Can data be exported from this application? What format? (API, CSV, SQL dump, proprietary) | Perf | All |
| 9 | Does the vendor provide a migration / export tool? | OE | SaaS |
| 10 | What is the data retention policy? (how long must data be kept?) | Security | All |
| 11 | Is there historical data that can be archived vs. actively migrated? | Cost | All |
| 12 | What databases are involved? (type, engine, version) | Perf | Self-Hosted |
| 13 | Are there file shares / unstructured data? (documents, images, videos) | Perf | All |
| 14 | What is the acceptable downtime for data cutover? | Reliability | All |
| 15 | Is Change Data Capture (CDC) possible for continuous sync? | Perf | Self-Hosted |
| 16 | What data transformation is needed during migration? (schema changes, encoding) | Perf | All |
| 17 | What data validation / reconciliation process will confirm completeness? | OE | All |
| 18 | Are there data dependencies requiring a specific migration order? | OE | All |
| 19 | Is there data shared between multiple applications? (shared DB, shared files) | Perf | All |
| 20 | What backup and recovery exists for this data today? | Reliability | All |

---

## How to Use

1. Start with the **database team** or **storage team** for volume and growth data
2. Work with **compliance / legal** for classification, residency, and retention requirements
3. For SaaS: check vendor documentation for export capabilities and data portability
4. For self-hosted: run database size queries, check file system usage
5. Identify **shared data** — apps sharing a database need to be migrated together

---

## Data Migration Method Selection

| Data Volume | Bandwidth | Acceptable Downtime | Recommended Approach |
|-------------|-----------|--------------------|--------------------|
| < 100 GB | Good internet | Hours | Direct transfer (DMS, native tools) |
| 100 GB – 5 TB | Direct Connect | Hours | AWS DMS with CDC |
| 5 TB – 50 TB | Direct Connect | Minimal | AWS DMS + parallel streams |
| 50 TB+ | Limited bandwidth | Days acceptable | AWS Snowball / Snowball Edge |
| Any size | Good network | Zero downtime | DMS with CDC + cutover |

---

## Data Classification Quick Reference

| Classification | Examples | Handling Requirements |
|---------------|----------|---------------------|
| **Restricted** | Credentials, encryption keys, health records | Encrypted everywhere, strict access control, audit all access |
| **Confidential** | PII, financial data, contracts, IP | Encrypted at rest + transit, access limited to need-to-know |
| **Internal** | Employee data, internal reports, project docs | Standard access controls, no public exposure |
| **Public** | Marketing content, public APIs, open data | No special controls required |

---

[← Previous: Section 05](./05-operations.md) | [Back to README](../README.md) | [Next: Section 07 →](./07-integration.md)
