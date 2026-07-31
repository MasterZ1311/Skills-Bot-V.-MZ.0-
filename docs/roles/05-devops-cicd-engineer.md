# 🔧 DevOps / CI-CD Engineer — Skills Guide

DevOps engineers automate the software delivery pipeline, manage infrastructure, and ensure systems are reliable, observable, and secure. This guide covers CI/CD, containers, orchestration, IaC, monitoring, and Git workflows.

---

## 🗺️ Skill Map at a Glance

| Concern | Top Skills |
|---|---|
| CI/CD | `@github-actions-advanced`, `@gitlab-ci-patterns`, `@circleci-automation` |
| Containers | `@docker-expert`, `@kubernetes-architect`, `@k8s-manifest-generator` |
| Service Mesh | `@istio-traffic-management`, `@linkerd-patterns`, `@service-mesh-expert` |
| IaC | `@terraform-specialist`, `@aws-cdk-development`, `@cloudformation-best-practices` |
| Git Workflows | `@git-advanced-workflows`, `@git-hooks-automation`, `@gitops-workflow` |
| Monitoring | `@prometheus-configuration`, `@grafana-dashboards`, `@datadog-automation` |
| Secrets | `@secrets-management`, `@devops-deploy` |
| Deployment | `@deployment-pipeline-design`, `@deployment-procedures`, `@appdeploy` |

---

## 🔄 CI/CD

### `@github-actions-advanced`
Complex GitHub Actions workflows — matrix builds, reusable workflows, composite actions.
```
@github-actions-advanced Create a production-grade CI/CD workflow for our Next.js app:
- Trigger: PR (run tests), push to main (deploy staging), release tag (deploy prod)
- Jobs: lint, type-check, unit-test, e2e-test, build, deploy
- Matrix: test on Node 20 and 22
- Reusable workflow for the test job
- Caching: node_modules, Next.js build cache
- Secrets: VERCEL_TOKEN, DATABASE_URL
```

### `@github-actions-templates`
Pre-built GitHub Actions templates for common workflows.
```
@github-actions-templates Generate workflows for:
1. Dependency security scanning (Dependabot + Snyk)
2. Automatic changelog generation on release
3. PR labeler based on changed files
4. Deploy preview URL comment on PRs
```

### `@github-actions-debugger`
Diagnosing broken GitHub Actions workflows.
```
@github-actions-debugger This workflow is failing with "Error: ENOENT".
[paste workflow YAML]
[paste error log]
Diagnose: working directory issues, missing files, path problems.
```

### `@gitlab-ci-patterns`
GitLab CI/CD pipelines, stages, environments.
```
@gitlab-ci-patterns Create a GitLab CI pipeline for our API:
- Stages: test, build, deploy-staging, deploy-production
- Docker image caching with GitLab Container Registry
- Environment-specific variables
- Manual approval gate for production
- Artifact passing between stages
```

### `@gitops-workflow`
GitOps with ArgoCD or Flux — declarative deployment.
```
@gitops-workflow Set up GitOps for our Kubernetes cluster:
- ArgoCD installation and configuration
- App of Apps pattern for multiple services
- Sync policy: auto-sync with self-heal
- Notifications on sync failure
- Rollback procedure
```

---

## 🐳 Containers

### `@docker-expert`
Dockerfile best practices, multi-stage builds, docker-compose.
```
@docker-expert Write a production Dockerfile for our Node.js API:
- Multi-stage: builder + runtime
- Non-root user
- Layer caching optimization
- Health check
- .dockerignore optimization
- Final image < 100MB
Also write docker-compose.yml for local dev with hot reload.
```

### `@kubernetes-architect`
Kubernetes architecture: namespaces, RBAC, resource limits, networking.
```
@kubernetes-architect Design the Kubernetes setup for our events platform:
- Namespaces: events-prod, events-staging, events-dev
- RBAC: developer access (read), CI (deploy), admin (full)
- Network policies: API can reach DB, UI can reach API only
- Resource limits and requests for each service
- HPA (Horizontal Pod Autoscaler) for API service
```

### `@k8s-manifest-generator`
Generate Kubernetes manifests: Deployment, Service, Ingress, ConfigMap, Secret.
```
@k8s-manifest-generator Generate K8s manifests for our events API:
- Deployment: 3 replicas, rolling update, liveness/readiness probes
- Service: ClusterIP
- Ingress: NGINX with TLS, routing /api/* to this service
- ConfigMap: non-sensitive env vars
- Secret: database URL (base64 encoded)
- HPA: scale 3-10 pods based on CPU > 70%
```

### `@helm-chart-scaffolding`
Helm chart creation and management.
```
@helm-chart-scaffolding Create a Helm chart for our events API:
- Templates: deployment, service, ingress, hpa, configmap
- values.yaml: image tag, replica count, ingress host, resources
- Environment-specific values files: values-staging.yaml, values-prod.yaml
- Helper templates for labels and selectors
```

---

## 🕸️ Service Mesh

### `@istio-traffic-management`
Istio for traffic management, mTLS, observability.
```
@istio-traffic-management Set up Istio for our microservices:
- mTLS between all services
- Traffic splitting: 90% stable, 10% canary for new registration service
- Circuit breaker for payment service
- Retries and timeouts per route
- Distributed tracing with Jaeger
```

### `@linkerd-patterns`
Linkerd as a lighter-weight alternative to Istio.
```
@linkerd-patterns Add Linkerd to our Kubernetes cluster:
- Inject Linkerd proxy into events namespace
- Golden signals dashboard
- mTLS auto-configured
- Multi-cluster traffic for disaster recovery
```

---

## 🏗️ Infrastructure as Code

### `@terraform-specialist` / `@terraform-infrastructure`
Terraform for cloud resources.
```
@terraform-specialist Write Terraform for our AWS infrastructure:
- VPC with public and private subnets
- ECS Fargate cluster for our API
- RDS PostgreSQL (Multi-AZ for prod)
- ElastiCache Redis for sessions
- Application Load Balancer with ACM certificate
- Route53 records
State: S3 backend with DynamoDB locking.
```

### `@aws-cdk-development` / `@cdk-patterns`
AWS CDK for TypeScript infrastructure.
```
@aws-cdk-development Create CDK stacks for our events platform:
- NetworkStack: VPC, subnets, security groups
- DatabaseStack: RDS Aurora Serverless v2
- ComputeStack: ECS Fargate service, ALB
- MonitoringStack: CloudWatch dashboards, alarms
Use CDK best practices: separate stacks, environment-specific configs.
```

### `@cloudformation-best-practices`
CloudFormation templates.
```
@cloudformation-best-practices Create a CloudFormation template for:
- EC2 Auto Scaling Group with launch template
- Application Load Balancer
- RDS read replica
- CloudFront distribution in front of ALB
```

---

## 📊 Monitoring & Observability

### `@prometheus-configuration`
Prometheus setup, scrape configs, recording rules, alerting rules.
```
@prometheus-configuration Configure Prometheus for our events platform:
- Scrape configs for: Node.js API, PostgreSQL exporter, Redis exporter
- Recording rules: request rate, error rate, p99 latency
- Alerting rules: API error rate > 1%, DB connections > 80%, memory > 85%
- AlertManager: route alerts to Slack (warning) and PagerDuty (critical)
```

### `@grafana-dashboards`
Grafana dashboard design for application and infrastructure metrics.
```
@grafana-dashboards Create dashboards for our events platform:
Dashboard 1: API Overview
- Request rate (req/s)
- Error rate (%)
- p50/p95/p99 latency
- Active connections
Dashboard 2: Business Metrics
- Registrations per hour
- Revenue per day
- Events with capacity alerts
```

### `@datadog-automation`
Datadog monitors, dashboards, APM.
```
@datadog-automation Set up Datadog APM for our Node.js API:
- dd-trace auto-instrumentation
- Custom spans for registration flow
- Monitor: p99 latency > 2s → alert
- Dashboard: service map with dependencies
- Log correlation with traces
```

### `@distributed-tracing`
OpenTelemetry tracing across services.
```
@distributed-tracing Implement distributed tracing for our microservices:
- OpenTelemetry SDK in Node.js API
- Context propagation through HTTP headers
- Spans for: DB queries, external API calls, queue operations
- Export to Jaeger / Tempo
- Sampling: 100% in dev, 10% in prod
```

---

## 🔗 Complete DevOps Prompt Chain

```
1️⃣  @git-advanced-workflows
    "Set up Git branching strategy: main, develop, feature/*, release/*"

2️⃣  @github-actions-advanced
    "Create CI workflow: lint → test → build → deploy-staging"

3️⃣  @docker-expert
    "Write production Dockerfile and docker-compose for local dev"

4️⃣  @k8s-manifest-generator
    "Generate K8s manifests: Deployment, Service, Ingress, HPA"

5️⃣  @helm-chart-scaffolding
    "Package manifests into Helm chart with environment values"

6️⃣  @terraform-specialist
    "Write Terraform for cloud infrastructure: VPC, ECS, RDS, ALB"

7️⃣  @prometheus-configuration
    "Configure Prometheus scraping + alerting rules"

8️⃣  @grafana-dashboards
    "Create API Overview and Business Metrics dashboards"

9️⃣  @deployment-procedures
    "Document runbook: deploy, rollback, hotfix procedures"

🔟  @secrets-management
    "Audit secrets: rotate DB passwords, set up Vault or AWS Secrets Manager"
```
