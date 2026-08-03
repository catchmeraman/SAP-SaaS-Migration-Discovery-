# 🏗️ SaaS & Application Migration Discovery Framework

> Structured discovery for 300+ SaaS/subscription applications (SAP, Atlassian, Salesforce, custom, and more) migrating to or integrating with AWS.

**Aligned with:** AWS Well-Architected Framework | SAP Lens | Migration Best Practices

---

## 🎯 What This Is

You have **300+ applications** — a mix of:
- Enterprise SaaS (SAP, Salesforce, Workday, ServiceNow)
- Productivity SaaS (Jira, Confluence, Slack, Microsoft 365)
- Self-hosted applications (on-prem, managed hosting, colo)
- Custom-built applications
- Legacy applications (status unknown)

You need to **discover what you have, where it runs, and what to do with it** before migrating to AWS.

This framework gives you **structured discovery questions broken into 10 sections** — start with Section 01 (Initial Discovery), then deep-dive per application.

---

## 📂 Discovery Sections

| # | Section | Priority | Questions | Purpose |
|---|---------|----------|-----------|---------|
| **01** | [Initial Discovery](./sections/01-initial-discovery.md) | 🔴 **START HERE** | 25 | Scope, stakeholders, constraints — ask these FIRST |
| **02** | [Application Portfolio](./sections/02-application-portfolio.md) | 🔴 HIGH | 22 | Inventory all 300+ apps (per-app identity, criticality, hosting) |
| **03** | [Infrastructure](./sections/03-infrastructure.md) | 🟡 MEDIUM | 28 | Compute, DB, storage (self-hosted apps only) |
| **04** | [Security & Compliance](./sections/04-security-compliance.md) | 🔴 HIGH | 25 | Identity, data protection, compliance frameworks |
| **05** | [Operations](./sections/05-operations.md) | 🟡 MEDIUM | 24 | Support model, monitoring, change management, backup |
| **06** | [Data](./sections/06-data.md) | 🔴 HIGH | 20 | Volume, portability, classification, residency |
| **07** | [Integration](./sections/07-integration.md) | 🔴 HIGH | 17 | Dependencies, APIs, middleware, external connections |
| **08** | [Licensing & Cost](./sections/08-licensing-cost.md) | 🟡 MEDIUM | 27 | Subscription costs, contracts, TCO baseline |
| **09** | [Networking](./sections/09-networking.md) | ⚪ OUT OF SCOPE | 15 | Network topology, DNS, connectivity (for future) |
| **10** | [Migration Strategy](./sections/10-migration-strategy.md) | 🔴 HIGH | 22 | 7R strategy, wave planning, decision matrix |

**Total: ~225 questions across 10 sections**

---

## 🏛️ Well-Architected Framework Alignment

Questions are mapped to AWS Well-Architected Framework pillars across **all application types**:

| Pillar | Sections | Key Questions (Examples) |
|--------|----------|-------------------------|
| **Operational Excellence** | Operations, Monitoring, Change Management | How are changes deployed? Patching cadence? SLA with vendor? |
| **Security** | Identity, Network, Data Protection | SSO/SAML config? Encryption at rest? Compliance frameworks? Data residency? |
| **Reliability** | HA, DR, Backup & Recovery | Vendor SLA? RTO/RPO? Failover testing? Multi-region? |
| **Performance Efficiency** | Compute, Storage, Network | User concurrency? API rate limits? Latency requirements? Data volume? |
| **Cost Optimization** | Current TCO, Licensing, Right-sizing | Annual subscription cost? Contract end date? BYOL eligibility? Unused licenses? |
| **Sustainability** | Consolidation, Decommission | Duplicate tools? Zombie subscriptions? Consolidation opportunities? |

---

## 🚀 Recommended Approach

```
┌────────────┬─────────────────────────────────────────────────┐
│ Week 1     │ Section 01: Initial Discovery (25 Qs)   ← START │
│            │ Section 02: Portfolio Inventory (all apps)       │
├────────────┼─────────────────────────────────────────────────┤
│ Week 2-3   │ Section 04: Security & Compliance               │
│            │ Section 06: Data                                 │
│            │ Section 07: Integration & Dependencies           │
├────────────┼─────────────────────────────────────────────────┤
│ Week 3-4   │ Section 03: Infrastructure (self-hosted apps)   │
│            │ Section 05: Operations                           │
│            │ Section 08: Licensing & Cost                     │
├────────────┼─────────────────────────────────────────────────┤
│ Week 5-6   │ Section 10: Migration Strategy & Waves          │
│            │ Final deliverables                               │
└────────────┴─────────────────────────────────────────────────┘
```

---

## 📊 Application Types Covered

| Type | Examples | Primary Discovery Focus |
|------|----------|------------------------|
| **Enterprise SaaS** | SAP S/4, Salesforce, Workday, ServiceNow | Integration, data portability, licensing |
| **Productivity SaaS** | Jira, Confluence, Slack, MS 365, Google Workspace | SSO, consolidation, cost optimization |
| **Self-Hosted** | SAP ECC, custom apps, legacy systems | Infrastructure, migration method, modernization |
| **Platform SaaS** | Datadog, PagerDuty, Snowflake, GitHub | Integration with AWS, data flow |
| **Managed Hosting** | Apps at MSP/hosting provider | Contract, data portability, timeline |
| **Unknown/Legacy** | Apps with no documentation | Start with Section 01 + 02 to identify |

---

## 🎯 Key Outputs After Discovery

| # | Deliverable | Source Sections |
|---|-------------|-----------------|
| 1 | **Application Inventory** (300+ apps categorized) | 01 + 02 |
| 2 | **Dependency Map** (who talks to whom) | 07 |
| 3 | **Security & Compliance Matrix** | 04 |
| 4 | **Data Classification & Residency Map** | 06 |
| 5 | **Cost Baseline** (total annual spend) | 08 |
| 6 | **Migration Strategy per App (7R)** | 10 |
| 7 | **Wave Plan with Timeline** | 10 |
| 8 | **Risk Register** | All sections |

---

## 🔧 Recommended Discovery Tools

| Tool | Purpose | Applies To |
|------|---------|------------|
| AWS Application Discovery Service | Auto-discover servers, dependencies, utilization | Self-hosted |
| AWS Migration Hub | Track migration progress | All types |
| AWS Migration Evaluator | TCO analysis and right-sizing | Self-hosted + managed |
| SAP EarlyWatch Alert | SAP system health baseline | SAP only |
| SAP Readiness Check | S/4HANA conversion readiness | SAP only |
| Vendor Admin Consoles | Export user lists, integrations, config | All SaaS |
| SSO/IdP Reports | Which apps use centralized identity | All types |
| CMDB / ServiceNow | Existing asset inventory baseline | All types |
| Network Flow Logs / APM | Dependency and traffic mapping | Self-hosted + hybrid |

---

## 📖 References

- [AWS Well-Architected Framework](https://docs.aws.amazon.com/wellarchitected/latest/framework/welcome.html)
- [AWS Well-Architected — SAP Lens](https://docs.aws.amazon.com/wellarchitected/latest/sap-lens/sap-lens.html)
- [AWS Migration Hub](https://aws.amazon.com/migration-hub/)
- [AWS Application Discovery Service](https://aws.amazon.com/application-discovery/)
- [AWS Migration Evaluator](https://aws.amazon.com/migration-evaluator/)
- [AWS Prescriptive Guidance — Migration](https://docs.aws.amazon.com/prescriptive-guidance/latest/migration-factory-guide/welcome.html)
- [SAP on AWS](https://aws.amazon.com/sap/)

---

**Author:** Ramandeep Chandna | August 2026
