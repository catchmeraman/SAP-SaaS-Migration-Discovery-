# 01 — Initial Discovery

> 🔴 **START HERE** — Ask these questions FIRST before any deep-dive.
> **25 Questions** | Pillar: Operational Excellence

---

## Purpose

Establish scope, identify stakeholders, understand constraints, and set the foundation for the entire discovery program. Without this section, everything else lacks context.

---

## Questions

| # | Question | WA Pillar | Notes |
|---|----------|-----------|-------|
| 1 | How many applications are in scope for this migration? | OE | Get the total count — even approximate |
| 2 | Who is the executive sponsor for this migration program? | OE | Single accountable leader |
| 3 | Who are the application owners / business owners per app? | OE | Map every app to a named owner |
| 4 | What is the primary migration driver? (datacenter exit, cost reduction, modernization, compliance, M&A) | OE | Understanding "why" shapes "how" |
| 5 | What is the target completion date for the overall program? | OE | Hard deadline vs aspirational |
| 6 | Are there any immovable deadlines? (lease expiry, contract end, compliance mandate) | OE | These constrain wave planning |
| 7 | What is the change freeze calendar? (year-end, holidays, peak seasons) | OE | Blackout windows for cutover |
| 8 | Is there an existing CMDB or asset inventory? How current is it? | OE | Baseline accuracy check |
| 9 | What percentage of apps have up-to-date documentation? | OE | Identify documentation gaps early |
| 10 | Who has tribal knowledge of undocumented systems? | OE | Key-person risk identification |
| 11 | What is the migration budget? (capex and opex separately) | Cost | Financial envelope |
| 12 | Are there existing AWS accounts / Landing Zone / Control Tower? | Security | Existing cloud footprint |
| 13 | What governance / approval process exists for cloud deployments? | Security | Change control gates |
| 14 | What compliance frameworks apply? (SOC2, ISO 27001, HIPAA, GDPR, PCI-DSS) | Security | Compliance scope |
| 15 | Is there an existing migration team or will one be formed? | OE | Capacity planning |
| 16 | What previous migration experience does the team have? | OE | Skill gap assessment |
| 17 | Are there any applications explicitly OUT of scope? Why? | OE | Define boundaries clearly |
| 18 | What is the risk appetite? (aggressive timeline vs conservative/safe) | OE | Shapes wave sizing |
| 19 | Is there a rollback strategy expectation? (run parallel, instant cutback) | Reliability | Business expectation on safety |
| 20 | What communication cadence is expected? (weekly, biweekly steering) | OE | Governance rhythm |
| 21 | Are there union / works council / regulatory consultation requirements? | Security | Legal/HR constraints |
| 22 | What time zones are stakeholders and users distributed across? | Perf | Cutover window planning |
| 23 | Is there an existing network connection to AWS? (Direct Connect, VPN, none) | Perf | Data transfer constraint |
| 24 | What is the current IT operating model? (centralized, federated, outsourced) | OE | Who does what |
| 25 | Are there any parallel transformation programs running? (ERP upgrade, org restructure) | OE | Conflict / dependency risk |

---

## How to Use

1. Schedule a **90-minute kickoff workshop** with the program sponsor + key stakeholders
2. Work through these questions in order
3. Document answers in a shared location (Confluence, SharePoint, Notion)
4. Use answers to scope Sections 02–10 appropriately
5. Identify gaps that need follow-up before deep-dive begins

---

## Red Flags to Watch For

- No named application owners → discovery will stall
- No CMDB or inventory → need to build one (add 2-4 weeks)
- Immovable deadline < 6 months → aggressive wave planning needed
- No AWS presence → Landing Zone setup is a prerequisite
- Single person holds all knowledge → key-person risk, prioritize knowledge capture

---

[← Back to README](../README.md) | [Next: Section 02 →](./02-application-portfolio.md)
