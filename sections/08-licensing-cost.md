# 08 — Licensing & Cost

> 🟡 **MEDIUM PRIORITY** — Subscription costs, contracts, TCO baseline, and licensing implications.
> **27 Questions** | Pillar: Cost Optimization

---

## Purpose

Build a complete cost baseline for each application and understand licensing constraints that affect migration decisions. Contract end dates drive urgency, BYOL eligibility affects AWS architecture, and TCO comparison justifies the business case.

---

## Questions

| # | Question | WA Pillar | Applies To |
|---|----------|-----------|------------|
| 1 | What is the total annual cost? (license + support + infrastructure + people) | Cost | All |
| 2 | What is the licensing model? (per-user, per-core, per-socket, flat fee, consumption-based) | Cost | All |
| 3 | What is the contract term? (monthly, annual, multi-year) | Cost | SaaS |
| 4 | When does the current contract expire? | Cost | SaaS |
| 5 | Is auto-renewal in the contract? What is the notice period to cancel? | Cost | SaaS |
| 6 | Are there early termination penalties? How much? | Cost | SaaS |
| 7 | How many licenses are purchased vs. actively used? | Cost | SaaS |
| 8 | What is the cost per user/unit? | Cost | SaaS |
| 9 | Are there tiered pricing levels? (e.g., Standard, Professional, Enterprise) | Cost | SaaS |
| 10 | What support tier is purchased? (Basic, Premium, Enterprise) | Cost | All |
| 11 | Is BYOL (Bring Your Own License) eligible for AWS? | Cost | Self-Hosted |
| 12 | What are the vendor's cloud licensing terms? (e.g., Oracle on AWS rules, SAP on AWS) | Cost | Self-Hosted |
| 13 | Are there license mobility rights? (Microsoft SA, Oracle Named User Plus) | Cost | Self-Hosted |
| 14 | What would the estimated AWS infrastructure cost be? (EC2, RDS, storage, network) | Cost | Self-Hosted |
| 15 | Are Reserved Instances or Savings Plans applicable? | Cost | Self-Hosted |
| 16 | What is the projected 3-year TCO on AWS vs. current? | Cost | Self-Hosted |
| 17 | What one-time migration costs are expected? (professional services, tooling, testing) | Cost | All |
| 18 | Are there duplicate / overlapping tools that can be consolidated? | Cost | All |
| 19 | Are there unused licenses or subscriptions that can be terminated? | Cost | All |
| 20 | What is the cost of NOT migrating? (risk, tech debt, compliance gap) | Cost | All |
| 21 | Are there volume discount opportunities by consolidating vendors? | Cost | SaaS |
| 22 | What currency is the contract in? Any FX risk? | Cost | SaaS |
| 23 | Is there a finance / procurement approval process for changes? | Cost | All |
| 24 | Who owns the vendor relationship? (IT, procurement, business unit) | OE | All |
| 25 | What data egress costs are expected? (vendor charges to export data) | Cost | SaaS |
| 26 | Are there training costs for the new platform/tooling? | Cost | All |
| 27 | What is the expected payback period for migration investment? | Cost | All |

---

## How to Use

1. Work with **procurement / finance** to get actual spend data (invoices, POs)
2. Check vendor portals for license utilization reports
3. For self-hosted: use AWS Migration Evaluator or AWS Pricing Calculator for target cost
4. Identify **quick wins**: unused licenses, zombie subscriptions, tier downgrades
5. Build a **cost comparison table**: current state vs. target state (3-year view)

---

## Licensing Risk Matrix

| Scenario | Risk | Mitigation |
|----------|------|-----------|
| Oracle per-core licensing on AWS | High cost impact | Use License Included (RDS) or negotiate |
| SAP indirect access on AWS | Commercial risk | Clarify with SAP licensing team |
| Microsoft SQL Server on AWS | BYOL requires SA | Confirm SA coverage or use RDS License Included |
| Contract locks until 2028 | Can't migrate yet | Plan for post-contract or negotiate early exit |
| Vendor doesn't support AWS hosting | Compliance risk | Get written vendor approval |
| Per-socket license on larger AWS instances | Cost increase | Right-size or change license model |

---

## Cost Baseline Template

| App Name | Annual License | Annual Support | Annual Infra | Annual People | Total Annual | Contract End |
|----------|---------------|----------------|--------------|---------------|--------------|--------------|
| SAP S/4 | $1,200,000 | $240,000 | $480,000 | $360,000 | $2,280,000 | 2027-12 |
| Jira Cloud | $96,000 | Included | Included | $0 | $96,000 | 2026-09 |
| Custom CRM | $0 (custom) | $0 | $120,000 | $180,000 | $300,000 | N/A |
| ... | ... | ... | ... | ... | ... | ... |

---

[← Previous: Section 07](./07-integration.md) | [Back to README](../README.md) | [Next: Section 09 →](./09-networking.md)
