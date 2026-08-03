# 07 — Integration

> 🔴 **HIGH PRIORITY** — Dependencies, APIs, middleware, and external connections.
> **17 Questions** | Pillar: Performance Efficiency, Reliability

---

## Purpose

Map all integration points for each application — what talks to what, how, and how often. Integration complexity is the #1 driver of migration risk. An app with 50 interfaces is fundamentally harder to migrate than a standalone app.

---

## Questions

| # | Question | WA Pillar | Applies To |
|---|----------|-----------|------------|
| 1 | What upstream systems send data INTO this application? (list all) | Perf | All |
| 2 | What downstream systems receive data FROM this application? (list all) | Perf | All |
| 3 | What integration method is used for each? (REST API, SOAP, file transfer/SFTP, database link, message queue, ESB, webhook, email) | Perf | All |
| 4 | What is the integration frequency? (real-time/event-driven, near-real-time, batch hourly, batch daily, batch weekly) | Perf | All |
| 5 | What middleware / integration platform is used? (MuleSoft, SAP PI/PO/CPI, Dell Boomi, Informatica, Apache Kafka, custom scripts) | Perf | All |
| 6 | What data format is exchanged? (JSON, XML, IDOC, CSV, EDI, flat file, BAPI) | Perf | All |
| 7 | What volume of data flows through each integration daily? | Perf | All |
| 8 | What credentials / service accounts are used? (API keys, OAuth tokens, certificates, basic auth) | Security | All |
| 9 | Are there certificate or token expiry dates to track? | Security | All |
| 10 | Are there SLAs on integration latency or delivery? | Reliability | All |
| 11 | What error handling exists? (retry logic, dead-letter queue, alerting, manual intervention) | Reliability | All |
| 12 | Is there an API gateway or integration layer in front? | Security | All |
| 13 | What happens to integrations during migration cutover? (break, queue, dual-run) | OE | All |
| 14 | Can integration endpoints be repointed without downtime? (DNS swap, config change, feature flag) | Reliability | All |
| 15 | Are there third-party / external integrations requiring vendor coordination? | OE | All |
| 16 | Are there point-to-point integrations (no middleware) that are at risk? | Reliability | All |
| 17 | What integration testing strategy exists? (end-to-end, contract testing, mocks) | OE | All |

---

## How to Use

1. Build a **dependency matrix** — each row is an app, each column is an integration partner
2. Visualize as a **network graph** — identify hubs (apps with many connections)
3. Identify **integration clusters** — apps that must migrate together
4. Flag integrations that cross the migration boundary (cloud ↔ on-prem during transition)
5. Plan for **temporary dual-running** where integrations can't be cut over instantly

---

## Integration Complexity Scoring

| Complexity | # Integrations | Characteristics | Migration Risk |
|-----------|---------------|-----------------|----------------|
| **Very High** | 50+ | Hub system (ERP, ESB), real-time, cross-domain | Must migrate last or in place |
| **High** | 20-50 | Many upstream/downstream, mix of sync/async | Needs careful wave planning |
| **Medium** | 5-20 | Standard APIs, batch files, well-documented | Manageable with testing |
| **Low** | 1-5 | Simple point-to-point, SSO only | Quick to repoint |
| **None** | 0 | Standalone, no integrations | Easiest to migrate |

---

## Common Integration Patterns on AWS

| Pattern | AWS Service | Use Case |
|---------|------------|----------|
| API-to-API | Amazon API Gateway + Lambda | REST/HTTP integrations |
| Event-driven | Amazon EventBridge | Async event routing |
| File transfer | AWS Transfer Family (SFTP/FTPS) | Batch file exchanges |
| Message queue | Amazon SQS / SNS | Decoupled messaging |
| Streaming | Amazon Kinesis / MSK | Real-time data streams |
| ETL / batch | AWS Glue / Step Functions | Data transformation pipelines |
| SaaS-to-AWS | Amazon AppFlow | SaaS data sync (Salesforce, SAP, etc.) |

---

[← Previous: Section 06](./06-data.md) | [Back to README](../README.md) | [Next: Section 08 →](./08-licensing-cost.md)
