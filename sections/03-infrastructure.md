# 03 — Infrastructure

> 🟡 **MEDIUM PRIORITY** — Compute, database, storage, and hosting details for self-hosted apps.
> **28 Questions** | Pillar: Performance Efficiency, Reliability

---

## Purpose

Capture the technical infrastructure footprint for applications that are **self-hosted or managed-hosted**. Pure SaaS apps (vendor-hosted) can skip most of this section — focus on apps you need to migrate or replatform.

---

## Questions

| # | Question | WA Pillar | Applies To |
|---|----------|-----------|------------|
| 1 | What operating system and version? (Windows Server 2019, RHEL 8, SLES 15) | Perf | Self-Hosted |
| 2 | How many servers / VMs does this application use? | Perf | Self-Hosted |
| 3 | What is the CPU allocation per server? (vCPUs / cores) | Perf | Self-Hosted |
| 4 | What is the RAM allocation per server? | Perf | Self-Hosted |
| 5 | What is the average CPU utilization? Peak? | Perf | Self-Hosted |
| 6 | What is the average memory utilization? Peak? | Perf | Self-Hosted |
| 7 | What hypervisor / virtualization platform? (VMware, Hyper-V, KVM, bare metal) | Perf | Self-Hosted |
| 8 | What database engine and version? (Oracle 19c, SQL Server 2019, HANA 2.0, PostgreSQL 15) | Perf | Self-Hosted |
| 9 | What is the database size? (data + indexes + logs) | Perf | Self-Hosted |
| 10 | What is the database growth rate? (GB/month) | Perf | Self-Hosted |
| 11 | What IOPS does the database require? (read/write split) | Perf | Self-Hosted |
| 12 | What storage type is used? (SAN, NAS, local SSD, NFS, object) | Perf | Self-Hosted |
| 13 | What is the total storage capacity allocated vs. used? | Perf | Self-Hosted |
| 14 | What storage throughput is required? (MB/s) | Perf | Self-Hosted |
| 15 | What middleware / application server is used? (Tomcat, WebSphere, IIS, SAP NetWeaver) | Perf | Self-Hosted |
| 16 | What web server / reverse proxy? (Apache, Nginx, IIS, F5) | Perf | Self-Hosted |
| 17 | Is the application clustered? (active-active, active-passive) | Reliability | Self-Hosted |
| 18 | What load balancer is used? (F5, HAProxy, NLB, ALB) | Reliability | Self-Hosted |
| 19 | Is there shared storage? (NFS, CIFS, GlusterFS, EFS equivalent) | Perf | Self-Hosted |
| 20 | What backup solution is in place? (Veeam, Commvault, native, snapshots) | Reliability | Self-Hosted |
| 21 | What is the backup schedule and retention? | Reliability | Self-Hosted |
| 22 | Are backups tested regularly? Last successful restore date? | Reliability | Self-Hosted |
| 23 | Is there a DR environment? Where is it located? | Reliability | Self-Hosted |
| 24 | What is the network bandwidth between app tier and DB? | Perf | Self-Hosted |
| 25 | Are there GPU requirements? (ML workloads, rendering) | Perf | Self-Hosted |
| 26 | What licensing is tied to infrastructure? (per-core, per-socket, per-VM) | Cost | Self-Hosted |
| 27 | For SAP: What is the SAPS benchmark requirement? | Perf | SAP only |
| 28 | Is the infrastructure end-of-life or end-of-support? When? | Reliability | Self-Hosted |

---

## How to Use

1. **Skip this section entirely** for pure vendor-hosted SaaS (Salesforce, Jira, Slack, etc.)
2. For self-hosted apps: gather data from VMware vCenter, monitoring tools (Zabbix, Nagios, CloudWatch), or AWS Application Discovery Service
3. For managed hosting: request infrastructure specs from your MSP
4. Use this data to **right-size AWS target** (EC2 instance type, RDS size, EBS volume type)

---

## AWS Target Mapping Guide

| Current | AWS Target | Key Sizing Input |
|---------|-----------|-----------------|
| VMware VMs | EC2 instances | vCPU, RAM, utilization |
| Oracle / SQL Server DB | RDS or EC2 + self-managed | DB size, IOPS, licensing |
| SAP HANA | EC2 (HANA-certified instances) | SAPS, memory, storage |
| NFS / shared storage | EFS or FSx | Throughput, IOPS |
| SAN / block storage | EBS (gp3, io2) | IOPS, throughput, size |
| F5 load balancer | ALB / NLB | Protocol, SSL offload |
| Backup (Veeam, Commvault) | AWS Backup | RPO, retention |

---

[← Previous: Section 02](./02-application-portfolio.md) | [Back to README](../README.md) | [Next: Section 04 →](./04-security-compliance.md)
