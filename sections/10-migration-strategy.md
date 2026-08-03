# 10 — Migration Strategy

> 🔴 **HIGH PRIORITY** — 7R strategy assignment, wave planning, and decision matrix.
> **22 Questions** | Pillar: Operational Excellence, Cost Optimization, All Pillars

---

## Purpose

Assign a migration strategy (7R) to each application, group applications into migration waves, and build the execution plan. This section pulls together findings from all previous sections into actionable decisions.

---

## Questions

| # | Question | WA Pillar | Applies To |
|---|----------|-----------|------------|
| 1 | What is the recommended migration strategy? (Retain / Rehost / Replatform / Refactor / Replace / Retire / Relocate) | OE | All |
| 2 | What is the justification for this strategy? (cost, risk, timeline, modernization, compliance) | OE | All |
| 3 | Is this application a candidate for retirement / decommissioning? | Sustain | All |
| 4 | Is this application a candidate for consolidation with another app? | Sustain | All |
| 5 | Is there an AWS-native replacement? (e.g., self-hosted DB → RDS, custom monitoring → CloudWatch) | Cost | Self-Hosted |
| 6 | What migration wave should this application be in? (1-5) | OE | All |
| 7 | What dependencies must be migrated first? (shared DB, middleware, IdP) | OE | All |
| 8 | What dependencies must be migrated together? (tightly coupled apps) | OE | All |
| 9 | What is the estimated migration duration for this application? | OE | All |
| 10 | What is the acceptable cutover window? (weekend, holiday, off-peak hours) | OE | All |
| 11 | What is the rollback plan if migration fails? | Reliability | All |
| 12 | Is a parallel-run period needed? (old + new running simultaneously) | Reliability | All |
| 13 | What testing is required before go-live? (functional, performance, integration, UAT, security) | OE | All |
| 14 | Who is the go/no-go decision maker for cutover? | OE | All |
| 15 | What success criteria define a completed migration? | OE | All |
| 16 | Is there a hypercare period after migration? How long? Who provides it? | OE | All |
| 17 | What AWS services will this application use post-migration? | Perf | All |
| 18 | What training is needed for the operations team? | OE | All |
| 19 | Are there quick wins? (apps that can migrate in < 1 week with low risk) | Cost | All |
| 20 | Are there blockers? (apps that CANNOT migrate until a condition is met) | OE | All |
| 21 | What organizational change management is needed? (new processes, new tools, new roles) | OE | All |
| 22 | What is the decommission plan for the old environment post-migration? | Sustain | All |

---

## How to Use

1. Complete Sections 01-08 first — this section **synthesizes** all findings
2. Use the **7R Decision Matrix** below to assign a strategy per app
3. Group apps into **waves** based on dependencies, risk, and business priority
4. Build a **migration calendar** with cutover windows
5. Get **sign-off** from application owners and business stakeholders on wave plan

---

## 7R Decision Matrix

| Strategy | When to Use | Complexity | Speed | Cost Impact |
|----------|-------------|-----------|-------|-------------|
| **Retain** | SaaS that stays with vendor, no change needed | None | Instant | No change |
| **Rehost** | Lift-and-shift, need speed, minimize change | Low | Fast (days-weeks) | Similar or lower |
| **Replatform** | Minor optimizations during migration (e.g., move to RDS) | Medium | Medium (weeks) | Usually lower |
| **Refactor** | Rearchitect for cloud-native (containers, serverless) | High | Slow (months) | Lower long-term |
| **Replace** | Swap with SaaS or AWS-native alternative | Medium | Medium (weeks-months) | Variable |
| **Retire** | Decommission, no longer needed | Low | Fast | Cost savings |
| **Relocate** | Move to another AWS account/region (VMware Cloud on AWS) | Low | Fast | Similar |

---

## Wave Planning Template

| Wave | Timeline | Strategy | Target Apps | Characteristics |
|------|----------|----------|-------------|-----------------|
| **Wave 0** | Month 1-2 | Foundation | None (infra setup) | Landing Zone, Direct Connect, IAM, monitoring |
| **Wave 1** | Month 2-4 | Rehost / Retire | Simple self-hosted, standalone, zombie apps | Low risk, build team muscle memory |
| **Wave 2** | Month 4-6 | Rehost / Replatform | Mid-tier apps, some integrations | Medium complexity, proven patterns |
| **Wave 3** | Month 6-9 | Replatform / Replace | Core business apps, SaaS optimization | High complexity, many dependencies |
| **Wave 4** | Month 9-12 | Replatform / Refactor | Critical systems (ERP, finance) | Highest risk, most preparation |
| **Wave 5** | Month 12+ | Retire / Optimize | Decommission old infra, optimize costs | Cleanup and steady-state |

---

## Migration Success Criteria (per app)

| Criteria | Measurement |
|----------|-------------|
| Functional testing passes | 100% of test cases pass |
| Performance within threshold | Response time ≤ baseline + 10% |
| All integrations working | End-to-end data flow confirmed |
| No P1/P2 incidents in 48 hours | Hypercare monitoring clean |
| Business owner sign-off | Written approval received |
| Old environment decommissioned | Resources released within 30 days |

---

[← Previous: Section 09](./09-networking.md) | [Back to README](../README.md)
