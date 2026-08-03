# 09 — Networking

> ⚪ **OUT OF SCOPE** — Documented here for future use when networking comes into scope.
> **15 Questions** | Pillar: Performance Efficiency, Security

---

## Purpose

Capture network topology, connectivity, DNS, and bandwidth requirements. This section is typically handled by a separate network workstream but is documented here so discovery can capture relevant data points when encountered.

---

## Questions

| # | Question | WA Pillar | Applies To |
|---|----------|-----------|------------|
| 1 | What is the current network topology? (flat, segmented, hub-and-spoke, mesh) | Security | Self-Hosted |
| 2 | What WAN connectivity exists between sites? (MPLS, SD-WAN, internet, dedicated links) | Perf | Self-Hosted |
| 3 | Is there existing connectivity to AWS? (Direct Connect, Site-to-Site VPN, none) | Perf | All |
| 4 | What bandwidth is available between on-prem and AWS? | Perf | Self-Hosted |
| 5 | What DNS solution is in use? (internal AD DNS, Route 53, Infoblox, BIND) | Perf | All |
| 6 | What is the DNS TTL for application endpoints? | Perf | All |
| 7 | Are there IP address dependencies? (hardcoded IPs, IP allowlists, certificates tied to IPs) | Security | All |
| 8 | What firewall rules reference this application? (source → destination → port) | Security | All |
| 9 | What proxy / web filtering applies to this application? (Zscaler, Squid, BlueCoat) | Security | All |
| 10 | What latency requirements exist between users and the application? | Perf | All |
| 11 | What latency requirements exist between this app and its integrations? | Perf | All |
| 12 | Are there any network-level compliance requirements? (encryption in transit, no internet exposure) | Security | All |
| 13 | What VPN or private connectivity is required for users to access the app? | Security | All |
| 14 | Are there Geographic / CDN requirements? (users in multiple regions) | Perf | All |
| 15 | What load balancing / traffic management exists? (Global: DNS-based, regional: L4/L7) | Perf | Self-Hosted |

---

## How to Use

1. This section is typically **deferred** to a dedicated network workstream
2. Capture answers opportunistically when they come up during other sections
3. Flag any **hard dependencies** (e.g., latency < 2ms between app and DB) that affect architecture
4. Identify apps that **cannot use internet connectivity** (must go via Direct Connect)
5. Note any **hardcoded IPs** that will break during migration

---

## Key Network Considerations for Migration

| Consideration | Impact | Action |
|---------------|--------|--------|
| No Direct Connect exists | Data transfer over internet (slow, less secure) | Plan DX provisioning (6-12 weeks lead time) |
| Low bandwidth to AWS | Large data migrations will be slow | Consider Snowball or staged migration |
| Hardcoded IPs in app config | App will break when IPs change | Refactor to use DNS names |
| Latency-sensitive integration | Must keep components close together | Co-locate in same AZ/region or migrate together |
| DNS TTL is 24+ hours | Cutover will have long propagation delay | Lower TTL weeks before migration |

---

## Status: Out of Scope

This section is included for completeness but is **not part of the current discovery scope**. It will be activated when the networking workstream begins.

---

[← Previous: Section 08](./08-licensing-cost.md) | [Back to README](../README.md) | [Next: Section 10 →](./10-migration-strategy.md)
