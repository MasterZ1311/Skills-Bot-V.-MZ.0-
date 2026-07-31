# ☁️ Cloud Infrastructure Architect — Skills Guide

Cloud architects design, build, and maintain the cloud infrastructure that powers modern applications — across AWS, Azure, GCP, and multi-cloud environments. This guide covers all platform-specific skills, networking, serverless, cost optimization, and IaC.

---

## 🗺️ Skill Map

| Concern | Top Skills |
|---|---|
| AWS Core | `@aws-skills`, `@aws-serverless`, `@aws-cdk-development`, `@aws-sst-development` |
| AWS Cost | `@aws-cost-optimizer`, `@aws-cost-cleanup`, `@cost-optimization` |
| Azure AI | `@azure-ai-projects-py`, `@azure-ai-openai-dotnet`, `@azure-ai-voicelive-py` |
| Azure Storage | `@azure-storage-blob-py`, `@azure-cosmos-py`, `@azure-eventhub-py` |
| Azure Identity | `@azure-identity-py`, `@azure-keyvault-py` |
| Multi-Cloud | `@multi-cloud-architecture`, `@hybrid-cloud-architect`, `@cloud-architect` |
| Serverless | `@aws-serverless`, `@azure-functions`, `@cloudflare-workers-expert`, `@gcp-cloud-run` |
| IaC | `@terraform-specialist`, `@aws-cdk-development`, `@cloudformation-best-practices` |
| Networking | `@network-engineer`, `@hybrid-cloud-networking`, `@mtls-configuration` |

---

## 🟡 AWS Skills

### `@aws-skills` (General)
```
@aws-skills Design a production-ready AWS architecture for our events platform:
- Compute: ECS Fargate (API) + Lambda (async jobs)
- Database: RDS Aurora PostgreSQL (Multi-AZ)
- Cache: ElastiCache Redis
- CDN: CloudFront + S3 for static assets
- Messaging: SQS + SNS for notifications
- Auth: Cognito for user management
Draw the architecture diagram in text and list all services with their roles.
```

### `@aws-serverless` / `@aws-serverless-eda`
```
@aws-serverless Design a serverless event-driven architecture for notifications:
- API Gateway → Lambda (registration handler)
- EventBridge for domain events
- Lambda targets: email (SES), SMS (SNS), push (Pinpoint)
- DynamoDB for notification state
- Step Functions for retry orchestration
```

### `@aws-cdk-development` / `@aws-sst-development`
```
@aws-sst-development Build our events API with SST (Serverless Stack):
- API: API Gateway + Lambda handlers
- Database: RDS with migrations
- Auth: SST Auth with Google OAuth
- Storage: S3 for event images
- Queues: SQS for email jobs
Full TypeScript with SST v3.
```

### `@aws-cost-optimizer` / `@aws-cost-cleanup`
```
@aws-cost-optimizer Analyze our AWS bill and find savings:
Our usage: ECS (2 tasks), RDS db.r5.large, ElastiCache cache.m5.large
Monthly cost: ~$800
Find: right-sizing opportunities, Reserved Instance recommendations,
unused resources, Savings Plans vs Reserved vs On-Demand analysis.
Target: reduce bill by 30%.
```

### `@aws-penetration-testing`
```
@aws-penetration-testing Perform a security assessment of our AWS setup:
- IAM roles with overly broad permissions
- S3 bucket public access exposure
- Security group overly open ports
- Unencrypted EBS volumes
- CloudTrail gaps
Provide findings with severity and remediation steps.
```

---

## 🔵 Azure Skills

Azure has an extensive set of SDK-specific skills. Here's how to use the key ones:

### Azure AI Services
```
@azure-ai-projects-py Set up an Azure AI Project for our events chatbot:
- Azure OpenAI deployment (GPT-4o)
- Azure AI Search for event knowledge base
- Prompt flow for RAG pipeline
- Evaluation metrics setup
- Python SDK integration
```

```
@azure-ai-contentsafety-py Add content moderation to event descriptions:
- Analyze event descriptions for harmful content
- Categories: hate, violence, sexual, self-harm
- Threshold configuration per category
- Action: reject or flag for review
```

```
@azure-ai-document-intelligence-ts Extract data from event flyers:
- Analyze uploaded PDF/image flyers
- Extract: event name, date, time, venue, contact
- Structure as JSON for our database
- Handle multiple formats (PDF, PNG, JPG)
```

### Azure Storage
```
@azure-storage-blob-py Set up Azure Blob Storage for event images:
- Container: events-images (private) and events-thumbnails (public)
- SAS token generation for direct browser upload
- Lifecycle policy: move to cool tier after 30 days
- CDN integration with Azure Front Door
```

```
@azure-cosmos-py Design Cosmos DB schema for our events:
- Container: events (partition key: /categoryId)
- Container: registrations (partition key: /userId)
- Indexing policy for query optimization
- TTL for expired event documents
- Change feed for real-time updates
```

### Azure Messaging
```
@azure-eventhub-py Implement event streaming with Azure Event Hubs:
- Producer: API sends registration events
- Consumer group: notifications, analytics, audit
- Python SDK with async producer/consumer
- Checkpointing with Azure Blob Storage
- Dead letter handling
```

```
@azure-servicebus-py Set up Service Bus for reliable messaging:
- Queue: email-notifications (with DLQ)
- Topic: registration-events with subscriptions per consumer
- Message sessions for ordered processing
- Message lock renewal for long-running consumers
```

### Azure Identity & Security
```
@azure-identity-py Configure Azure Managed Identity:
- System-assigned identity for our App Service
- Grant: Blob Storage Contributor, Key Vault Secrets User
- No credential management in code
- Works in local dev with DefaultAzureCredential
```

```
@azure-keyvault-py Manage secrets with Azure Key Vault:
- Store: database connection string, Stripe API key, JWT secret
- Access policy: App Service identity can read secrets
- Secret rotation: automate with Event Grid + Function
- Audit logging to Log Analytics
```

### Azure Monitoring
```
@azure-monitor-query-py Query Azure Monitor logs for our API:
- Query: error rate by endpoint over last 24h
- Query: p99 latency distribution
- Query: failed authentication attempts (potential attack)
- Set up alerts on KQL query results
```

---

## 🌍 Multi-Cloud & Hybrid

### `@multi-cloud-architecture`
```
@multi-cloud-architecture Design a multi-cloud strategy for our events platform:
- Primary: AWS (API, database)
- Secondary: Azure (AI services, identity)
- CDN: Cloudflare
- DNS failover strategy
- Cost implications
- Operational complexity tradeoffs
When does multi-cloud make sense vs when to stay single-cloud?
```

### `@hybrid-cloud-architect`
```
@hybrid-cloud-architect Design hybrid cloud for our university client:
- On-premise: sensitive student data (FERPA compliance)
- Cloud: event management, public-facing API
- VPN / ExpressRoute connection
- Data synchronization strategy
- Identity federation (AD + Azure AD)
```

---

## ⚡ Serverless

### `@cloudflare-workers-expert`
```
@cloudflare-workers-expert Deploy our event listing API to Cloudflare Workers:
- Edge caching with Cache API
- KV for session storage
- D1 SQLite for event data at edge
- R2 for image storage
- Route: /api/events → Worker, /api/admin → origin
Global performance: serve from 300+ PoPs.
```

### `@gcp-cloud-run`
```
@gcp-cloud-run Deploy our Node.js API to Google Cloud Run:
- Container image from Artifact Registry
- Minimum instances: 1 (avoid cold starts)
- Maximum: 20 instances
- Cloud SQL connection via Cloud SQL Auth Proxy
- Secret Manager for environment variables
- Cloud Armor for DDoS protection
```

---

## 💰 Cost Optimization

### `@cost-optimization`
```
@cost-optimization Review our cloud spend ($2,400/month) and find savings:
- Right-size: EC2, RDS, ElastiCache instances
- Reserved instances vs Savings Plans for predictable workloads
- S3 lifecycle policies (IA, Glacier for old event data)
- Lambda: provisioned concurrency only for critical endpoints
- Data transfer cost reduction
Target: 25% reduction without impacting performance.
```

---

## 🔗 Complete Cloud Architecture Prompt Chain

```
1️⃣  @cloud-architect
    "Design overall architecture: services needed, regions, HA strategy"

2️⃣  @aws-skills (or @azure-ai-projects-py for Azure)
    "Select specific services: compute, database, cache, messaging, auth"

3️⃣  @terraform-specialist
    "Write IaC for all infrastructure: VPC, compute, database, networking"

4️⃣  @aws-serverless (or @azure-functions)
    "Design serverless event-driven components: async jobs, webhooks"

5️⃣  @kubernetes-architect + @k8s-manifest-generator
    "Container orchestration: deploy API and workers"

6️⃣  @prometheus-configuration + @grafana-dashboards
    "Set up full observability: metrics, dashboards, alerts"

7️⃣  @secrets-management + @azure-keyvault-py
    "Secure all credentials: Key Vault / Secrets Manager"

8️⃣  @aws-cost-optimizer
    "Right-size resources, set up Reserved Instances, optimize spend"

9️⃣  @aws-penetration-testing + @cloud-penetration-testing
    "Security audit: IAM, network exposure, encryption gaps"
```
