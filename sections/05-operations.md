# 05 — Operations

> 🟡 **MEDIUM PRIORITY** — Support model, monitoring, change management, and backup.
> **24 Questions** | Pillar: Operational Excellence

---

## Purpose

Understand how each application is operated, supported, monitored, and maintained today. This determines the operational readiness for migration and the target operating model on AWS.

---

## Questions

| # | Question | WA Pillar | Applies To |
|---|----------|-----------|------------|
| 1 | Who provides L1/L2/L3 support? (Internal team, vendor, MSP, combination) | OE | All |
| 2 | What are the support hours? (24/7, business hours, follow-the-sun) | OE | All |
| 3 | What is the support SLA? (response time, resolution time) | OE | All |
| 4 | What ticketing system is used? (ServiceNow, Jira SM, Zendesk, email) | OE | All |
| 5 | What is the change management process? (CAB, peer review, automated, none) | OE | All |
| 6 | How are deployments / releases done? (CI/CD, manual, vendor-managed, scheduled) | OE | All |
| 7 | What is the release frequency? (daily, weekly, monthly, quarterly, vendor-driven) | OE | All |
| 8 | What is the patching cadence? (OS, middleware, application) | OE | Self-Hosted |
| 9 | What monitoring is in place? (APM, infrastructure, logs, synthetics) | OE | All |
| 10 | What monitoring tools are used? (CloudWatch, Datadog, Dynatrace, Zabbix, Nagios, vendor) | OE | All |
| 11 | What alerting rules exist? Who gets paged? | OE | All |
| 12 | Is there a runbook / playbook for common incidents? | OE | All |
| 13 | What is the on-call rotation? | OE | All |
| 14 | How are incidents classified? (Sev1-4, P1-P4, custom) | OE | All |
| 15 | What is the average MTTR (Mean Time to Resolve) for P1 incidents? | OE | All |
| 16 | How many incidents occurred in the last 12 months? Top causes? | OE | All |
| 17 | Are there non-production environments? (Dev, Test, Staging, UAT, Pre-Prod) | OE | All |
| 18 | How is data refreshed in non-prod environments? | OE | All |
| 19 | What automation exists today? (IaC, scripts, scheduled tasks, cron jobs) | OE | Self-Hosted |
| 20 | Is there configuration drift detection? | OE | Self-Hosted |
| 21 | What backup strategy is in place? (full, incremental, differential, snapshot) | Reliability | All |
| 22 | What is the backup frequency and retention period? | Reliability | All |
| 23 | Are backups tested regularly? When was the last successful restore? | Reliability | All |
| 24 | Are there key-person dependencies? (only one person knows how it works) | OE | All |

---

## How to Use

1. Interview the **operations / SRE team** and the **application support team**
2. Review ticketing system for incident volume and patterns
3. Check monitoring dashboards for coverage gaps
4. For SaaS apps: focus on vendor SLA, support model, and upgrade process
5. For self-hosted: full operational review (patching, monitoring, backup, DR)

---

## Operational Maturity Assessment

| Level | Characteristics | Typical Apps |
|-------|----------------|--------------|
| **Level 1 — Reactive** | No monitoring, no runbooks, break-fix only | Legacy, undocumented |
| **Level 2 — Managed** | Basic monitoring, manual patching, some docs | Older self-hosted |
| **Level 3 — Defined** | Runbooks, regular patching, alerting configured | Most production apps |
| **Level 4 — Measured** | SLOs defined, MTTR tracked, post-mortems done | Critical apps |
| **Level 5 — Optimized** | Full automation, self-healing, chaos engineering | Cloud-native apps |

---

[← Previous: Section 04](./04-security-compliance.md) | [Back to README](../README.md) | [Next: Section 06 →](./06-data.md)
