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
| B | Application Type Classification | 20 min | SaaS vs COTS vs Custom — determines feasibility path |
| C | SaaS & Vendor-Hosted Applications | 30 min | Vendor migration options, AWS availability, offering review |
| D | COTS / Standalone Products | 25 min | Vendor AWS support, marketplace, deployment models |
| E | AWS Target Architecture Preferences | 30 min | How they want to run on AWS |
| F | Compute, Storage & Database Sizing | 30 min | Right-sizing inputs for AWS pricing |
| G | Networking & Data Transfer | 15 min | Egress, connectivity, bandwidth |
| H | Licensing & Commercial | 20 min | License impact on AWS cost |
| I | Operations & Manpower | 20 min | Team effort, ongoing run cost |
| J | CI/CD & Automation | 20 min | Existing automation, target state |
| K | Migration Effort & Timeline | 15 min | One-time migration cost & duration |
| L | Compliance, Regulatory, Identity & Hidden Dependencies | 30 min | PII, data residency, AD, shared drives, cache, transaction logs, licenses |
| M | Application Interdependency & Integration Mapping | 25 min | Cross-app dependencies, shared DB/cache/files, tight coupling |
| N | Wave / Sequencing Strategy | 20 min | Migration order, clusters, critical path, hybrid state cost |
| O | Business Process Criticality | 15 min | Process gates, revenue impact, calendar-critical periods |
| P | Testing Environment Lifecycle & Data Refresh | 20 min | Env lifecycle, ephemeral vs permanent, data masking, perf testing |

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

## B — Application Type Classification

> *Before estimating cost, classify each app — the cost/effort model is fundamentally different for SaaS vs COTS vs Custom.*

| # | Question | Cost Impact | Why It Matters |
|---|----------|-------------|----------------|
| B1 | What category is this application? (Pure SaaS / COTS self-hosted / COTS vendor-hosted / Custom-built / Legacy unknown) | Very High | Determines the entire estimation approach |
| B2 | Is this a vendor-managed SaaS (you have no infrastructure access) or self-managed on your infrastructure? | Very High | SaaS = no infra to migrate; COTS self-hosted = full migration |
| B3 | Is the application subscription-based (SaaS) or perpetual-license (COTS)? | High | Affects ongoing cost model |
| B4 | Does the vendor offer an AWS-hosted version or deployment option? | Very High | If yes, may be a simple subscription change |
| B5 | Is this application available on AWS Marketplace? | High | Marketplace = simplified procurement + potential EDP credits |
| B6 | Is this a standalone application or part of a larger suite? (e.g., SAP S/4 is part of SAP ecosystem) | High | Suite dependencies affect migration order |
| B7 | Who manages upgrades and patches? (Vendor fully / Shared / Customer fully) | Medium | Responsibility model on AWS |
| B8 | What is the deployment model? (Multi-tenant SaaS / Single-tenant SaaS / Dedicated instance / On-prem install) | High | Single-tenant and dedicated are more migratable |

### Application Type Decision Tree

```
Is the application vendor-hosted with NO infrastructure access?
├── YES → Pure SaaS (Section C applies)
│   ├── Does vendor offer AWS-region hosting? → Check vendor migration options
│   ├── Need to move data to AWS? → Data portability assessment
│   └── Integration with AWS workloads? → API/connectivity assessment
│
└── NO → You manage infrastructure
    ├── Is it a commercial product (COTS)?
    │   ├── YES → COTS on your infra (Section D applies)
    │   │   ├── Does vendor support AWS deployment? → Vendor certification check
    │   │   ├── Available on AWS Marketplace? → Marketplace deployment
    │   │   └── Vendor has no AWS story? → Risk assessment needed
    │   │
    │   └── NO → Custom-built / Legacy (Sections E-K apply directly)
    └──
```

---

## C — SaaS & Vendor-Hosted Applications

> *For applications like SAP S/4HANA Cloud, Salesforce, ServiceNow, Workday, Dynamics 365, Jira Cloud — where the vendor manages everything. Key question: What are the vendor's AWS offerings and can we influence where it runs?*

| # | Question | Cost Impact | Why It Matters |
|---|----------|-------------|----------------|
| **Vendor Offering Review** | | | |
| C1 | Does the vendor offer hosting on AWS? (e.g., SAP on AWS, ServiceNow on AWS) | Very High | If yes, migration may be a commercial conversation, not technical |
| C2 | What deployment options does the vendor offer? (Public cloud choice / Dedicated tenancy / RISE with SAP / vendor's own DC) | Very High | Determines if you can choose AWS |
| C3 | For SAP: Is RISE with SAP an option? What is the commercial model? (subscription, managed by SAP on hyperscaler) | Very High | RISE = SAP manages infra on AWS; different cost model |
| C4 | For SAP: Is it SAP S/4HANA Cloud Public Edition (multi-tenant) or Private Edition (single-tenant on hyperscaler)? | Very High | Public = no infra choice; Private = you can specify AWS |
| C5 | Does the vendor allow region selection? Can you choose an AWS region? | High | Data residency + latency |
| C6 | What is the vendor's roadmap? Are they moving to AWS or another cloud? | Medium | Future-proofing |
| C7 | Is there a vendor-specific migration program? (e.g., SAP RISE Migration, Salesforce platform migrations) | High | Vendor may cover migration cost/effort |
| C8 | Does the vendor have AWS-specific certifications or partnerships? (ISV partner, AWS Outposts support) | Medium | Maturity indicator |
| **Feasibility Assessment** | | | |
| C9 | Can you contractually request AWS hosting from this vendor? | Very High | Some vendors lock you to their cloud/DC |
| C10 | Is there a cost difference between vendor-hosted on AWS vs other options? | High | Premium for cloud choice? |
| C11 | What SLA does the vendor provide if hosted on AWS vs their own DC? | Medium | SLA may differ by hosting option |
| C12 | Can data be kept in a specific AWS region (data sovereignty)? | High | Regulatory requirement |
| C13 | What connectivity options exist between the SaaS app and your AWS environment? (PrivateLink, VPN, public API, dedicated interconnect) | High | Private connectivity has cost |
| C14 | Does the vendor support AWS PrivateLink for private connectivity? | Medium | Secure + no egress cost |
| **Integration & Data** | | | |
| C15 | What APIs does the vendor expose? (REST, SOAP, OData, GraphQL, proprietary) | Medium | Integration effort |
| C16 | Can you use AWS services to integrate? (EventBridge partner events, AppFlow connectors, Transfer Family) | Medium | Native integration = less custom code |
| C17 | What is the data export capability? (full export, incremental, real-time CDC, API only) | High | Affects data strategy and DR |
| C18 | Is there vendor lock-in risk? What happens if you need to leave? | High | Exit cost planning |
| C19 | What is the vendor's backup/DR responsibility vs yours? | Medium | Shared responsibility clarity |
| C20 | Are there API rate limits that affect integration volume? | Medium | May need premium API tier = cost |
| **Cost Specific** | | | |
| C21 | What is the current SaaS subscription cost? (annual, per-user, per-transaction) | High | Current baseline |
| C22 | Would moving to vendor-on-AWS change the subscription price? | High | Sometimes more expensive |
| C23 | Are there additional charges for AWS-hosted option? (dedicated tenancy premium, region premium) | High | Hidden SaaS cost |
| C24 | What integration/middleware licensing is needed? (SAP CPI, MuleSoft, Informatica) | High | Integration layer cost |
| C25 | Is there a migration/transition fee from vendor to move to AWS-hosted? | Medium | One-time cost |

### SaaS Vendor AWS Availability Quick Reference

| Vendor / Product | AWS Offering Available? | Migration Path |
|-----------------|------------------------|----------------|
| **SAP S/4HANA** | Yes — RISE with SAP on AWS, BTP on AWS | RISE = vendor-managed on AWS; Private Edition = customer choice |
| **SAP ECC** (self-hosted) | Yes — customer-managed on EC2 | Full infrastructure migration (Section E-K) |
| **Salesforce** | Hyperforce (AWS available in select regions) | Commercial switch; data residency choice |
| **ServiceNow** | Runs on AWS in select instances | Request AWS-hosted instance |
| **Workday** | Multi-tenant, no region choice for customer | Retain; integrate via API |
| **Oracle Cloud (Fusion)** | Oracle Cloud only (OCI) | Cannot move to AWS; integrate only |
| **Microsoft Dynamics 365** | Azure-only (Microsoft cloud) | Cannot move to AWS; integrate only |
| **Jira / Confluence Cloud** | Atlassian-managed (on AWS already) | Already on AWS; no action needed |
| **Slack** | Already runs on AWS | Already on AWS; no action needed |

---

## D — COTS / Standalone Products (Self-Hosted)

> *For commercial off-the-shelf software that YOU deploy on YOUR infrastructure — e.g., SAP ECC on your servers, Oracle E-Business Suite, IBM MQ, TIBCO, SAS, custom ERP packages. Key question: Does the vendor certify/support running on AWS?*

| # | Question | Cost Impact | Why It Matters |
|---|----------|-------------|----------------|
| **Vendor AWS Support** | | | |
| D1 | Does the vendor officially support / certify running on AWS? | Very High | Unsupported = risk of losing vendor support |
| D2 | Is the product listed on AWS Marketplace? What edition/version? | High | Marketplace = easier procurement, pre-configured AMI |
| D3 | Are there vendor-published AWS reference architectures? | Medium | Saves design effort; proven patterns |
| D4 | What AWS instance types are certified by the vendor? (e.g., SAP-certified EC2 instances) | High | Limits instance choice; may be expensive instances |
| D5 | Are there AWS-specific technical requirements from the vendor? (e.g., HANA needs specific EBS types, Oracle needs dedicated hosts) | Very High | Vendor requirements override cost optimization |
| D6 | Does the vendor charge differently for cloud deployment? (cloud tax, different licensing for AWS) | Very High | Oracle, IBM, SAP all have cloud-specific licensing rules |
| D7 | Is the vendor's support contract valid on AWS? Or do you need a new contract? | High | Support gaps during migration |
| D8 | Does the vendor provide migration tooling for AWS? (e.g., SAP DMO, Oracle RMAN to RDS) | Medium | Reduces migration effort |
| **Deployment & Architecture** | | | |
| D9 | What is the current architecture? (monolithic, 3-tier, microservices, distributed) | High | Affects AWS target design |
| D10 | What OS does the vendor require? (Windows only, RHEL only, SLES only, any Linux) | Medium | License cost per OS; Graviton may not be supported |
| D11 | What database does the vendor require? (Oracle only, SQL Server, HANA, vendor-specific) | Very High | DB license is often largest cost |
| D12 | Can the vendor's required DB be replaced with AWS-managed? (Oracle → RDS Oracle, SQL Server → RDS SQL) | High | RDS License-Included vs BYOL |
| D13 | What middleware is required? (WebSphere, WebLogic, JBoss, IIS, vendor-specific) | Medium | Each has licensing implications |
| D14 | Does the product support containerization? (can it run in Docker/ECS/EKS) | High | Containers = better density, lower cost |
| D15 | Does the product support auto-scaling or is it fixed capacity? | Medium | Fixed = overprovisioned; auto-scale = cost-efficient |
| D16 | What HA/clustering does the vendor support on AWS? (active-active, active-passive, vendor-specific clustering) | High | Determines Multi-AZ architecture |
| **Migration Feasibility** | | | |
| D17 | Has anyone migrated this product to AWS before? (vendor case studies, AWS partner references) | Medium | Proven path reduces risk/effort |
| D18 | Are there known blockers to running on AWS? (hardware dongles, proprietary storage, specific network requirements) | Very High | Blockers may make migration impossible |
| D19 | Are there hardcoded dependencies on physical infrastructure? (MAC address licensing, USB dongles, specific NIC drivers) | High | May block migration entirely |
| D20 | What is the upgrade path? Should you upgrade BEFORE or AFTER migration? | High | Migrate then upgrade or combine into one event |
| D21 | Is the vendor sunsetting this product? Is there a SaaS replacement? | Very High | If sunsetting → Replace strategy instead of Rehost |
| D22 | Does the vendor offer a managed service on AWS? (vendor manages on your AWS account) | High | Like COTS-as-a-Service — vendor ops, your infra |

### COTS on AWS — Common Scenarios

| Product | AWS Support? | Key Consideration | Typical Strategy |
|---------|-------------|-------------------|-----------------|
| **SAP ECC / S/4HANA (on-prem)** | ✅ Fully certified | Must use SAP-certified instance types; HANA memory sizing | Rehost to EC2 (certified instances) |
| **Oracle E-Business Suite** | ✅ Supported | Oracle per-core licensing on AWS is expensive; consider Dedicated Hosts | Rehost (Dedicated Hosts for BYOL) |
| **Oracle Database** | ✅ RDS or EC2 | BYOL needs Dedicated Hosts; License-Included = RDS only | Replatform (RDS) or Rehost (EC2 BYOL) |
| **Microsoft SQL Server** | ✅ RDS or EC2 | SA required for BYOL; License-Included via RDS is simpler | Replatform (RDS License-Included) |
| **IBM MQ** | ✅ On EC2 / Amazon MQ | Amazon MQ supports IBM MQ protocol | Replatform (Amazon MQ) |
| **TIBCO / MuleSoft** | ✅ On EC2 or ECS | License mobility needed | Rehost or Replace (with EventBridge/Step Functions) |
| **SAS** | ✅ AWS Marketplace | Cloud-specific licensing | Replatform (AWS Marketplace edition) |
| **Informatica** | ✅ IDMC on AWS | Cloud edition available | Replace (IDMC cloud-native) |
| **Custom ERP / legacy** | ⚠️ Varies | No vendor = no certification needed; but also no support | Rehost or Replace |

### Vendor Feasibility Assessment Checklist

Before estimating cost for any COTS product on AWS:

- [ ] Vendor officially supports AWS deployment (get it in writing)
- [ ] Certified instance types identified (limits cost optimization)
- [ ] Licensing model on AWS confirmed (BYOL rules, cloud licensing changes)
- [ ] Vendor support contract valid on AWS (no support gap)
- [ ] No hardware-specific blockers (dongles, proprietary storage)
- [ ] Database requirement confirmed (can it use RDS or must be self-managed?)
- [ ] HA/DR approach validated with vendor (Multi-AZ supported?)
- [ ] Migration tooling available (vendor tools, partner tools, or manual)
- [ ] Reference architectures / case studies exist

---

## E — AWS Target Architecture Preferences

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

## F — Compute, Storage & Database Sizing

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

## G — Networking & Data Transfer

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

## H — Licensing & Commercial

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

## I — Operations & Manpower

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

## J — CI/CD & Automation

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

## K — Migration Effort & Timeline

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

---

## L — Compliance, Regulatory, Identity & Hidden Dependencies

> *These are the items that silently inflate cost, block migration, or force expensive architecture decisions. Miss any of these and your estimate will be WRONG.*

### L1 — Compliance & Regulatory Requirements

| # | Question | Cost Impact | Why It Matters |
|---|----------|-------------|----------------|
| L1.1 | What regulatory frameworks apply? (GDPR, HIPAA, PCI-DSS, SOX, FISMA, FedRAMP, DORA, MAS, APRA) | Very High | Each framework may require specific controls = additional services cost |
| L1.2 | Is there PII (Personally Identifiable Information) stored? What types? (names, SSN, DOB, addresses, health records, biometrics) | Very High | PII requires encryption, DLP, audit logging, access control — all add cost |
| L1.3 | Is there PHI (Protected Health Information)? HIPAA BAA required? | Very High | HIPAA on AWS requires BAA-eligible services only (limits choices) |
| L1.4 | Is there PCI cardholder data? What PCI-DSS level? | Very High | PCI scoping requires isolated VPC, WAF, logging, quarterly scans = cost |
| L1.5 | Is there financial / SOX-regulated data? Audit trail requirements? | High | Immutable logs, separation of duties, CloudTrail = cost |
| L1.6 | **Must this application operate in a SINGLE specific AWS region?** (data sovereignty / regulatory mandate) | Very High | Single-region eliminates multi-region DR; limits HA choices |
| L1.7 | What country/jurisdiction must data reside in? (e.g., India-only, EU-only, Australia-only, UAE-only) | Very High | Limits region selection; some regions are more expensive |
| L1.8 | Is cross-border data transfer allowed? Under what mechanism? (SCCs, BCRs, adequacy decision) | High | Cross-region replication for DR may be prohibited = single-region DR only |
| L1.9 | Are there data classification labels applied? (Restricted, Confidential, Internal, Public) | Medium | Classification drives encryption, access control, and monitoring requirements |
| L1.10 | Is data masking / tokenization required in non-prod environments? | High | Amazon Macie, DMS masking, or third-party tools = additional cost |
| L1.11 | What audit / evidence requirements exist? (who accessed what, when, retention) | High | CloudTrail, Config Rules, Access Analyzer, Security Hub = cost |
| L1.12 | Is there a mandatory security baseline / hardening standard? (CIS Benchmarks, NIST 800-53, IRAP) | Medium | Compliance automation (Security Hub, Config conformance packs) = cost |
| L1.13 | Are there industry-specific regulations? (banking: operational resilience; healthcare: consent management; government: IL4/IL5) | High | May force GovCloud, dedicated tenancy, or specific services |
| L1.14 | Is a third-party audit / certification required on AWS? (SOC2 Type II, ISO 27001 for YOUR environment) | High | Audit prep effort + tooling for evidence collection |
| L1.15 | What is the data retention period required by regulation? (7 years financial, 6 years GDPR proof, etc.) | High | Long retention = significant storage cost (S3 Glacier, log archives) |
| L1.16 | Is there a legal hold / e-discovery requirement? | Medium | S3 Object Lock, Vault Lock = cost |
| L1.17 | What encryption key management is required? (AWS-managed, CMK, CloudHSM, external KMS) | High | CloudHSM = $1.50/hr per device; CMK = $1/month per key |
| L1.18 | Is there a DLP (Data Loss Prevention) requirement? | Medium | Amazon Macie = cost per GB scanned |

### L2 — Identity, SSO & Active Directory

| # | Question | Cost Impact | Why It Matters |
|---|----------|-------------|----------------|
| L2.1 | What is the current Identity Provider? (On-prem Active Directory, Azure AD/Entra ID, Okta, Ping, OneLogin) | High | Determines AWS identity architecture |
| L2.2 | **Is there an on-premises Active Directory that applications depend on?** | Very High | Requires AD Connector or AWS Managed Microsoft AD or self-managed DC on EC2 |
| L2.3 | Do applications require domain-joined servers? (Windows AD domain membership) | Very High | AWS Managed Microsoft AD = ~$144/month (Standard) or ~$580/month (Enterprise) |
| L2.4 | Is Kerberos authentication required? (Windows integrated auth, SQL Server Windows auth) | High | Requires AWS Managed AD or self-managed AD on EC2 |
| L2.5 | Is LDAP bind used for application authentication? (direct LDAP queries to AD) | High | AD Connector or Managed AD needed; network latency consideration |
| L2.6 | What SSO protocol? (SAML 2.0, OIDC, WS-Federation, proprietary) | Medium | AWS IAM Identity Center is free; but integration effort varies |
| L2.7 | How many users need SSO access to AWS console + applications? | Medium | IAM Identity Center is free but IdP setup = effort cost |
| L2.8 | Is there multi-forest or multi-domain AD trust? | High | Complex AD topology = more expensive AWS AD setup + trust config |
| L2.9 | Are there Group Policy Objects (GPOs) that applications depend on? | Medium | Must replicate GPO logic in AWS (Systems Manager, custom scripts) |
| L2.10 | Is AD used for DNS resolution internally? (AD-integrated DNS zones) | Medium | Route 53 Resolver + Outbound/Inbound endpoints = cost (~$0.125/hr/endpoint/direction) |
| L2.11 | What is the network latency requirement from AWS to on-prem AD? | High | AD auth is latency-sensitive; may force Direct Connect over VPN |
| L2.12 | Is there a plan to migrate to cloud-native identity? (Azure AD/Entra, AWS IAM Identity Center) | Medium | Migration effort vs maintaining hybrid AD long-term |
| L2.13 | Are there service accounts in AD used by applications? How many? | Medium | Must be replicated or converted to AWS Secrets Manager |
| L2.14 | Is there Radius/NPS for VPN or Wi-Fi authentication that apps depend on? | Low-Medium | May need Network Access Analyzer or third-party on EC2 |

### L3 — On-Premises Shared Drives / File Services

| # | Question | Cost Impact | Why It Matters |
|---|----------|-------------|----------------|
| L3.1 | **Are there on-prem shared drives / file shares that applications depend on?** (SMB/CIFS, NFS, DFS) | Very High | FSx for Windows or EFS = significant ongoing cost |
| L3.2 | What protocol is used? (SMB/CIFS for Windows, NFS for Linux, DFS Namespaces) | High | SMB → FSx for Windows ($$$); NFS → EFS ($$); DFS → complex |
| L3.3 | What is the total size of shared drives used by this application? | High | Direct storage cost input |
| L3.4 | How many applications / users share the same file system? | High | Shared dependency = must migrate together or build bridge |
| L3.5 | What access patterns? (random read/write, sequential, mostly read, archival) | Medium | FSx SSD vs HDD; EFS modes; determines tier and cost |
| L3.6 | What throughput / IOPS requirements for file shares? | High | FSx SSD vs HDD; EFS bursting vs provisioned = 3-5x cost difference |
| L3.7 | Are there AD-based file permissions (ACLs) on shared drives? | High | FSx for Windows preserves ACLs; EFS/S3 do not |
| L3.8 | Do users access file shares via mapped drives (Z:\, H:\, network paths)? | Medium | FSx for Windows + AD required to maintain user experience |
| L3.9 | Is DFS Replication or DFS Namespaces used across sites? | Medium | Multi-AZ FSx or DataSync replication = cost |
| L3.10 | Can any file data be moved to S3 instead of block file shares? | High | S3 is 10-50x cheaper than FSx/EFS for archival data |
| L3.11 | Are there print servers that depend on file shares? | Low | May need FSx or EC2-based print server |
| L3.12 | What is the data growth rate for file shares? (GB/month) | Medium | Storage cost projection |

### L4 — Database Transaction Logs, Replication & Dependencies

| # | Question | Cost Impact | Why It Matters |
|---|----------|-------------|----------------|
| L4.1 | **Are database transaction logs (redo logs, WAL, binlog) used by OTHER systems?** (log shipping, CDC, replication subscribers) | Very High | Downstream consumers must be migrated or replicated |
| L4.2 | Is there SQL Server log shipping or Always-On Availability Groups? | High | Multi-AZ RDS handles HA; but cross-region/log subscribers = extra cost |
| L4.3 | Is Oracle Data Guard / Active Data Guard in use? Sync or async? | Very High | Active Data Guard license on AWS = expensive; or replace with DMS |
| L4.4 | Is Oracle GoldenGate or Streams in use? | Very High | GoldenGate licensing on AWS is costly; consider DMS CDC alternative |
| L4.5 | Is MySQL/PostgreSQL streaming replication in use? How many replicas? | Medium | RDS Read Replicas = per-replica cost |
| L4.6 | Is SAP HANA System Replication (HSR) configured? Sync or async? | Very High | Multi-AZ HANA = second full-size memory-optimized instance = huge cost |
| L4.7 | Are transaction logs shipped to a data warehouse or analytics platform? (CDC to Redshift, Snowflake, data lake) | High | DMS CDC or native CDC on AWS = ongoing compute cost |
| L4.8 | What is the transaction log volume per day? (GB) | Medium | Storage cost for log retention + I/O throughput |
| L4.9 | Are there point-in-time recovery requirements? How far back? | Medium | RDS automated backups (up to 35 days free); beyond = manual + S3 cost |
| L4.10 | Is there a reporting / read-only replica? How is it used? | High | Read replica = additional instance cost (can be smaller) |
| L4.11 | Are there ETL jobs that read transaction logs? (Informatica CDC, Attunity, Debezium) | High | Must replicate on AWS; tool licensing + compute cost |
| L4.12 | Is database mirroring used for any failover? | Medium | RDS Multi-AZ replaces this but at Multi-AZ pricing |

### L5 — Cache, In-Memory & Session Dependencies

| # | Question | Cost Impact | Why It Matters |
|---|----------|-------------|----------------|
| L5.1 | **Is there a caching layer that this application depends on?** (Redis, Memcached, Hazelcast, Apache Ignite, Ehcache, Varnish, Windows AppFabric) | High | ElastiCache / MemoryDB = significant cost based on node size |
| L5.2 | What cache technology is used? (Redis, Memcached, Hazelcast, vendor-specific) | High | ElastiCache Redis vs Memcached vs MemoryDB — different pricing |
| L5.3 | What is the total cache memory size requirement? | High | Directly determines node type and count = cost |
| L5.4 | Is the cache clustered / distributed? How many nodes? | High | Multi-node cluster = cost × node count |
| L5.5 | Is cache data persistence required? (survives restart / failover) | Medium | MemoryDB = durable (more $); ElastiCache = ephemeral (less $) |
| L5.6 | **Is Redis or Memcached licensed separately?** (Redis Enterprise, Hazelcast Enterprise, GigaSpaces) | Very High | Enterprise license + infra cost; or switch to AWS-managed (free tier vs enterprise features) |
| L5.7 | What is the cache hit ratio? Is the cache actually effective? | Medium | Low hit rate → may not need expensive large cache |
| L5.8 | Is HTTP session state stored in cache? (sticky sessions, session replication) | Medium | Affects HA design; ElastiCache for session store = cost |
| L5.9 | Is the cache shared across multiple applications? | High | Shared = migration dependency; cost attribution complexity |
| L5.10 | Can the application function WITHOUT the cache? (degraded mode) Or is cache a HARD dependency? | High | Hard dependency = cache must be Multi-AZ (replication) = 2x cost |
| L5.11 | What is the latency requirement for cache access? (<1ms needed?) | Medium | Same-AZ placement or DAX; sub-ms may force specific instance types |
| L5.12 | **Are there in-memory database dependencies?** (SAP HANA, VoltDB, SingleStore/MemSQL, Redis as primary DB) | Very High | In-memory DBs require memory-optimized instances (r6g/x2idn) = very expensive |
| L5.13 | What is the eviction policy? (LRU, LFU, TTL) Does the app handle eviction gracefully? | Low | Architecture consideration for right-sizing |
| L5.14 | Is Redis used for pub/sub, queues, or streams (beyond caching)? | Medium | May need larger cluster or separate Redis for non-cache workloads |

### L6 — Licensing for Supporting Infrastructure & Services

| # | Question | Cost Impact | Why It Matters |
|---|----------|-------------|----------------|
| L6.1 | Is there a separate caching license? (Redis Enterprise, Hazelcast Enterprise, GigaSpaces XAP) | High | BYOL or switch to AWS-managed open-source |
| L6.2 | Is there a separate middleware license? (IBM WebSphere, Oracle WebLogic, TIBCO, MuleSoft) | High | Per-core licensing may change on AWS |
| L6.3 | Is there a monitoring/APM tool license? (Dynatrace, AppDynamics, New Relic, Splunk) | High | Per-host pricing changes with cloud scale; consider CloudWatch/X-Ray |
| L6.4 | Is there a backup tool license? (Commvault, Veeam, Veritas NetBackup, Cohesity) | Medium | AWS Backup may replace = cost saving |
| L6.5 | Is there an antivirus / EDR license? (CrowdStrike, Trend Micro, Symantec, Carbon Black) | Medium | Per-instance; marketplace options may be cheaper |
| L6.6 | Is there a private CA / certificate management license? (Venafi, DigiCert, internal PKI) | Medium | AWS Private CA = $400/month/CA; ACM public certs = free |
| L6.7 | Is there a secrets management tool license? (CyberArk, HashiCorp Vault Enterprise, Thycotic) | Medium | AWS Secrets Manager = $0.40/secret/month; may replace expensive tools |
| L6.8 | Is there a WAF / DDoS license? (F5, Imperva, Akamai) | High | AWS WAF + Shield vs third-party; significant cost difference |
| L6.9 | Is there a load balancer license? (F5 BIG-IP, Citrix ADC/NetScaler, HAProxy Enterprise) | High | AWS ALB/NLB replaces; but F5 features may require AWS Marketplace F5 |
| L6.10 | **Are there per-core / per-CPU licenses that MULTIPLY on AWS?** | Very High | Oracle, IBM DB2, SAP — uncontrolled licensing can 5-10x cost |
| L6.11 | Are there data integration / ETL tool licenses? (Informatica PowerCenter, Talend, DataStage) | High | AWS Glue may replace; or BYOL on EC2 |
| L6.12 | Is there a job scheduler license? (Control-M, AutoSys, Tivoli) | Medium | AWS Step Functions / EventBridge Scheduler may replace |

---

---

## M — Application Interdependency & Integration Mapping (Cross-App)

> *This is NOT per-app integration (covered in Section 07). This is the CROSS-APPLICATION view — which apps are coupled together and CANNOT be migrated independently. This is the #1 reason migrations fail or stall.*

### M1 — Shared Infrastructure Dependencies

| # | Question | Cost Impact | Why It Matters |
|---|----------|-------------|----------------|
| M1.1 | **Which applications share the same database instance?** (multi-schema, shared DB server) | Very High | Shared DB = apps MUST migrate together or need temporary bridge |
| M1.2 | Which applications share the same cache cluster? (shared Redis/Memcached) | High | Cache dependency = co-migration or dual-write during transition |
| M1.3 | Which applications share the same file system / shared drive? | High | Shared NFS/SMB = migrate together or use DataSync bridge |
| M1.4 | Which applications share the same middleware / ESB / integration platform? | High | ESB migration = all consumers affected simultaneously |
| M1.5 | Which applications share the same Active Directory / identity service? | High | AD migration affects ALL dependent apps at once |
| M1.6 | Which applications share the same message queue / event bus? | Medium | Queue consumers must be repointed together or bridged |
| M1.7 | Which applications share the same API gateway or reverse proxy? | Medium | Gateway repointing affects all backend services |
| M1.8 | Are there any database links (dblinks) between applications? | Very High | Hard coupling — apps must be same-region or link breaks |

### M2 — Tight Coupling & Real-Time Dependencies

| # | Question | Cost Impact | Why It Matters |
|---|----------|-------------|----------------|
| M2.1 | Which applications have synchronous (real-time) API calls between them? | Very High | Latency will increase if one is on-prem and one is on AWS during migration |
| M2.2 | What is the maximum acceptable latency between these coupled apps? (<5ms, <50ms, <200ms) | High | Sub-10ms may REQUIRE co-migration to same AZ |
| M2.3 | Are there distributed transactions (2-phase commit) spanning multiple apps? | Very High | Must migrate together or redesign transaction model |
| M2.4 | Which applications call each other in a synchronous chain? (A → B → C → D) | High | Chain breaks if any link crosses cloud/on-prem boundary with high latency |
| M2.5 | Are there any file-based integrations with tight timing dependencies? (file must arrive within X minutes) | Medium | Timing may break during hybrid state |
| M2.6 | Is there session affinity / shared session state between applications? | High | Session sharing across cloud boundary = complex |

### M3 — Integration Topology Mapping

| # | Question | Cost Impact | Why It Matters |
|---|----------|-------------|----------------|
| M3.1 | **Draw the integration map: which apps talk to which apps?** (visual topology) | — | Foundation for wave planning |
| M3.2 | Which application is the "hub" with the most connections? (highest fan-out) | High | Hub app = migrate last (most complex) or first (unblocks everything) |
| M3.3 | Are there circular dependencies? (A → B → C → A) | High | Circular = must migrate entire cluster together |
| M3.4 | Which integrations can tolerate a hybrid state? (some on AWS, some on-prem during transition) | Medium | Determines if parallel-run is feasible |
| M3.5 | What is the "blast radius" if this app goes down? How many other apps are affected? | High | High blast radius = more testing, more risk, more effort |
| M3.6 | Are there any external third-party integrations that cannot be changed? (partner APIs, government systems, bank gateways) | Medium | Fixed endpoints = migration must work around them |

---

## N — Wave / Sequencing Strategy (Cross-App Migration Order)

> *Individual app assessment is necessary but not sufficient. You must determine WHICH APPS MOVE TOGETHER and in WHAT ORDER. Get this wrong and you'll have apps stranded in a broken hybrid state.*

### N1 — Migration Grouping & Clusters

| # | Question | Cost Impact | Why It Matters |
|---|----------|-------------|----------------|
| N1.1 | Based on dependency mapping (Section M), what are the natural "migration clusters"? (apps that MUST move together) | Very High | Cluster size determines wave complexity and effort |
| N1.2 | What is the size of each cluster? (number of apps, servers, databases) | High | Large clusters = long cutover windows = more risk |
| N1.3 | Are there any apps that can move INDEPENDENTLY with zero dependencies? (standalone) | High | Quick wins — move first to build confidence |
| N1.4 | Are there "bridge" apps that connect two clusters? (gateway between two groups) | High | Bridge apps may need to run in hybrid mode longest |
| N1.5 | What is the maximum cluster size you can migrate in a single cutover window? | High | Practical constraint on wave sizing |

### N2 — Critical Path & Blockers

| # | Question | Cost Impact | Why It Matters |
|---|----------|-------------|----------------|
| N2.1 | **Which apps BLOCK other apps from migrating?** (must-move-first dependencies) | Very High | These are on the critical path — any delay cascades |
| N2.2 | Is Active Directory / Identity a blocker? (must be available on AWS before apps can move) | Very High | AD/Identity is often Wave 0 prerequisite |
| N2.3 | Is the integration platform / ESB a blocker? (must move or bridge before dependent apps) | High | MuleSoft/SAP PI migration often gates everything |
| N2.4 | Is shared storage a blocker? (must be available on AWS before apps can access files) | High | FSx/EFS provisioning is a prerequisite |
| N2.5 | Are there external constraints on migration order? (vendor maintenance windows, contract dates, compliance deadlines) | High | Hard constraints override technical sequencing |
| N2.6 | What is the longest lead-time item? (Direct Connect provisioning, license procurement, vendor approval) | High | Start these NOW regardless of wave |

### N3 — Sequencing Logic

| # | Question | Cost Impact | Why It Matters |
|---|----------|-------------|----------------|
| N3.1 | What is the proposed wave order? (Wave 1 → 2 → 3 → ...) | — | Master plan |
| N3.2 | What is the duration of each wave? (weeks/months) | High | Total program timeline and parallel-run cost |
| N3.3 | What is the cutover window for each wave? (weekend, off-hours, holiday period) | Medium | Determines overtime / contractor cost |
| N3.4 | How long will the "hybrid state" last? (some apps on-prem, some on AWS) | Very High | Hybrid state = dual infrastructure cost + bridge maintenance |
| N3.5 | What is the cost of running hybrid? (DirectConnect + bridge apps + dual monitoring) | Very High | Often overlooked — can be $10K-$50K+/month |
| N3.6 | What is the rollback strategy per wave? (can you roll back one wave without affecting others?) | High | Isolation between waves reduces risk |
| N3.7 | Are there any "big bang" migrations required? (all-or-nothing, no incremental option) | Very High | Big bang = highest risk + highest parallel-run cost |

### Wave Sequencing Decision Framework

```
WAVE 0 (Foundation — Month 1-2):
├── Landing Zone / Account structure
├── Direct Connect / VPN connectivity
├── Active Directory on AWS (Managed AD or AD Connector)
├── Shared file services (FSx/EFS) if needed as prerequisite
├── Monitoring / logging infrastructure
└── CI/CD pipelines for AWS

WAVE 1 (Quick Wins — Low dependency, standalone apps):
├── Independent apps with NO shared DB/cache/files
├── Low business criticality
├── Small team can migrate with minimal coordination
└── Purpose: build muscle memory, prove patterns

WAVE 2-3 (Core — Medium clusters):
├── Apps with moderate dependencies (5-15 integrations)
├── Shared cache / shared file systems
├── Business-important but not business-critical
└── Purpose: migrate bulk of portfolio

WAVE 4 (Critical — Large tightly-coupled clusters):
├── ERP / Finance / Core business systems
├── SAP + all SAP-connected apps
├── Highest dependency count
├── Longest cutover window needed
└── Purpose: final business-critical migrations

WAVE 5 (Cleanup):
├── Decommission on-prem infrastructure
├── Cancel MSP contracts
├── Terminate Direct Connect (if no longer needed)
└── Cost optimization (Savings Plans, right-sizing)
```

---

## O — Business Process Criticality (Independent of Technical Footprint)

> *A technically simple, low-cost app can be the most business-critical system in the company. App criticality ≠ infrastructure size. This section captures BUSINESS PROCESS dependency that determines true RTO/RPO and migration priority.*

| # | Question | Cost Impact | Why It Matters |
|---|----------|-------------|----------------|
| O1 | **What business process does this application support?** (e.g., period-end close, payroll, order fulfillment, regulatory reporting) | Very High | Small app + critical process = high migration priority |
| O2 | What is the revenue impact if this business process stops? ($/hour, $/day) | Very High | Drives investment in HA architecture |
| O3 | What is the regulatory impact if this process fails? (fines, license revocation, audit failure) | Very High | Compliance deadline may override technical sequencing |
| O4 | Is this app a GATE for a business process? (nothing downstream can proceed without it) | Very High | Gate apps need highest availability + priority migration |
| O5 | What is the business process RTO? (not the app RTO — the PROCESS recovery time) | Very High | Process RTO may be tighter than individual app RTO |
| O6 | Are there seasonal/calendar-critical periods? (year-end close, tax filing, open enrollment, Black Friday) | High | Migration CANNOT happen during these windows |
| O7 | What manual workarounds exist if this app is unavailable? | Medium | Workaround = buys time; no workaround = zero tolerance for downtime |
| O8 | How many people / departments are blocked if this app is down? | High | Wide impact = higher business criticality regardless of app size |
| O9 | Is this app on the critical path for financial reporting? (SOX implications) | Very High | SOX = audit trail must be continuous during migration |
| O10 | Does this app participate in a time-sensitive chain? (e.g., nightly batch → report by 6am → board meeting) | High | Timing chain breaks during migration = business impact |
| O11 | What is the business owner's risk tolerance for this migration? (zero risk vs acceptable short outage) | High | Shapes architecture cost (hot standby vs restore from backup) |
| O12 | Is there a contractual SLA with EXTERNAL customers tied to this app? | Very High | Customer-facing SLA breach = penalties, reputation |
| O13 | Rate this app: BUSINESS CRITICALITY independent of cost/size (Critical / High / Medium / Low) | — | May differ significantly from technical criticality |
| O14 | If this app is "Low cost" but "Critical business process" — should it be in an earlier wave? | High | Resequencing decision |

### Business Criticality vs. Technical Complexity Matrix

```
                        TECHNICAL COMPLEXITY
                    Low              High
                ┌────────────────┬────────────────┐
    High        │ PRIORITY 1     │ PRIORITY 2     │
BUSINESS        │ Small but      │ Large & critical│
CRITICALITY     │ critical —     │ — needs most   │
                │ migrate early  │ planning & HA   │
                │ (quick win     │ (Wave 3-4,     │
                │ + high value)  │ highest effort) │
                ├────────────────┼────────────────┤
    Low         │ PRIORITY 3     │ PRIORITY 4     │
                │ Easy to move,  │ Complex but    │
                │ low urgency —  │ low impact —   │
                │ batch in any   │ consider Retire │
                │ wave           │ or defer       │
                └────────────────┴────────────────┘
```

---

## P — Testing Environment Lifecycle & Data Refresh

> *Non-prod environments are often 40-60% of total AWS cost. How they're provisioned, refreshed, and decommissioned directly affects cost. Data refresh from prod involves masking, tooling, and timing that are often unbudgeted.*

### P1 — Environment Strategy & Lifecycle

| # | Question | Cost Impact | Why It Matters |
|---|----------|-------------|----------------|
| P1.1 | What non-prod environments are required? (Dev, SIT, UAT, Pre-Prod, Performance/Load Test, Training, Sandbox, DR Test) | Very High | Each environment = cost multiplier; 5 envs = 5x base cost |
| P1.2 | Which environments are permanent (always-on) vs. transient (spun up on demand)? | Very High | Transient = 70-90% cheaper if well automated |
| P1.3 | Can performance/load testing environments be EPHEMERAL? (created before test, destroyed after) | Very High | Ephemeral perf env saves $10K-$50K/month for large apps |
| P1.4 | What is the sizing of each non-prod environment relative to prod? (50%, 75%, 100%, variable) | High | Perf test MUST be prod-like = 100% size; Dev can be 25% |
| P1.5 | How many concurrent performance tests run per month? Duration of each? | High | Determines if environment is permanent or on-demand |
| P1.6 | Is there a shared performance testing environment across multiple apps? | Medium | Shared = lower cost per app but scheduling conflicts |
| P1.7 | What is the environment provisioning lead time today? (hours, days, weeks) | Medium | Long lead time = pressure to keep environments always-on = waste |
| P1.8 | Is there IaC to spin up/tear down environments? (Terraform, CDK, CloudFormation) | Very High | No IaC = environments stay running permanently = cost |

### P2 — Data Refresh from Production

| # | Question | Cost Impact | Why It Matters |
|---|----------|-------------|----------------|
| P2.1 | **How often is non-prod data refreshed from production?** (daily, weekly, monthly, quarterly, never) | High | Frequent refresh = ongoing compute + storage + masking cost |
| P2.2 | What is the production data size that gets copied to non-prod? | High | 500GB refresh daily = significant I/O + storage cost |
| P2.3 | **Is data masking / anonymization required during refresh?** (PII, PHI, PCI data must be masked) | Very High | Masking tooling = license cost + compute time + effort |
| P2.4 | What masking tool is used or planned? (Delphix, Informatica TDM, custom scripts, AWS DMS transformation, Oracle Data Masking) | High | Tool licensing can be $50K-$200K/year; or custom = development effort |
| P2.5 | How long does a data refresh take today? (minutes, hours, days) | Medium | Long refresh = environment unavailable = developer idle time cost |
| P2.6 | Is the refresh automated or manual? | Medium | Manual = people cost per refresh cycle |
| P2.7 | Are there referential integrity issues when masking? (masked FK must match masked PK) | Medium | Complex masking = more effort, more tooling, more cost |
| P2.8 | Can you use database snapshots for refresh instead of full copy? (RDS snapshot restore, Aurora clone) | High | Aurora clone = seconds, near-zero cost; snapshot restore = faster than dump/reload |
| P2.9 | Is there synthetic data generation as an alternative to prod refresh? | Medium | Synthetic = no masking needed but may not represent realistic workloads |
| P2.10 | What are the compliance requirements for non-prod data? (must PII be masked? audit trail for who accessed unmasked data?) | High | Regulated industries may mandate masking = non-negotiable cost |

### P3 — Performance & Load Testing Specifics

| # | Question | Cost Impact | Why It Matters |
|---|----------|-------------|----------------|
| P3.1 | Is there a dedicated performance testing environment? Or shared with UAT? | High | Dedicated = permanent cost; shared = scheduling conflicts |
| P3.2 | What load testing tool is used? (JMeter, Gatling, k6, LoadRunner, NeoLoad, Locust) | Medium | LoadRunner/NeoLoad = expensive license; JMeter/k6 = free |
| P3.3 | Does perf testing require production-scale data volume? | Very High | Full prod data in perf env = storage cost = prod-size environment |
| P3.4 | Does perf testing require production-scale compute? (same instance types as prod) | Very High | Perf env at prod scale = doubling compute cost during test windows |
| P3.5 | How frequently are performance tests run? (every sprint, monthly, quarterly, pre-release only) | High | Frequency × duration × environment size = ongoing cost |
| P3.6 | What is the typical performance test duration? (hours, days, weeks of soak testing) | High | Week-long soak test at prod scale = significant cost |
| P3.7 | Can performance testing use spot instances? (if test can tolerate interruption) | Medium | Spot = 70-90% savings for perf test compute |
| P3.8 | Is chaos engineering / resilience testing planned? (requires prod-like environment) | Medium | Additional environment time or dedicated resilience env |

### Environment Cost Optimization Strategies

| Strategy | Savings | Requirement |
|----------|---------|-------------|
| Schedule non-prod (stop nights/weekends) | 65-70% | Automation (Lambda + EventBridge or Instance Scheduler) |
| Ephemeral perf test environments | 80-90% | IaC (Terraform/CDK) to create/destroy |
| Aurora clones for data refresh | 90%+ vs full copy | Aurora database (clone is instant + storage-only cost) |
| Smaller non-prod instances | 30-60% | Right-size (Dev = t3.medium vs Prod = m6i.2xlarge) |
| Shared environments across teams | 40-50% | Scheduling and namespace isolation |
| Spot instances for testing | 70-90% | Fault-tolerant test workloads |
| Synthetic data instead of prod refresh | Variable | Eliminates masking cost; may not be acceptable for UAT |

---

## ⚠️ Common Cost Traps — Don't Miss These

| Trap | Impact | Question to Ask |
|------|--------|-----------------|
| **Data transfer egress** | Can be 20-30% of bill | G1: How much data leaves AWS monthly? |
| **NAT Gateway charges** | $0.045/GB processed | G8: How much traffic goes through NAT? |
| **Multi-AZ database** | 2x DB cost | E4: Is Multi-AZ required? |
| **Oracle licensing on AWS** | Can 10x DB cost | H2: What Oracle licensing model? |
| **Always-on non-prod** | 40-60% waste | E8: Can non-prod be scheduled? |
| **Overprovisioned instances** | 30-50% waste | F1: What is actual utilization vs allocated? |
| **No Savings Plans** | Paying 40-60% more | E10: What commitment level? |
| **Forgotten DR environment** | 50-100% cost increase | E5: Do you need multi-region DR? |
| **Log/metric retention** | Grows silently | E14: What retention for logs and metrics? |
| **Parallel run period** | Temporary 2x cost | K5-K6: How long will both environments run? |
| **SI partner cost** | $1.5-3K/day/person | K3: Is external help needed? |
| **Training & certification** | $5-10K per person | K10: What AWS training needed? |
| **Vendor doesn't support AWS** | Entire migration blocked | D1: Does vendor certify AWS deployment? |
| **SaaS vendor cloud lock-in** | Cannot move to AWS | C9: Can you contractually request AWS hosting? |
| **COTS licensing change for cloud** | 2-5x cost increase | D6: Does vendor charge differently for cloud? |
| **On-prem Active Directory dependency** | $144-$580/month + DX latency | L2.2: Does app need domain-joined servers? |
| **Shared file drives (SMB/NFS)** | FSx = $0.13-$0.25/GB/month | L3.1: Are there shared drives apps depend on? |
| **Database transaction log consumers** | Extra replicas + compute | L4.1: Are logs used by other systems? |
| **Redis/Cache Enterprise licensing** | $50K-$200K/year on top of infra | L5.6: Is cache separately licensed? |
| **Data sovereignty — single region** | No multi-region DR possible | L1.6: Must app operate in one region only? |
| **PII/PHI compliance controls** | Encryption + audit + DLP = 20-40% more | L1.2-L1.3: Is there regulated data? |
| **In-memory DB (HANA, VoltDB)** | Memory-optimized instances = 2-3x cost | L5.12: In-memory database dependency? |
| **Per-core licensing explosion** | Oracle/SAP on large instances = 5-10x | L6.10: Per-core licenses that multiply? |
| **Private CA requirement** | $400/month per CA | L6.6: Is there a PKI/CA requirement? |
| **Shared DB forces co-migration** | Delayed timeline, larger waves | M1.1: Which apps share same DB? |
| **Hybrid state during migration** | $10K-$50K+/month dual infra | N3.4-N3.5: How long is hybrid state? |
| **"Small" app gates critical process** | Underinvested HA = process failure | O4: Is app a gate for a business process? |
| **Always-on perf test environment** | Prod-scale compute running 24/7 | P1.3: Can perf env be ephemeral? |
| **Prod data refresh without masking tool** | Manual effort + compliance risk | P2.3-P2.4: Is masking required? What tool? |
| **Non-prod environments never stopped** | 40-60% wasted spend | P1.2: Which envs are permanent vs transient? |

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
- [ ] Compliance & regulatory requirements documented (GDPR, HIPAA, PCI, SOX, data residency)
- [ ] PII/PHI/PCI data identified — encryption and DLP cost factored
- [ ] Data residency / single-region mandate confirmed
- [ ] Active Directory dependency assessed (Managed AD, AD Connector, or cloud-native)
- [ ] SSO / identity architecture decided (IAM Identity Center, federation, Managed AD)
- [ ] On-prem shared drive dependencies mapped (FSx vs EFS vs S3)
- [ ] Database transaction log / replication consumers identified
- [ ] Cache / in-memory dependencies documented (ElastiCache sizing, licensing)
- [ ] Supporting service licenses inventoried (middleware, APM, backup, WAF, scheduler)
- [ ] CI/CD current state and target state documented
- [ ] IaC requirement (yes/no, tool choice)
- [ ] Team composition for migration (internal + external)
- [ ] Estimated migration duration per phase
- [ ] Parallel-run duration
- [ ] Ongoing operational FTE estimate
- [ ] Training requirements identified
- [ ] Cross-app dependency map created (shared DB, cache, files, ESB)
- [ ] Migration clusters identified (apps that must move together)
- [ ] Critical path / blocker apps identified (Wave 0 prerequisites)
- [ ] Hybrid state duration and cost estimated
- [ ] Wave sequence draft with timeline
- [ ] Business process criticality rated (independent of app size/cost)
- [ ] Calendar-critical / blackout periods identified
- [ ] Gate apps identified (small apps that block critical processes)
- [ ] Non-prod environment lifecycle strategy (permanent vs ephemeral)
- [ ] Data refresh cadence and masking requirements documented
- [ ] Performance test environment sizing and frequency captured
- [ ] Data masking tool identified (or build vs buy decision)

---

**Author:** Ramandeep Chandna | August 2026
