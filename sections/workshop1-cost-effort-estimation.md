# 🎯 Workshop 1 — Cost & Effort Estimation Discovery

> **Purpose:** Gather all inputs required to produce an initial AWS running cost estimate + migration effort/manpower estimate per application.
>
> **Duration:** 2-3 hours per application group (batch similar apps together)
>
> **Attendees:** Application Owner, Technical Lead, Infra/Platform Team, Finance/Procurement, DevOps/Automation Lead
>
> **Output:** Per-app cost model (current vs AWS) + effort estimate (person-days) + automation assessment

---

## 📋 Workshop Agenda

| # | Section | Duration | Purpose |
|---|---------|----------|---------|
| A | Current State & Hosting Model | 30 min | What exists today, where it runs |
| B | AWS Target Architecture Preferences | 30 min | How they want to run on AWS |
| C | Compute, Storage & Database Sizing | 30 min | Right-sizing inputs for AWS pricing |
| D | Networking & Data Transfer | 15 min | Egress, connectivity, bandwidth |
| E | Licensing & Commercial | 20 min | License impact on AWS cost |
| F | Operations & Manpower | 20 min | Team effort, ongoing run cost |
| G | CI/CD & Automation | 20 min | Existing automation, target state |
| H | Migration Effort & Timeline | 15 min | One-time migration cost & duration |

---

## A — Current State & Hosting Model

> *Establishes the baseline — you cannot estimate target cost without knowing what exists today.*

| # | Question | Cost Impact | Why It Matters |
|---|----------|-------------|----------------|
| A1 | What is the application name and business function? | — | Context for all decisions |
| A2 | Where is it hosted today? (on-prem DC / colo / MSP / vendor SaaS / hybrid) | High | Determines migration complexity |
| A3 | What is the current monthly/annual infrastructure cost? (compute + storage + network + support) | High | Baseline for cost comparison |
| A4 | What is the current monthly/annual licensing cost? | High | May change on AWS |
| A5 | What is the current monthly/annual people cost to operate this app? (FTEs or hours) | High | Manpower baseline |
| A6 | What is the current uptime SLA? (99.9%, 99.95%, 99.99%) | High | Drives HA architecture = more cost |
| A7 | How many environments exist? (Prod, DR, UAT, Staging, Dev, Sandbox) | High | Each environment = cost multiplier |
| A8 | What is the environment sizing ratio? (e.g., Dev = 50% of Prod, UAT = 75%) | High | Non-prod cost calculation |
| A9 | Is the application stateful or stateless? | Medium | Affects architecture choices |
| A10 | What are the peak usage hours? Is it 24/7 or business-hours only? | High | Scheduling savings (stop non-prod nights/weekends) |

---

## B — AWS Target Architecture Preferences

> *How does the customer want to run this on AWS? This shapes the entire cost model.*

| # | Question | Cost Impact | Why It Matters |
|---|----------|-------------|----------------|
| B1 | What migration strategy is preferred? (Rehost / Replatform / Refactor / Replace) | High | Rehost = fast/similar cost; Refactor = cheaper long-term but high effort |
| B2 | Do you want managed services (RDS, ECS, Lambda) or self-managed (EC2 + install yourself)? | High | Managed = higher AWS cost but lower people cost |
| B3 | Do you want containers (ECS/EKS) or VMs (EC2) or serverless (Lambda/Fargate)? | High | Fundamentally different cost models |
| B4 | Do you want multi-AZ high availability? | High | Multi-AZ ≈ 2x compute cost |
| B5 | Do you need multi-region DR? Or single-region with multi-AZ is sufficient? | Very High | Multi-region = 2x+ cost |
| B6 | What is the target RTO/RPO? | High | Lower RTO/RPO = higher cost (hot standby vs cold) |
| B7 | Do you want auto-scaling or fixed capacity? | Medium | Auto-scaling saves cost but adds complexity |
| B8 | Should non-prod environments be always-on or scheduled? (stop nights/weekends) | High | Scheduling saves 60-70% on non-prod |
| B9 | Do you want spot instances for non-critical workloads? | Medium | Up to 90% savings but requires fault tolerance |
| B10 | What commitment level? (On-Demand / 1-year RI / 3-year RI / Savings Plans) | Very High | 3-year commitment = 40-60% savings |
| B11 | Do you want Graviton (ARM) instances where possible? | Medium | 20% cheaper than x86 equivalents |
| B12 | What storage tier strategy? (all SSD, tiered hot/warm/cold, lifecycle policies) | Medium | S3 tiering saves significantly on large data |
| B13 | Do you want a landing zone / shared services model or standalone account? | Medium | Shared services (Transit Gateway, centralized logging) spreads cost |
| B14 | What observability stack? (CloudWatch native / third-party like Datadog / Grafana) | Medium | Third-party = additional license cost |
| B15 | What backup strategy on AWS? (AWS Backup / native snapshots / third-party) | Medium | Retention period drives storage cost |

---

## C — Compute, Storage & Database Sizing

> *These numbers directly feed into AWS Pricing Calculator.*

| # | Question | Cost Impact | Why It Matters |
|---|----------|-------------|----------------|
| C1 | How many application servers? What CPU (vCPU) and RAM per server? | High | Direct EC2 instance type selection |
| C2 | How many web/proxy servers? What CPU and RAM? | Medium | ALB may replace need for web servers |
| C3 | How many database servers? What CPU, RAM, and storage? | High | Largest cost component often |
| C4 | What database engine? (Oracle, SQL Server, MySQL, PostgreSQL, HANA, MongoDB) | Very High | License cost varies enormously |
| C5 | What is the database size? (data + indexes + logs + temp) | High | Storage + IOPS cost |
| C6 | What are the IOPS requirements? (peak read + peak write) | High | gp3 vs io2 can be 3-10x cost difference |
| C7 | What storage is needed beyond database? (file shares, object storage, archive) | Medium | EBS vs EFS vs S3 vs FSx |
| C8 | What is the total storage size per environment? | High | Direct cost input |
| C9 | What is the monthly data growth rate? | Medium | Cost projection over time |
| C10 | Are there batch processing / GPU / HPC requirements? | High | GPU instances are expensive |
| C11 | What is the peak concurrent user count? | Medium | Affects instance sizing |
| C12 | What are the memory requirements? (in-memory caching, Redis, Memcached) | Medium | ElastiCache sizing |
| C13 | Are there message queue requirements? (SQS, RabbitMQ, Kafka) | Low-Medium | MSK is expensive; SQS is cheap |
| C14 | What container resource limits? (CPU/memory per container if containerized) | Medium | Fargate pricing is per vCPU/GB |
| C15 | For SAP: What is the SAPS requirement? | High | Determines EC2 instance family |

---

## D — Networking & Data Transfer

> *Data transfer is the hidden AWS cost that catches everyone off guard.*

| # | Question | Cost Impact | Why It Matters |
|---|----------|-------------|----------------|
| D1 | How much data egress per month? (data going OUT of AWS to internet/on-prem) | High | $0.09/GB adds up fast |
| D2 | How much inter-region data transfer? (if multi-region) | High | $0.02/GB between regions |
| D3 | How much inter-AZ data transfer? (between availability zones) | Medium | $0.01/GB — often overlooked |
| D4 | Is Direct Connect required? What bandwidth? (1Gbps, 10Gbps) | High | $0.30/hr for dedicated 1Gbps + data charges |
| D5 | Is a VPN sufficient or is Direct Connect mandatory? | Medium | VPN = $0.05/hr vs DX = $0.30/hr |
| D6 | How many VPN connections needed? | Low | $0.05/hr per connection |
| D7 | Is a CDN (CloudFront) needed? What volume? | Medium | Can reduce egress cost significantly |
| D8 | What NAT Gateway traffic volume? (private subnet → internet) | Medium | $0.045/GB + $0.045/hr — often surprises |
| D9 | Are there data transfer costs FROM current vendor to AWS during migration? | Medium | One-time but can be significant |
| D10 | What elastic IP / static IP requirements? | Low | Free if attached; $0.005/hr if idle |

---

## E — Licensing & Commercial

> *Licensing can make or break an AWS cost model. Oracle/SAP/Microsoft rules are complex.*

| # | Question | Cost Impact | Why It Matters |
|---|----------|-------------|----------------|
| E1 | Can you BYOL (Bring Your Own License) to AWS? | Very High | BYOL vs License-Included = 2-4x cost difference |
| E2 | What is the Oracle licensing model? (Named User Plus / Processor / AWS RDS License Included) | Very High | Oracle per-core on AWS can be catastrophic |
| E3 | What is the SQL Server edition? (Standard / Enterprise / Express / Web) | High | Enterprise on EC2 = $$$; RDS License Included is often cheaper |
| E4 | What is the SAP licensing situation? (S/4HANA license, HANA runtime, indirect access) | Very High | SAP on AWS has specific commercial terms |
| E5 | Do you have Microsoft License Mobility / Software Assurance? | High | Determines if Windows/SQL can move to AWS |
| E6 | What Red Hat / SUSE licenses exist? Can they move? | Medium | BYOL vs AWS marketplace RHEL pricing |
| E7 | Are there any third-party software licenses tied to physical hardware? | High | May need new cloud licenses |
| E8 | What monitoring/backup tool licenses exist? (Dynatrace, Commvault, etc.) | Medium | May be replaced by AWS-native (cost saving) |
| E9 | What is the contract status? (can you exit, or locked in?) | High | Locked contracts = dual cost during transition |
| E10 | Are there any vendor restrictions on running in public cloud? | High | Some vendors prohibit or charge extra |

---

## F — Operations & Manpower

> *People cost is often higher than infrastructure cost. Capture both ongoing and migration effort.*

| # | Question | Cost Impact | Why It Matters |
|---|----------|-------------|----------------|
| F1 | How many FTEs currently operate this application? (admin, DBA, infra, support) | High | Baseline people cost |
| F2 | What is the blended rate per resource? (fully loaded cost per person per day/month) | High | Effort × rate = people cost |
| F3 | Will managed services reduce operational headcount? | High | RDS vs self-managed DB saves DBA time |
| F4 | What is the L1/L2/L3 support model? (in-house, outsourced, vendor) | Medium | Outsourced support contracts |
| F5 | What is the expected ongoing AWS operational effort? (patching, monitoring, incident response) | High | Ongoing cost post-migration |
| F6 | Is there a dedicated DevOps/SRE team or will one be needed? | High | New team = new cost |
| F7 | What training is needed for the team to operate on AWS? | Medium | Training cost (AWS certs, hands-on labs) |
| F8 | Will you use AWS Managed Services (AMS) or operate yourself? | High | AMS = premium but lower headcount |
| F9 | What is the security operations model? (SOC team, MSSP, GuardDuty + automation) | Medium | MSSP contracts or internal SOC cost |
| F10 | What is the expected cost of 24/7 on-call coverage on AWS? | Medium | May increase or decrease vs current |
| F11 | Are there compliance/audit activities that require dedicated effort? | Medium | Audit prep, evidence collection |
| F12 | What is the application support model post-migration? (hypercare period, transition to BAU) | Medium | Temporary extra headcount during transition |

---

## G — CI/CD & Automation

> *Automation reduces ongoing effort cost but has upfront build cost. Assess what exists and what's needed.*

| # | Question | Cost Impact | Why It Matters |
|---|----------|-------------|----------------|
| G1 | Is there an existing CI/CD pipeline? What tool? (Jenkins, GitLab CI, GitHub Actions, Azure DevOps, AWS CodePipeline) | Medium | Reuse vs build new |
| G2 | Is infrastructure managed as code today? (Terraform, CloudFormation, CDK, Ansible, Puppet) | High | IaC = faster, repeatable, lower drift |
| G3 | If no IaC exists, do you want it on AWS? (adds upfront effort but reduces ongoing) | High | 5-15 days effort per app but saves operational cost |
| G4 | What is the deployment frequency? (daily, weekly, monthly, quarterly) | Medium | Higher frequency = more automation value |
| G5 | What automated testing exists? (unit, integration, performance, security) | Medium | Gaps mean manual testing effort per release |
| G6 | Is there automated provisioning of non-prod environments? | High | Saves 60-70% non-prod cost if environments are ephemeral |
| G7 | What configuration management is in place? (Ansible, SSM, Chef, Puppet, none) | Medium | Impacts patching and drift management effort |
| G8 | Is there automated backup and DR testing? | Medium | Manual DR tests = expensive people time |
| G9 | What observability automation exists? (auto-alerting, auto-remediation, runbook automation) | Medium | Reduces incident response effort |
| G10 | Is there automated scaling configured or will it need to be built? | Medium | Manual scaling = on-call overhead |
| G11 | What security automation exists? (automated scanning, SAST/DAST in pipeline, automated patching) | Medium | Reduces security operations effort |
| G12 | What is the estimated effort to build CI/CD on AWS if none exists? | High | One-time investment (typically 10-30 person-days per app) |
| G13 | Do you want GitOps? (ArgoCD, Flux) | Medium | Adds setup effort but reduces drift |
| G14 | What AWS automation services are preferred? (CodePipeline, CodeBuild, CodeDeploy, Step Functions) | Low | Service cost differences are small |
| G15 | Is there a platform engineering team that provides shared pipelines? | Medium | Shared = lower per-app effort |

---

## H — Migration Effort & Timeline

> *One-time costs to get from current state to AWS target state.*

| # | Question | Cost Impact | Why It Matters |
|---|----------|-------------|----------------|
| H1 | What is the estimated migration duration? (days/weeks/months per app) | High | Duration × team size = effort cost |
| H2 | How many people are needed for the migration? (architects, engineers, testers, PM) | High | Team composition |
| H3 | Is an external partner / SI needed? What is their rate? | Very High | SI rates = $1,500-$3,000/day |
| H4 | What is the testing effort? (functional, performance, integration, security, UAT) | High | Often 30-50% of total migration effort |
| H5 | Is a parallel-run period needed? (dual cost during transition) | High | Running both environments = temporary 2x cost |
| H6 | How long is the parallel run / hypercare? (2 weeks, 1 month, 3 months) | High | Longer = more cost |
| H7 | What data migration effort is needed? (volume, transformation, validation) | High | Large DBs can take days to transfer |
| H8 | What cutover downtime is acceptable? (zero, hours, weekend, week) | High | Zero-downtime migration is much more complex/expensive |
| H9 | What documentation needs to be created? (architecture, runbooks, SOPs) | Medium | Often overlooked effort |
| H10 | What AWS skills gap training is needed before migration starts? | Medium | Upfront investment |
| H11 | Are there migration tools/licenses needed? (AWS MGN, DMS, third-party tools) | Low-Medium | Mostly free/cheap but some third-party tools cost |
| H12 | What is the cost of rollback if migration fails? | Medium | Risk budget |

---

## 📊 Cost Estimation Output Template

After the workshop, produce this per application:

### Monthly AWS Running Cost Estimate

| Component | Service | Size/Config | Monthly Cost |
|-----------|---------|-------------|--------------|
| Compute (Prod) | EC2 / ECS / Lambda | e.g., 2x m6i.xlarge | $XXX |
| Compute (Non-Prod) | EC2 / ECS | e.g., 1x m6i.large (scheduled) | $XXX |
| Database (Prod) | RDS / Aurora / DynamoDB | e.g., db.r6g.xlarge Multi-AZ | $XXX |
| Database (Non-Prod) | RDS | e.g., db.r6g.large Single-AZ | $XXX |
| Storage | EBS / S3 / EFS | e.g., 500GB gp3 + 2TB S3 | $XXX |
| Networking | ALB / NAT GW / DX / VPN | e.g., ALB + 2 NAT GW | $XXX |
| Data Transfer | Egress + inter-AZ | e.g., 500GB/mo egress | $XXX |
| Backup | AWS Backup / Snapshots | e.g., 30-day retention | $XXX |
| Monitoring | CloudWatch / Grafana | e.g., custom metrics + logs | $XXX |
| Security | GuardDuty / WAF / KMS | e.g., WAF + GuardDuty | $XXX |
| Licensing | BYOL or License-Included | e.g., SQL Server Standard | $XXX |
| **TOTAL MONTHLY** | | | **$X,XXX** |
| **TOTAL ANNUAL** | | | **$XX,XXX** |

### Savings Opportunities

| Opportunity | Savings | Commitment |
|-------------|---------|-----------|
| Savings Plans (1-year) | -XX% | 1-year commitment |
| Savings Plans (3-year) | -XX% | 3-year commitment |
| Graviton instances | -20% | ARM compatibility required |
| Non-prod scheduling | -65% on non-prod | Automation needed |
| Spot for batch workloads | -70-90% | Fault tolerance required |
| S3 lifecycle policies | -XX% on storage | Data tiering rules |

### Migration Effort Estimate

| Phase | Activities | People | Days | Person-Days |
|-------|-----------|--------|------|-------------|
| Planning & Design | Architecture, sizing, IaC design | 2 | 5 | 10 |
| Build & Configure | IaC, CI/CD, infrastructure setup | 3 | 10 | 30 |
| Data Migration | DB migration, file sync, validation | 2 | 5 | 10 |
| Testing | Functional, perf, integration, UAT | 3 | 10 | 30 |
| Cutover & Go-Live | Cutover execution, DNS, validation | 4 | 2 | 8 |
| Hypercare | Post-migration monitoring & fixes | 2 | 10 | 20 |
| Documentation | Runbooks, architecture docs, SOPs | 1 | 5 | 5 |
| **TOTAL** | | | | **113 person-days** |

### Ongoing Operational Effort (Monthly)

| Activity | Effort (hours/month) | Role |
|----------|---------------------|------|
| Patching & maintenance | X hrs | Platform Engineer |
| Monitoring & alerting | X hrs | SRE / DevOps |
| Incident response | X hrs | L2/L3 Support |
| Backup & DR testing | X hrs | Platform Engineer |
| Security operations | X hrs | SecOps |
| Cost optimization reviews | X hrs | FinOps |
| CI/CD pipeline maintenance | X hrs | DevOps |
| **TOTAL** | **X hrs/month** | |

---

## ⚠️ Common Cost Traps — Don't Miss These

| Trap | Impact | Question to Ask |
|------|--------|-----------------|
| **Data transfer egress** | Can be 20-30% of bill | D1: How much data leaves AWS monthly? |
| **NAT Gateway charges** | $0.045/GB processed | D8: How much traffic goes through NAT? |
| **Multi-AZ database** | 2x DB cost | B4: Is Multi-AZ required? |
| **Oracle licensing on AWS** | Can 10x DB cost | E2: What Oracle licensing model? |
| **Always-on non-prod** | 40-60% waste | B8: Can non-prod be scheduled? |
| **Overprovisioned instances** | 30-50% waste | C1: What is actual utilization vs allocated? |
| **No Savings Plans** | Paying 40-60% more | B10: What commitment level? |
| **Forgotten DR environment** | 50-100% cost increase | B5: Do you need multi-region DR? |
| **Log/metric retention** | Grows silently | B14: What retention for logs and metrics? |
| **Parallel run period** | Temporary 2x cost | H5-H6: How long will both environments run? |
| **SI partner cost** | $1.5-3K/day/person | H3: Is external help needed? |
| **Training & certification** | $5-10K per person | H10: What AWS training needed? |

---

## ✅ Workshop Checklist

Before closing the workshop, confirm you have:

- [ ] Current monthly/annual cost (infrastructure + licensing + people)
- [ ] Target AWS architecture decision (managed vs self-managed, VM vs container vs serverless)
- [ ] Compute sizing per environment (vCPU, RAM, instance type)
- [ ] Database sizing (engine, size, IOPS, Multi-AZ yes/no)
- [ ] Storage requirements (type, size, growth rate)
- [ ] Data transfer estimates (egress, inter-AZ, inter-region)
- [ ] Licensing model confirmed (BYOL vs License-Included)
- [ ] Commitment preference (On-Demand vs 1yr vs 3yr Savings Plan)
- [ ] Non-prod scheduling decision (always-on vs scheduled)
- [ ] HA/DR architecture decision (Multi-AZ, Multi-Region, pilot light, warm standby, hot)
- [ ] CI/CD current state and target state documented
- [ ] IaC requirement (yes/no, tool choice)
- [ ] Team composition for migration (internal + external)
- [ ] Estimated migration duration per phase
- [ ] Parallel-run duration
- [ ] Ongoing operational FTE estimate
- [ ] Training requirements identified

---

**Author:** Ramandeep Chandna | August 2026
